# ML Core Banking System Core Connector Test Harness
A development and testing harness for core connectors that support the P2P cross currency use case in a Mojaloop scheme


# Overview 
This test harness is intended to provide a core connector testing and development framework. The idea of a core connector came about as a result of the need to have an integration middlewear between the Mojaloop Connector (SDK Scheme Adapter) and a Financial Service Provider (FSP) Core Banking System.

# Developing a Core Connector
To develop a core connector, it is important to understand the different apis that the core connector needs to expose and those it needs to consume to support receipt and initiation of payments in a Mojaloop scheme.

Here is a diagram that illustrates role of a core connector in a typical mojaloop payments scheme

![CBS Integratio Overview](./assets/CBS%20Integration%20Overview.png)

In the illustration, we see the core connector being used as an adaptor between the FSP Core Banking System and the Mojaloop Connector to facilitate payments. 

A core connector is an api server that receives requests from the mojaloop connector and brokers them to your core banking system and vice versa to perform payment operations.

The core connector is supposed to implement and expose two apis for it to be able to effectively support payment operations in a mojaloop scheme.

Below is an architecture diagram that shows how a core connector supports payee receipt of funds from a mojaloop scheme.

![Payee Architecture](./assets/CBS%20Integration%20Diagrams-Payee%20Architectural%20Flow.drawio.png)

In this architecture the core connector is depicted as a stateless synchronous api that supports the crediting of funds into an beneficiaries account.



# Setup


# Run 
To run the 