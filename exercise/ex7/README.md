![Pic 0](images/ex7-0.png)

# Exercise 7 - Building a Micro-Integration to publish files onto your Mesh

SAP Integration Suite, advanced event mesh supports a range of protocols that enable applications to publish events to Event Brokers or consume events from them. 
These protocols include AMQP, MQTT, JMS, HTTP(S) (including REST Delivery Points to push events to REST APIs), Solace Messaging Format (SMF, high-performance binary messaging protocol over TCP), etc.

Advanced Event Mesh comes with automatic protocol conversion, meaning that publishing and subscribing applications do not have to use the same protocol.

![AEM Protocols](images/protocols.png)

Beyond protocol-based messaging, Advanced Event Mesh also offers Micro-Integrations.

<a href="https://help.pubsub.em.services.cloud.sap/Micro-Integrations/Managed/create-micro-integration-details.htm" target="_blank">Micro-Integrations</a> provide **lightweight integration functionality** that connects external systems directly to Advanced Event Mesh, enabling them to participate in Event-Driven data distribution.
These external systems include enterprise applications, LLMs & AI Agents, messaging services, data stores, file servers, etc. 
Depending on the Micro-Integration, data can flow from an external system onto the mesh or from the mesh to an external target.

**Lightweight** is a key characteristic of Micro-Integrations.
They are particularly well suited to focused and high-volume integration scenarios that require little to no transformation or processing, such as event streaming.

![Micro-Integrations Overview](images/cloud-mi-overview.png)

In this exercise, we will explore Micro-Integrations by building a flow that publishes files from an SFTP server to topics and queues on your Event Broker. 
Whenever a new CSV file is placed on the SFTP server, each line will be published as a separate JSON event to the mesh, ready for subscribers to consume at their own pace.

After completing these steps, you will be familiar with the structure, configuration and deployment of Micro-Integrations in SAP Integration Suite, advanced event mesh.

Before diving into the hands-on part, let's discuss some of the potential use cases.

## 7.1 - Micro-Integration Use Cases

In this section, we will outline (potential) use cases for Micro-Integrations on SAP Integration Suite, advanced event mesh. 

Note, that the use cases we will discuss will often involve one specific source or target system. 
In reality, a source system will be posting events on the event broker and multiple applications will consume these events. 
This means that Source Micro-Integrations (MIs that enable source systems to publish events on the mesh) will often publish on a topic where multiple queues are subscribed to.
Target Micro-Integrations can be one of the receivers as they will often listen to a specific queue and publish to a specific target system.

## 7.1.1 - Non-SAP Enterprise Applications as publisher or subscriber

Not all enterprise applications can be configured to publish or consume events directly. Some applications work natively with events but only expose them through their own publish/subscribe APIs. In that case, you still need some lightweight logic that subscribes to these APIs and forwards the events to the event broker, or that takes events from the event broker and publishes them through the application's API.

Looking at the non-SAP world, Salesforce is a great example of this. 
Its Pub/Sub API lets you subscribe to Salesforce events (such as Change Data Capture events and Platform Events) and publish Platform Events back into Salesforce. 
So, you may be wondering:
- "How do we get Salesforce events published to our topics and queues in AEM?"
- "How do we publish events from AEM back to Salesforce through this Pub/Sub API?"

Well, you probably guessed it by now: the answer is Micro-Integrations.

AEM offers a Salesforce Micro-Integration that leverages the Salesforce Pub/Sub API and works in both directions: you can use it to get Salesforce events onto the topics and/or queues you specify, or to send events from AEM back to Salesforce as a custom Platform Event.

Below, you can see an example of what such a Micro-Integration looks like. In this example, the Micro-Integration subscribes to Salesforce events through the Pub/Sub API. Whenever an event occurs (e.g., an Account change event), the Micro-Integration publishes it to a topic or queue on the event broker service named "us1".

![Salesforce Source Micro-Integration](images/SFSource.png)

![Salesforce Source Micro-Integration](images/SFSource2.png)

In case: 
1. one of your applications cannot natively receive events
2. there is no Micro-Integration available, but it natively offers public REST APIs
3. you don't need to transform the event

