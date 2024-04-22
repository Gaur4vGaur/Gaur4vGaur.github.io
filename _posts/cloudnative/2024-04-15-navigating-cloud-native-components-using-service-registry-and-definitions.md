---
title: 'Navigating Cloud Native Components: Using Service Registry and Definitions'
description: Explore the Service Registry and Service Definitions, crucial components in cloud-native architecture for efficient, reliable, and scalable applications. It explains their role in synchronous and asynchronous communication.
tags: ["cloud", "cloud native", "api", "communication"]
category: ["cloud native"]
date: 2024-04-15
permalink: '/cloud_native/navigating-cloud-native-components-using-service-registry-and-definitions/'
image:
  path: https://raw.githubusercontent.com/Gaur4vGaur/traveller/master/images/cloudnative/2024-04-15-navigating-cloud-native-components-using-service-registry-and-definitions/CoverImage.jpg
  width: 800
  height: 500
---


## Introduction
Imagine a bustling shopping centre in a metropolis with dozens of independent boutiques, each located in different corners. You’re on a mission to find a specific outfit, and to do so, you must navigate through the complex maze of stores. This is much like a cloud-native architecture - a complex system of small, independent services that need to locate and interact with each other. Navigating this system can be as challenging as finding that perfect outfit, but with the right tools, it is manageable.

One practical solution is to utilize Registry/Discovery and Definitions services. Imagine service discovery as an interactive mall directory, guiding shoppers directly to the store they need. Service definitions, on the other hand, function as a detailed store catalogue, outlining each service offering. In this article, we’ll explore these crucial components, their role in synchronous and asynchronous communication, and how they contribute to efficient, reliable, and scalable cloud-native applications. 

As I have mentioned in a [previous article](https://www.gaurgaurav.com/cloud_native/decoding-synchronous-and-asynchronous-communication-in-cloud-native-applications/){:target="_blank"}, there are two types of communication in cloud-native architecture. Let’s first focus our discussion on how synchronous communication can use service registry and service definitions to improve the architecture.

## Synchronous Communication: A Real-Time Interaction
Synchronous communication is like a real-world conversation where both parties must be available for the interaction. It involves direct, back-and-forth communication where the client waits for the server to respond before proceeding.

### Service Definitions: The Blueprint of Synchronous Communication
Synchronous communication relies on predefined and agreed-upon interfaces. A service definition serves as a contract that outlines how a service provides its functionality. This contract is shared with consumers before establishing communication. Typically, such a contract includes several key elements:
- **Service Interface Definition**: Lists exposed operations and endpoints with names, parameters, and return values.
- **Preferred Communication Protocol**: Specifies the communication protocol between services, such as REST, gRPC, or GraphQL.
- **Data Exchange Format**: Defines the format for exchanging data, which can be specified using JSON, Protobuf (Protocol Buffer), or XML.
- **Versioning**: Includes a version to guide users in case there are changes in the contract.
Service definitions act as living documentation for the service and can help reduce errors during development.

### Service Registry: The Centralized Information Hub
As mentioned earlier, the first step is to find a service that can help to achieve a specific functionality. Developers often do this by asking questions over Slack or in forums. However, this process is inadequate and can be streamlined through a service registry.
A service registry maintains a list of available services and stores information such as service name, location (IP and ports), health status, and metadata. This centralized approach helps consumers efficiently find and interact with the services they need in real-time.

![Service Regitery to expose Service Definition](https://raw.githubusercontent.com/Gaur4vGaur/traveller/master/images/cloudnative/2024-04-15-navigating-cloud-native-components-using-service-registry-and-definitions/ServiceRegistry.png)*Service Regitery to expose Service Definition*


### Guide to Using the Service Registry

Here is a guide to using the service registry:
- **Service Registration**: When a new service is deployed, it automatically registers itself with the service registry. This process involves essential information like the service name, location, and other relevant metadata.
- **Service Discovery**: Client applications needing to interact with specific services query the service registry using criteria such as the service name. This allows them to locate the appropriate services dynamically.
- **Service Routing**: The service registry provides clients with essential information, such as the network address, to connect and invoke the desired service.
- **Service Updates**: As services scale or change locations, they update their registration information in the service registry. This ensures that clients always connect to the correct service instances.
  

Service registries work hand in hand with service definitions, forming complementary concepts within synchronous communication. While the registry helps locate services, definitions describe how to interact with them effectively.


## Asynchronous Communication: The Flexible Interaction
Unlike synchronous communication, asynchronous communication enables services to communicate without waiting for an immediate response, much like sending an SMS where the recipient can reply at their convenience. This approach fosters decoupling and flexibility within systems.


### Schema Definition: The Language of Asynchronous Communication
In asynchronous interactions, service definitions are message formats and protocols. Both the producing and consuming services must speak the same "language" to communicate effectively. This entails comprehending the structure and format of the exchanged messages.
The data format follows a specific structure defined by a common schema. Since communication occurs asynchronously through a message broker or event bus, the producer ensures that it publishes data according to the agreed-upon schema, facilitating processing by the consumer.


### Schema Registry: The Centralized Schema Storage
Like services in synchronous messaging, this shared messaging contract must be easily discoverable. To achieve this, producer and consumer services utilize a centralized metadata registry to store the schemas. Let's dive deep into the schema registry process guide.

![Schema Regitery to expose Schema Definition](https://raw.githubusercontent.com/Gaur4vGaur/traveller/master/images/cloudnative/2024-04-15-navigating-cloud-native-components-using-service-registry-and-definitions/SchemaRegistry.png)*Schema Regitery to expose Schema Definition*

### Guide to Using the Schema Registry
- **Schema Definition**: When a new service is deployed, it defines the message schemas using a preferred format like Apache Avro, Protocol Buffers, or JSON Schema. This schema defines the structure of the messages, including the data types of fields.
- **Schema Registration**: The producer registers the schema with the schema registry. This usually involves providing the schema definition and a unique identifier to the registry. The registry may offer versioning capabilities to track changes.
- **Message Production**: When the producer service needs to send a message, it:
  - Fetches the relevant schema from the registry (if not already cached).
  - Validates the message data against the schema to ensure it complies with the expected format.
  - Serializes the message using the chosen format (e.g., Avro serialization).
  - Publishes the serialized message to the message broker or event bus.
- Message Consumption: On the receiving end, the consumer service:
  - Subscribes to the relevant topic or queue in the message broker.
  - Fetches the schema for the expected messages from the registry (if not already cached).
  - Desterilizes the received message based on the fetched schema.
  - Processes the data according to its internal logic.

The schema registry ensures that producers and consumers use compatible message formats, even as schemas evolve. This compatibility is crucial in cloud native distributed systems where different services may be developed and updated independently.


## Conclusion
By adopting these Service Definition Patterns, developers can build cloud-native applications that are not only efficient and reliable but also maintainable and scalable. The service definition and how to share that definition with the rest of the organization is tightly coupled to the overall governance aspects of building cloud-native applications. Therefore, adopting schemas in asynchronous messaging is vital for ensuring the reliability and safety of cloud-native applications. 


