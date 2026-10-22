![Pic 0](../../images/ex1-0.png)

## Exercise 1.1 - Log into Advanced Event Mesh and explore it

After completing these steps you will have familiarized yourself with AEM. These steps will give you a first impression and an overview of AEM


1. Log into Advanced Event Mesh

Link: https://eu10.console.pubsub.em.services.cloud.sap/login?zone-id=908a280d-c2f1-4d2b-b003-de94ffc5a4ee

<b> User: handson_***@education.cloud.sap </b> (replace *** with your user number)

<b>Password</b>: provided by your moderator

2. Explore Advanced Event Mesh  

![Pic 1](../../images/ex1-1.png)

Check out the different areas in the <b>Advanced Event Mesh cockpit</b>, representing the different categories of services AEM offers:

- <b>Mission Control</b>: Mission Control makes it easy to deploy event brokers, create event meshes, and optimize and monitor the health/performance of an event-driven system. Mission Control is a section in the Cloud Console that permits you to access event brokers, visualize and manage your event broker services, and visualize and design event meshes. Mission Control has a Cluster Manager and Mesh Manager that permits you to create event broker services and manage your event mesh.

     - <b>Cluster Manager</b>: event broker services are made available via Cluster Manager. Each event broker service consists of event brokers configured in a High-Availability (HA) setup.

     - <b>Mesh Manager</b>: use Mesh Manager to connect multiple event broker services that span different data centers to create an event mesh. An event mesh is an architectural layer that allows events from one application to be dynamically routed and received by any other application no matter where these applications are deployed (no cloud, private cloud, public cloud). This layer is composed by a network of event broker services. Event broker services are a modern form of messaging middleware, which are designed to move events across the distributed enterprise.

- <b>Event Portal</b>: Event Portal provides event management services. This subscribed service provides powerful tools to create, design, share, and manage various aspects of an EDA based on event brokers or other streaming technologies (such as Kafka).

- <b>Event Monitoring and Insights</b>: With Insights, we provide curated dashboards, easy-to-understand visualizations based on historical and real-time metrics, and timely notifications about your event broker services. This advanced information allows you to identify problems before they occur and helps you to better manage your services as your EDA scales. You can work with SAP to configure your monitoring to meet your needs. For advanced monitoring requirements, there's a single entry point to build custom visualizations to meet your organization's requirements. Coupled with visualizations is a notification email framework that alerts you when key metrics fall outside of your established thresholds. These notifications allow you to monitor what's occurring and correct developing issues before they impact or degrade your EDA. You can configure these notifications to integrate with your existing notification and logging systems.

3. Click on <b>Cluster Manager</b>

> [!TIP]
> If you don't see any broker, please uncheck Only show my services


![Pic 13](../../images/ex1-13.png)

> [!TIP]
> If you are asked to select an Environment, click `Default` from the drop-down.
> ![Pic 13.1](images/ex1-131.png)

4. Select a <b>Broker</b> by clicking on it

5. Click <b>Open Broker Manager</b>

![Pic 14](../../images/ex1-14.png)  

6. Explore the <b>Broker Manager</b>

![Pic 15](../../images/ex1-15.png)  

On the left side of the screen are the main sections to navigate through:

- <b>Message VPN</b>: VPN-level stats and config (a Message VPN is a virtual partition of a single broker... one AEM broker can host multiple Message VPNs, and each VPN can have different authorization schemes and topic spaces; client/messaging application activity happens within the scope of a VPN)
- <b>Clients</b>: information about connected and configured client applications
- <b>Queues</b>: used for Guaranteed / persistent messaging
- <b>Connectors</b>: helpful wizards to connect to a variety of web services
- <b>Access Control</b>: where you create new client usernames, ACL profiles, and client profiles
- <b>Replay</b>: where you can enable replay, to allow the broker to send previous messages again NOTE: Solace brokers do not use replay for recovery of persistent data (like Kafka)... there is a more fine-grained approach in Solace where each individual message is ACKnowledged to the broker when the consumer application is done with it
- <b>Try Me!</b> this is where we will connect two WebSocket test applications

## Exercise 1.2 - Create a queue in Advanced Event Mesh

After completing these steps you will have created a queue in Advanced Event Mesh.

1. Go back to the original tab in your browser and click on <b>Cluster Manager</b> on the left.
2. In the All Services screen click on the <b>Broker Europe Tile</b> (Frankfurt)

![Pic 2](../../images/ex1-2.png)  

> [!TIP]
> If you cannot see the tiles, uncheck the Only show my services box

3. Press <b>Manage</b>

![Pic 4](../../images/ex1-4.png)     

4. Under <b>Broker Manager Quick Setting</b> click the Queues link. A new window opens up.

![Pic 5](../../images/ex1-5.png)      

5. Click the <b>+Queue</b> button on the top right

![Pic 6](../../images/ex1-6.png)        

6. In the pop up enter the <b>queue name: User_*** (replace *** with your number)</b>

7. Click <b>Create</b>

![Pic 7](../../images/ex1-7.png)      

8. On the next screen click <b>Apply</b>

![Pic 8](../../images/ex1-8.png)      

