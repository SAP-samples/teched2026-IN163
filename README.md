# TechEd 2026: Build your <span style="color:red;"> own </span> event-driven scenario with advanced event mesh

[![REUSE status](https://api.reuse.software/badge/github.com/SAP-samples/teched2026-IN163)](https://api.reuse.software/info/github.com/SAP-samples/teched2026-IN163)

![Pic 1](images/ISF-1.png)

## Description

SAP Integration Suite, advanced event mesh is SAP's flagship event broker. Actually, it is more than just that: it allows you to create meshes of event brokers and to use a state-of-the-art eventing platform, operations and event management features. Explore these powerful features of SAP Integration Suite, advanced event mesh. 

The goal is for you to learn what you need for an event-driven scenario in an end-to-end way. Start with core event-driven architecture elements like queues and topics, then dive into advanced topics and filtering to simulate a social media–like experience. Next you can try out advanced capabilities such as event replay. You will as well learn about the features and tools that allow you to achieve the event-driven hockey stick - and before that you learn what that is. In the starter scenario you can simulate events using AEM tooling on both source and consumer side. AEM offers the TryMe! tool for that. On top, you can install a small local HTML based tool for local event consumption.

If you want to go beyond, in the advanced track we do offer the chance to check out the new Event Studio that is part of the Event Add-On for ERP. The Add-On allows you to create custom events, and this is what you can do. Create an event, trigger it, and consume it in AEM using the TryMe! tool. Then, you can have a look at on-ramping and off-ramping events to the mesh using the Micro-Integration functionality of Advanced Event Mesh.

## Overview

![Pic 2](images/ISF-2.png)

SAP Integration Suite, advanced event mesh is a fully managed event streaming and management service that enables enterprise-wide and enterprise-grade event-driven architecture. It is a full blown, general purpose event mesh. AEM offers enterprise-grade performance, reliability, security and governance. It scales to very large use cases – and very means very very very in this case.

- Advanced Event Mesh is a distributed mesh of event brokers that can be deployed across environments, both in the cloud and on-premise
- It offers a full purpose set of eventing services covering all relevant use cases
- AEM supports event streaming, event management and event monitoring
- Brokers fully scale as required and come in different broker sizes to perfectly fit different needs

SAP Integration Suite, advanced event mesh

- is a general purpose, multi-site EDA platform and portal
- allows for SAP to SAP, SAP to Everything and Everything to Everything scenarios
- supports distributed networks of event brokers deployed in different clouds and on-premises
- can be deployed in your cloud of choice or in your on premises K8S
- provides sophisticated authentication and security features like Kerberos, OAuth or TLS
- offers fine-grained filtering options
- allows for real-time monitoring, capacity insights and distributed tracing
- provides support for all relevant protocols like JMS, REST, AMQP and MQTT, plus SMF.
- allows to start small and upgrade to bigger T-shirt sizes when your business grows
- provides an outstanding performance up to billions of events per day

SAP Integration Suite, advanced event mesh features touched, some just shortly, some in more detail, include:

- SAP Integration Suite, advanced event mesh queues
- SAP Integration Suite, advanced event mesh Cluster Manager
- SAP Integration Suite, advanced event mesh Mesh Manager
- SAP Integration Suite, advanced event mesh Event Designer and Event Management
- SAP Integration Suite, advanced event mesh Try Me! tool
- SAP Integration Suite, advanced event mesh Event Portal

## Requirements

- Understanding the basics of event-driven architectures, namely events, queues, topics, event subscriptions ...
- You would be able to execute most of the exercises without prior experience by just following the descriptions. For taking value out of the chance to explore Advanced Event Mesh some experience with event-driven architectures is recommended.
- Important: this session offers a main track on beginner level. On top of that, it offers several deep dive topics that typically require at least advanced know-how. If you follow the main track and add the Event Studio exercise, you are well equipped to create custom events, distribute them via the broker based on topics and filtering, and consume them. In a real world scenario you would obviously have to replace the TryMe! tool with a real consumer. This is very straighforward though - as you could see by adding the HTML-based consumer to your scenario.

## Exercises

![Pic 3](images/ISF-3.png)

### MAIN TRACK (work on exercises in the given order)

This is the main track that is geared at beginner level. Start from the beginning and follow the flow of the exercises.

Preparation and Setup

- [Getting Started](exercise/ex0/)

SAP Integration Suite, advanced event mesh

- [Exercise 1 - Explore SAP Integration Suite, advanced event mesh](exercise/ex1/README.md)

    - [Exercise 1.1 - Log into Advanced Event Mesh and Explore it](exercise/ex1/README.md#exercise-11---log-into-advanced-event-mesh-and-explore-it)
    - [Exercise 1.2 - Create a Queue in Advanced Event Mesh ](exercise/ex1/README.md#exercise-12---create-a-queue-in-advanced-event-mesh)
    - [Exercise 1.3 - Create a Subscription in Advanced Event Mesh](exercise/ex1/README.md#exercise-13---create-a-queue-subscription-in-advanced-event-mesh)
    - [Exercise 1.4 - Send an event from the Try Me! Tool to your Topic](exercise/ex1/README.md#exercise-14---send-an-event-from-the-try-me-tool-to-your-topic)
    - [Exercise 1.5 - Consume events from an external consumer app](exercise/ex1/README.md#exercise-15---consume-events-using-an-external-consumer-app)

- [Exercise 2 - Topic Hierarchies](exercise/ex2/README.md)

    - [Exercise 2.1 - Learn about Topic Hierarchies and Wildcards](exercise/ex2/README.md#exercise-21-learn-about-topic-hierarchies-and-wildcards)
    - [Exercise 2.2 - Practice Topic Hierarchies and Wildcards using Try Me !](exercise/ex2/README.md#exercise-22-practice-topic-hierarchies-and-wildcards-using-tryme---berlin-edition) 

- [Exercise 3 - Persistent and Non-Persistent Quality of Service](exercise/ex3/README.md)

    - [Exercise 3.1 - Learn about Persistency and QoS](exercise/ex3/README.md#exercise-31-learn-about-delivery-modes-persistency-and-quality-of-service)
    - [Exercise 3.2 - Experimenting with Persistency](exercise/ex3/README.md#exercise-32-experimenting-with-persistency)

- [Exercise 4 - Event Replay](exercise/ex4/README.md)

    - [Exercise 4.1 - Learn about Replay](exercise/ex4/README.md#exercise-41-learn-about-replay)
    - [Exercise 4.2 - Experimenting with Replay](exercise/ex4/README.md#exercise-42-experimenting-with-replay)

- [Exercise 5 - The Event-Driven Hockey Stick](exercise/ex5/README.md)

    - [Exercise 5.1 - Learn about the Event-Driven Hockey Stick](exercise/ex5/README.md#exercise-51-learn-about-the-event-driven-hockey-stick)
    - [Exercise 5.2 - Explore Events in the SAP Business Accelerator Hub](exercise/ex5/README.md#exercise-52-explore-events-in-the-sap-business-accelerator-hub)
    - [Exercise 5.3 - Experiment with the Event Portal](exercise/ex5/README.md#exercise-53-experiment-with-the-event-portal)

### Deep Dive Topics 

The Deep Dive track contains advanced topics. You can pick a single topic, or work on both topics in whatever order you like.

- [Exercise 6 - Event Add-On for ERP - Event Studio Deep Dive ](exercise/ex6/README.md)

    - [Exercise 6.1 - Learn about Event Studio and the Scenario](exercise/ex6/README.md#exercise-61-learn-about-event-studio-and-the-scenario)
    - [Exercise 6.2 - Open Event Studio and Create your CDS Payload](exercise/ex6/README.md#exercise-62-open-event-studio-and-create-your-cds-payload)
    - [Exercise 6.3 - Define the Payload Content](exercise/ex6/README.md#exercise-63-define-the-payload-content)
    - [Exercise 6.4 - Create an Event for your Payload](exercise/ex6/README.md#exercise-64-create-an-event-for-your-payload)
    - [Exercise 6.5 - Deploy the Event to Advanced Event Mesh](exercise/ex6/README.md#exercise-65-deploy-the-event-to-advanced-event-mesh)
    - [Exercise 6.6 - Consume your Events in AEM](exercise/ex6/README.md#exercise-66-consume-your-events-in-aem)
    - [Exercise 6.7 - Change your Payload (Optional)](exercise/ex6/README.md#exercise-67-change-your-payload-optional)

- [Exercise 7 - Micro-Integrations](exercise/ex7/README.md)

    - [Exercise 7.1 - Micro-Integration Use Cases](exercise/ex7/README.md#71---micro-integration-use-cases)
    - [Exercise 7.2 - Exploring Micro-Integrations](exercise/ex7/README.md#72---exploring-micro-integrations)
    - [Exercise 7.3 - Getting familiar with the structure of a Micro-Integration](exercise/ex7/README.md#73---getting-familiar-with-the-structure-of-a-micro-integration)
    - [Exercise 7.4 - Configuring and deploying a flow in an SFTP Source Micro-Integration](exercise/ex7/README.md#74---configuring-and-deploying-a-flow-in-an-sftp-source-micro-integration)
    - [Exercise 7.5 - Micro-Integration Licensing (Optional)](exercise/ex7/README.md#75---micro-integration-licensing-optional)

## User Data and Password

In order to log into Advanced Event Mesh, you can use the below email address with XXX replaced with your group number.

handson_XXX@education.cloud.sap (e.g. handson_012@education.cloud.sap)

The initial password will be provided to you by the moderators.

When you log in for the first time, you can enter a password of your choice.

## Background Material

In case that you would like to dig deeper into event-driven architecture: a lot of material to get up to speed with SAP Integration Suite, advanced event mesh and EDA in general is available. Please find a selection below:

- Blogs

    - [AEM Blog Post Collection](https://community.sap.com/t5/technology-blog-posts-by-sap/sap-integration-suite-advanced-event-mesh-blog-collection/ba-p/14111943)

- Tutorials and Missions

    - [Discovery Center Mission: Get started with AEM](https://discovery-center.cloud.sap/missiondetail/4582/4870/)
    - [Developer Center: AEM with SAP Integration Suite, SAP S/4HANA Cloud, SAP Build Proces Automation](https://developers.sap.com/tutorials/mission-get-started-with-sap-integration-suite-advanced-event-mesh/)

- Videos

    - [Devtoberfest 2024: Discover SAP Event Sources for Advanced Event Mesh](https://www.youtube.com/watch?v=NujHueCBDOw)
    - [Devtoberfest 2025: Hands-on session: practical demonstration of the Event Add-On for ERP](https://www.youtube.com/watch?v=vwzqNLISvHo)
    - [Devtoberfest 2026: Hands-on session: practical demonstration of the Event Studio](https://www.youtube.com/watch?v=HyLYUZ7bcII)

- Documentation

    - [Help](https://help.pubsub.em.services.cloud.sap/Cloud/cloud-lp.htm)
    - [SAP Discovery Center - AEM](https://discovery-center.cloud.sap/serviceCatalog/advanced-event-mesh?region=all)
    - [Advanced Event Mesh Tutorials](https://help.pubsub.em.services.cloud.sap/Cloud/ggs_signup.htm)

## Getting Started

Please continue with [Exercise 0](exercise/ex0/README.md)

## How to obtain support

Support for the content in this repository is available during the actual time of the online session for which this content has been designed. Otherwise, you may request support via the [Issues](../../issues) tab.

## License
Copyright (c) 2026 SAP SE or an SAP affiliate company. All rights reserved. This project is licensed under the Apache Software License, version 2.0 except as noted otherwise in the [LICENSE](LICENSES/Apache-2.0.txt) file.