You can also leverage <a href="https://help.pubsub.em.services.cloud.sap/API/REST/REST-Consumers.htm#rest-delivery-point" target="_blank">REST Delivery Points</a> (RDP's) in AEM. 
An RDP delivers the events that land in a queue to a REST endpoint as HTTP requests, serving as an outbound webhook. The endpoint only needs to be reachable from the event broker service. For the opposite direction, any application that can send HTTP requests can publish events directly to the event broker using its REST messaging interface.

## 7.1.2 - Content-Based Topic Destinations and Lightweight Transformations

One of the strengths of Advanced Event Mesh is the fine-grained and flexible filtering using wildcards in topic subscriptions.
This makes the platform very flexible, as you it allows a single subscription to attract events published on many different topics.

For example: 
- **salesorder/created/>** matches all sales order created events, no matter how many levels follow the verb.
- **businesspartner/*/de/>** matches all events on German business partners, regardless of whether the business partner was created, updated, etc.

A well-designed topic typically follows a noun/verb/properties structure, and defining a reusable topic taxonomy is key when setting up event-driven architectures.

However, an application cannot always control the topic it publishes its events on. Take Salesforce again: its event channels are defined by the Salesforce Pub/Sub APIs, so most likely it is not following your exact topic taxonomy.

This is where the Content-Based Topic Destinations (also referred to as Smart Topic Destinations in AEM) come in. 
They allow you to combine fixed values (e.g., salesorder/created) with fields from the payload or headers of the incoming event (e.g., region, country, or supplier ID), so that each event is published on the right topic that follows __your__ topic taxonomy

You define a Smart Topic Destination in the Mappings step of a Micro-Integration. Below is an example of the one we will build later in this chapter.

![Smart Topic Destination](images/smartTopic.png)

Micro-Integrations also come with the option to perform lightweight transformations when events are coming in or going out.
For example, sometimes you may want to remove or map one or more fields from the payload before it's published on topics or in queues or before you send it to a specific target, this is where the lightweight transformation options of the Micro-Integrations come in.

## 7.1.3 - Event Streaming

Event streaming refers to a continuous, real-time flow of events, often at high volumes. This is common in IoT scenarios: sensors capturing the vibrations of machines or engines, for example, can generate thousands of events every second. These events are often streamed directly to databases, data lakes, or data warehouses. This doesn't require complex integration logic, but it does require moving large volumes of events reliably. AEM includes Micro-Integrations for several data stores, such as MongoDB and PostgreSQL, that you can use for this purpose. 
A Micro-Integration for SAP HANA Cloud is also planned, which will allow you to stream events into SAP HANA Cloud or SAP Business Data Cloud.

![Event Streaming](images/eventStreaming.png)

## 7.1.4 - Change Data Capture

We previously discussed how Micro-Integrations can push events directly into a data store, but the other way around is also possible. With Change Data Capture (CDC), a Micro-Integration listens for changes in a database (inserts, updates, and deletes) and, whenever a change occurs, publishes it as an event to a topic on the mesh. AEM comes with CDC Micro-Integrations for various databases, such as Oracle, Microsoft SQL Server, IBM Db2, MySQL, PostgreSQL, and MongoDB. They capture changes from the database's transaction log instead of repeatedly querying its tables, so the source application doesn't need to be changed. 
A CDC Micro-Integration for SAP HANA is also planned, which will allow you to publish changes in SAP HANA/BDC as events to AEM.

![Change Data Capture](images/cdc.png)


## 7.1.5 - LLM Processing

An event is often a good input for a large language model (LLM), for example to classify a support ticket, summarize a document, or extract structured data from free text. The LLM's response can then be captured as a new event, which in turn serves as input for other applications. 
You can use a processor Micro-Integration for this, it forwards events from the event broker to an LLM, and publishes the LLM's response back to the event broker, ready to be consumed by other systems.

![Change Data Capture](images/llmMI.png)

## 7.1.6 - File-to-Mesh

Another typical use case for Micro-Integrations is File-to-Mesh: a source Micro-Integration picks up files from a file server (e.g., an SFTP server) and publishes their contents to the mesh, making them available for event-driven distribution to all interested applications. Because many legacy applications still rely on SFTP to exchange data, this is a common way to integrate them into an event-driven architecture (often referred to as liberating legacy applications). The reverse is also possible: a target Micro-Integration can write events as files to a file server.

In the exercise in this chapter, you will build a File-to-Mesh scenario that also uses Content-Based Topic Destinations.

![Change Data Capture](images/fileToMesh.png)

## 7.2 - Exploring Micro-Integrations

Now, that you have a better understanding of potential use cases, let's have a look at the Micro-Integrations that are currently available in SAP Integration Suite, advanced event mesh:
1. In the Advanced Event Mesh Cloud Console, scroll down to the section "**Micro-Integrations**" and click on the tile "**Micro-Integrations: Create and manage Micro-Integrations**".

![Cloud Console to Micro-Integrations](images/MI1.png)

2. This takes you to an overview of all Micro-Integrations that are configured in your environment. You can also see the deployment/error status of each MI (e.g. Not Deployed, Deployed, Down, etc.).
3. If you want to see the list of all Micro-Integrations that can be configured on Advanced Event Mesh, click on "**Create Micro-Integrations**".

![Create Micro-Integration](images/ExploreMI1.png)

4. This opens a list of all available Micro-Integrations, as you can see, they provide integrations with a range of systems and services, including:
* **Databases and Data Warehouses**: 
PostgreSQL, MySQL, Oracle DB, MongoDB, IBM DB2, and others.
* **File and Object Stores**: 
SFTP, Amazon S3, Azure Blob Storage, and others.
* **Enterprise Applications**: 
Salesforce, for example.
* **Messaging Services**: 
Amazon SNS, Amazon SQS, Azure Service Bus, Google Pub/Sub, IBM MQ, and others.

You will see three different types of Micro-Integrations:
* With a **Target MI**, data streams _from an advanced event mesh event broker service_ to the external system.
* With a **Source MI**, data streams _from the external system_ to an advanced event mesh event broker service.
* With a **Processor MI**, data streams from an advanced event mesh event broker service, through a transformation, and back to that same event broker service.

![Available Micro-Integrations](images/MI_list.png)

Selecting a Micro-Integration from this list would start the process of creating a new instance of that Micro-Integration. **We are not going to create a Micro-Integration for this exercise**. Instead, we will create a flow (explained later in the exercise) within an existing MI. 

For now, take a moment to familiarise yourself with the available Micro-Integrations before moving on to the next part of the exercise.
You can also find a list of all available Micro-Integrations <a href="https://help.pubsub.em.services.cloud.sap/Micro-Integrations/Managed/create-micro-integration-details.htm" target="_blank">here</a>.


## 7.3 - Getting familiar with the structure of a Micro-Integration

1. Return to the overview of all configured **Micro-Integrations**.

![Cloud Console to Micro-Integrations](images/MI1.png)

2. Open your assigned SFTP Source Micro-Integration.

![Open SFTP MI](images/openSFTPMI.png)

3. At the top of the page, you see the state of your Micro-Integration. Below it, the page provides an overview of the source and target connection details on the left and the configured flows on the right. Under the connection details, click "**View Details**".

![MI Details](images/MI_overview1.png)

4. Here, you can inspect the connection configuration for your Micro-Integration. Each Micro-Integration (no matter the type) has a **Source Connection** and **Target Connection**. These contain the connection parameters required to access the relevant systems, including the event broker and, where applicable, an external system. Depending on the connection, these parameters may include server URL, port, and authentication credentials. Connection details are configured at the Micro-Integration level, and are shared by all of its flows.
For this SFTP source Micro-Integration, the connections are configured as follows:
* **Source Connection**: Contains the configuration of the checkpoint queue (aka Last Value Queue, LVQ). You want to make sure that when a file gets reuploaded that only the added lines in the CSV get processed as new events, this is where the checkpoint queue comes in. It is a specialised queue used to keep track of the current progress of file processing. You can create the queue beforehand (using Event Portal or Broker Manager), or have it created automatically when you deploy the Micro-Integrations.
* **Target Connection**: Contains the configuration for the broker to which the events (individual CSV lines) will be published. For this MI, it is also the broker on which the Micro-Integration is deployed and the checkpoint queue is created.

**TIP**: Write down the name of your broker, we will need it to test our Micro-Integration

![MI Connection Details](images/MI_connectionDetails.png)

5. Go back to your assigned SFTP Source Micro-Integration.

![Return to MI](images/back_to_MI_from_connection.png)

6. As mentioned before, on the right side of the page, you can see the flows. A flow defines a single data path through the Micro-Integration. 
Eeach flow specifies:
* source and target endpoints, such as queue, topic endpoint, or vendor-specific destination (e.g. SFTP Server URI, credentials and Delimited File Configuration)
* optionally, error handling, including dead message queue routing and retry settings
* optionally, header and payload mappings
The following diagram shows an example of a source Micro-Integration that contains two flows. The flows share the source and target connection configurations; the flows consist of individual endpoints and mappings (the diagram does not show error configuration).

![Micro-Integrations Flow Architecture](images/mi-component-architecture-cloud.png)

Every Micro-Integration comes with five flows. So for the example of the SFTP Source Micro-integration, these flows can listen to their own SFTP server or directory/folder and map different objects, for example:
* 5 flows that listen to the same SFTP server but each to a different directory. One listens to the directory /salesorders and maps sales order objects, another one that listens to /businesspartners and maps business partner objects, etc.: _in this case you require 1 Micro-Integration_.
* 5 flows that each listen to a different SFTP server and all map and publish sales order objects: _in this case you require 1 Micro-Integration_.
* 10 flows that listen to the same SFTP server, but map different objects: _in this case you require 2 Micro-Integrations_

Let's see flows in action! In the next part of the exercise you will configure and deploy your own flow in your assigned SFTP Source Micro-Integration.

## 7.4 - Configuring and deploying a flow in an SFTP Source Micro-Integration

Now that you are familiar with the concept of flows in Micro-Integrations, it’s time to configure and deploy your own flow.

Because we are using an SFTP Source Micro-Integration, we need an SFTP server from which it can retrieve files.

For this exercise, we will use <a href="https://sftpcloud.io/tools/free-sftp-server" target="_blank">sftpcloud.io</a> to set up a temporary SFTP server and <a href="https://www.filestash.app/sftp-client.html" target="_blank">Filestash</a> to upload files to it.


### 7.4.1 - Create a temporary SFTP Server

1. Go to <a href="https://sftpcloud.io/tools/free-sftp-server" target="_blank">sftpcloud.io</a>.
2. Click on "**Create test SFTP Server**".
This creates a test SFTP Server that is available for one hour. This should be enough for the duration of this hands-on exercise.

![Create SFTP Server](images/sftptest1.png)

3. After the creation, you see an overview of:
* Host
* Port
* Username
* Password 

**NOTE**: Keep this tab open or copy these credentials to a notepad, we will need them very soon.

![SFTP Credentials](images/sftptest2.png)

### 7.4.2 - Configure and deploy the flow 

1. Go to **SAP Integration Suite, advanced event mesh** &rarr; **Micro-Integrations**.
2. Open your assigned SFTP Source Micro-Integration.

![Go to your MI](images/gotoMI2.png)

3. On the right side of the page, under "**Flows**", click "**Create Flow**"

![Create a flow](images/Flow1.png)

4. Enter a **unique name** and description for your flow, for example:
* **Name**: \[FIRSTNAME]\_\[LASTNAME]\_csv_to_events
* **Description**: Reads each new or updated sales CSV from the SFTP inbound folder, splits it into individual rows, maps every row to a JSON event

![Add flow name & description](images/Flow2.png)

5. Scroll down to “Source Endpoint” and add your SFTP Server URI in the format: **sftp://\<host\>:\<port\>/upload/retail/sales/inbound**

6. Enter the username and password of the SFTP server created in step 2

![Enter username & password](images/Flow3.png)

7. Scroll down again and use the following delimiter file configuration:
* **Event delimiter:  \n**
* **Field delimiter: ,**
**NOTE**: This means that for each line in the CSV file an event will be created, and that fields are separated with a “,”.

8. Scroll to Target Endpoint and enter the following Topic Name:
**retail/sales/lineitem/recorded/v1**
**NOTE**: For our use case, we will work with content-based topics, but since the topic field is mandatory the topic name we add serves as a placeholder.

9. Click "**Next: Error Handling**"

![Add flow details](images/Flow4.png)

10.  Optionally, you can enable retries and a Dead Message Queue (DMQ) here. Click “**Next: Mappings**”.

![Error Handling](images/Flow5.png)

11. We can now create a mapping. You’ll see a title that says, “No Source Payload”, under this title click on “**Import File**” and import the file [retail_sales_events.json](materials/retail_sales_events.json).
Afterwards, use the same file for the target payload. We are not going to focus on the message mapping in this example, we'll focus on the content-based topics instead.
**Note**: Even though we upload CSV files later, the Micro-Integration automatically converts them to a JSON structure.

![Add Sample JSON](images/Flow6.png)

12. After the files are uploaded, **link the source and the target fields**, they are all the same, make sure you don’t miss any.

![Link fields](images/Flow7.png)

**If you didn’t miss any, you should see a message saying that all target fields are mapped.**

![Check all fields are mapped](images/Flow8.png)

13. Once the mapping is complete, we will construct the topic using the Smart Topic Destination. This allows you to build a topic taxonomy based on the payload of your message. The topic that we will construct will look like this:

**retail/sales/lineitem/recorded/v1/{region}/{storeId}/{productCategory}/{transactionId}**

As you see, it is a combination of fixed values (retail/sales/lineitem/recorder/v1/) and the payload. In the next step, we will create these constants/fixed values.

14. Hover to the left side of your screen, and click on the “**+**” button next to “**Constants**”, under “**SFTP Source**”, and add the following constants:
* **First levels of the topic taxonomy:**
**Data Type**: String
**Constant Value**: retail/sales/lineitem/recorded/v1

![Constant 1](images/Flow9.png)

* **Separator:**
* **Data Type**: String
* **Constant Value**: /

![Constant 2](images/Flow10.png)

15. Drag a line from one of the values that we just created to “**Smart Topic Destination**” and click on “**Add transformation**”.

![Smart topic mapping 1](images/Flow11.png)

16. Use the **Concatenate** function and build a transformation that looks like this (use “**Add input**” to add fields):
* retail/sales/lineitem/recorded/v1
* /
* region
* /
* storeId
* /
* productCategory
* /
* transactionId
**NOTE**: Ensure there are no double slashes (//) and that the topic does not end with a slash (/).

17. Click "**Apply**", then click "**Next: Summary**".

![Smart topic mapping 1](images/Flow12.png)

18. Click "**Create**".

![Flow Summary](images/Flow13.png)


19. After a few seconds, you see the overview of your Micro-Integration again, and you see that your flow has been added. To deploy the flow, you need to deploy the Micro-Integration. You do that by clicking on the "**Deploy**" button at the top right of the page. A pop-up will appear to confirm the deployment. Confirm by clicking on "**Deploy**" once more.
**NOTE**: If somebody else already deployed their flow, you need to first undeploy and then redeploy the Micro-Integration. Before doing that, please inform one of the instructors.

![Deploy Micro-Integration](images/Flow14.png)

20. After the deployment, you should see that the flow is running successfully.


### 7.4.3 - Testing your Micro-Integration flow 

1. After creating the flow in your Micro-Integration, go to the cloud console &rarr; “**Cluster Manager**” and **open the broker to which your MI was deployed**.

![Go to your Broker](images/MITEST1.png)

2. Go to the “**Try Me!**” tab, **copy the client password** and click on “**Open Broker Manager**”.

![Go to Try Me!](images/MITEST2.png)

3. **Enter the client password** on the left side of the screen and click “**Connect**” on both the “Publisher” and “Subscriber” sides.

![Connect to broker](images/MITEST3.png)

4. Subscribe to the following topics:
* retail/sales/lineitem/recorded/v1/>
* retail/sales/lineitem/recorded/v1/emea/store-042/>
* retail/sales/lineitem/recorded/v1/*/*/beverages/>
**NOTE**: We're making use of <a href="https://help.pubsub.em.services.cloud.sap/Messaging/Wildcard-Charaters-Topic-Subs.htm" target="_blank">wildcards</a> for topic subscription.

![Subscribe to topics](images/MITEST4.png)

5. Now, go to <a href="https://www.filestash.app/sftp-client.html" target="_blank">Filestash</a> and connect to the SFTP Server you created with <a href="https://sftpcloud.io/tools/free-sftp-server" target="_blank">sftpcloud.io</a>.

![Connect to SFTP Server](images/filestash1.png)

6. You should see the following folder structure: **/upload/retail/sales/inbound**. This was created by the Micro-Integration flow we created in step 3.
**NOTE**: During the setup of the MI, we specified the folder structure. The moment we deployed the flow, it connected to the SFTP Server and looked for the folders. It didn’t find these folders, so it created them.

7. Click through until you are in the folder “inbound”.

8. Download the file [retail_sales_sample.csv](materials/retail_sales_sample.csv).

9. Drag and drop the CSV into the folder “inbound”.

![Upload CSV](images/uploadCSV.png)

10. After the upload, go back to the “**Try Me!**” tab and you should see the CSV lines flowing in as separate events. Feel free to play around with the file and subscriptions.

![Monitor incoming events](images/MITEST8.png)

### 7.4.4 - Troubleshooting

If issues occur:
1. Check if the SFTP server is still active (it expires after one hour).
2. Verify the SFTP credentials.
3. Ensure there are no double slashes (//) in the Smart Topic Destination.

You can view logs via Micro-Integrations &rarr; Select your SFTP Source MI &rarr; three dots at the top right &rarr; View Error Logs

![Troubleshooting Micro-Integrations](images/TS1.png)

## 7.5 - Micro-Integration Licensing (Optional)

This is an optional section that explains in more detail how Micro-Integrations are licensed.

Micro-Integrations in SAP Integration Suite, advanced event mesh are available through two licensing models: 
* **Included with your AEM broker**: certain broker classes (starting with Broker 1K) include a predefined number of Micro-Integrations at no additional charge. These included Micro-Integrations cannot be shared across SAP BTP Subaccounts. The table below shows how many Micro-Integrations are included with each broker service class.

| Service Class (nr of Connections) | Nr. of Micro-Integrations Included |
| --- | --- |
| Broker 100 (Developer) | 0 |
| Broker 250 (Smallest Production Broker) | 0 |
| Broker 1K | 2 |
| Broker 5K | 5 |
| Broker 10K | 8 |
| Broker 50K | 15 |
| Broker 100K | 25 |

* **Pay by the hour**: Additional Micro-Integrations can be deployed to any broker size. In this case, you're charged for each hour an additional Micro-Integration is in the "**Deployed**" state.

Each Micro-Integration can contain up to five flows. If you require more than five flows, you need to leverage multiple Micro-Integrations.

For pricing and licensing details, visit the <a href="https://discovery-center.cloud.sap/serviceCatalog/advanced-event-mesh/?tab=service_plan&commercialModel=cpea&region=all&service_plan=standard" target="_blank">SAP Discovery Center</a>.

## Summary 

In this exercise, you explored how Micro-Integrations connect external systems directly to Advanced Event Mesh, enabling lightweight, high-volume integration scenarios.

You learned:
* Which Micro-Integrations are available and what the different types are (Source, Target, Processor).
* How to create Micro-Integrations and configure their flows.
* Turn files into events by reading a CSV file from an SFTP server, converting each row into a JSON event, and publishing it to the mesh.
* Use content-based topics to route events based on their payload, allowing consuming applications to subscribe to the data they need.

The result? You transformed file-based data into individual JSON events, ready for your applications to consume through the mesh.