9. Check whether you can find your queue in the list  

![Pic 9](../../images/ex1-9.png)   

## Exercise 1.3 - Create a queue subscription in Advanced Event Mesh

10. Click on your queue

11. Click on <b>Subscriptions</b>

12. Click on "+Subscription" button

![Pic 10](../../images/ex1-10.png)  


13. Enter the topic into the field. Use <b>topic_user_XXX</b> as the topic, and replace XXX with your group/participant number.

![Pic 11](../../images/ex1-11.png)  

HINT: Please note that this topic is very simple to facilitate this exercise. In real world scenarios, you can work with topic hierarchies that allow for filtering etc. And you could actually use dynamic topics, which are based on event data, as well. We will dive into this later.

14. Click on <b>Create</b>

15. Check on whether your queue subscription got created

![Pic 12](../../images/ex1-12.png)  

## Exercise 1.4 - Send an event from the Try Me! tool to your topic

16. Switch to the **"Try Me!"** Section in the menu on the left.

![Pic 13](../../images/ex1-26.png)  

17. You are now in the TryMe tool

![Pic 14](../../images/ex1-27.png)  

18. You might have to press the **"expand"-Icon** to show the connection details. Please note, that the properties have to be adjusted, otherwise the connection is not going to work. We will add the correct properties in the next steps.

19. Keep your current browser tab open and **go back to the other/main tab** and click on **"Connect"**. Select **View by "Protocol"**.

![Pic 15](../../images/ex1-28.png)  

20. Open the **"Solace Web Messaging"** entry and select **"Solace JavaScript API"**. Now a Pop-up on the right shows the connection details to use.

![Pic 16](../../images/ex1-29.png)  

21. Copy these details into the according properties of the "Try Me!" tool. Overwrite existing values.
    
![Pic 17](../../images/ex1-30.png)  

22. Click on <b>Connect</b> in the <b>Publisher Section</b>
> [!IMPORTANT]
> If your browser asks to select a certificate for authentication, press "Cancel" otherwise the connection will fail! If you accidentially clicked on a certificate, please restart your browser to show the dialog again.

![Pic 18](../../images/ex1-31.png)  

23. Enter your topic. You can as well clear all the pre-filled entries by clicking the link. On top, add a message that you like. Recommendation would be Hello World!
    
![Pic 19](../../images/ex1-32.png)  

24. Click <b>Publish</b> to send your message to your topic

25. You should see 1 message published in the <b>Publish Status</b>

![Pic 20](../../images/ex1-33.png)  

26. Switch back to the <b>Queues</b>

![Pic 21](../../images/ex1-34.png)  

27. Search for the queue you have created earlier. You should see one message queued.

![Pic 22](../../images/ex1-35.png)  

You have sent a message to a topic, and via the subscription this message has been stored in your queue.

28. You can leave it like this or you can play the same game a little longer: go back to the Try Me! tool and just click publish, and see in the other tab how the count of messages gets increased (don't forget to refresh the page). When you click on your queue, you get a more detailed summary.

![Pic 23](../../images/ex1-36.png)  

## Exercise 1.5 - Consume events using an external consumer app

29. There is one more thing you can do. So far we have been using the TryMe! tool which is very convenient since it is part of AEM. It is a real producer and a real consumer, and publishes and consumes the same way "external producers and consumers" would do.

It might still be interesting - and give you some more the feeling of real E2E eventing - to see a consumer outside of AEM that consumes events from your queue.

We have therefore created this small HTML-Application that connects to the AEM Broker and consumes events from a queue.

You can find it here in the GitHub structure: ex1/consumerapp/aem-consumer.html

Download this file and start it.

30. The app starts up in your browser and looks like this

![Pic 24](../../images/ex1-37.png)  

31. Now fill in the required entries that you get from your broker. It's the same approach as before with the TryMe! tool. 

![Pic 25](../../images/ex1-38.png) 

With one difference: this time you need to enter the queue to consume from as well. The queue will most likely be User_XXX (replace XXX with your number).

![Pic 26](../../images/ex1-39.png) 

32. Click connect and make sure to click Cancel if you are asked for a certificate. You have to be quick, otherwise you get a timeout.

![Pic 27](../../images/ex1-40.png) 

33. Now you are connected and every event ending up in your queue is consumed by the Consumer-App.

![Pic 28](../../images/ex1-41.png) 

So, you have two options using the TryMe! tool: you can either send an event directly into the queue or you can publish an event to your topic.

HINT: consuming from a queue is a common approach. You get the buffering that you do not get from topics. If you, and we have done that, combine this with the queue subscribing to a topic you have the best of both worlds.

34. When you are done click Disconnect and close the tool. If the app would keep consuming this could lead to confusion with some of our later exercises - the reason would be two consumers consuming from the same queue. So, please disconnect the consumer app.

## Summary

You have now created a queue in AEM and have subscribed to events via a topic. You have sent events to this topic using the Try me! tool. On top, ylou have consumed events from the queue via an external tool.

Please continue with [Exercise 2 - Explore Topic Hierarchies and Wildcards](../ex2/README.md)
