# ML Core Banking System Core Connector Test Harness
A development and testing harness for core connectors that support the P2P cross currency use case in a Mojaloop scheme


# Pre-requisites
Before you go through this test harness documentation, the following are required for you to be able to go through it successfully

- Working knowledge of RESTful APIs
- Docker 

# Overview 
This test harness is intended to provide a core connector testing and development framework. The idea of a core connector came about as a result of the need to have an integration middlewear between the Mojaloop Connector (SDK Scheme Adapter) and a Financial Service Provider (FSP) Core Banking System.

# Developing a Core Connector
To develop a core connector, it is important to understand the different apis that the core connector needs to expose and those it needs to consume to support receipt and initiation of payments in a Mojaloop scheme.

Here is a diagram that illustrates role of a core connector in a typical mojaloop payments scheme

![CBS Integratio Overview](./assets/CBS%20Integration%20Overview.png)

In the illustration, we see the core connector being used as an adaptor between the FSP Core Banking System and the Mojaloop Connector to facilitate payments. 

A core connector is an api server that receives requests from the mojaloop connector and brokers them to your core banking system and vice versa to perform payment operations.

The core connector is supposed to implement and expose two apis for it to be able to effectively support payment operations in a mojaloop scheme.

# Incoming Payments - Payee

Below is an architecture diagram that shows how a core connector supports payee receipt of funds from a mojaloop scheme.

![Payee Architecture](./assets/CBS%20Integration%20Diagrams-Payee%20Architectural%20Flow.drawio.png)

In this architecture the core connector is shown as a stateless synchronous api that supports the crediting of funds into an beneficiaries account.

The Mojaloop connector and the switch are simulated using TTK. To support incoming transactions, the core connector should implement the backend api for the Mojaloop connector.

Please find the api schema for the mojaloop connector backend api at the this location

```bash
./resources/api-spec/payee.yaml
```

Here is a [link](./resources/api-spec/payee.yaml) to the api schema file

When an inbound payment is received by the  mojaloop connector, it will make requests to your core connector to perform account lookup, quoting and effecting transfers.

The core connector needs to respond to the api calls described in the open api specification for payee.

# Outgoing Payments - Payer
Here is another illustration that shows how the core connector should be used to support out going payments i.e Payments that are initiated fron your financial service provider's customers.

![](./assets/CBS%20Integration%20Diagrams-Payer%20Architectural%20Flow.drawio.png)

In this architecture, the core connector is shown as a stateless synchronous api that brokers initiation of payments through the mojaloop connector.

To initiate payments to a beneficiary whose account is held in another financial service provider, the core connector would need to make requests to the outbound api of the mojaloop connector.

Please find the api schema for the mojaloop connector outgoing api at the this location

```bash
./resources/api-spec/payer.yaml
```

Here is a [link](./resources/api-spec/payer.yaml) to the api schema file

To have a fully functioning core connector, it needs to implement both apis for handling incoming payments and initiating outgoing payments.

Here is an architecture to illustrate how all the different apis line up in the integration setup.

![API Overview](./assets//CBS%20Integration%20Diagrams-APIOverview.drawio.png)


# Setup

Once you have developed the core connector, it will need to be tested to verify that it responds accordingly to all happy paths and exception cases.

To test your core connector, you will need to package it as a docker image using a [Dockerfile](https://docs.docker.com/reference/dockerfile/)

To build your core connectors image as part of this test harness, you will need to put the core connector code in the src folder and run the following command.

```bash
docker compose up --build
```
With the --build flag, docker will rebuild the image of the core connector using the src folder as the build context so it is important that the Dockerfile is at the root of the src folder.

Once all is in place correctly, the test stack will be created.

# Running Tests
To run the tests, you will need to open the TTK UI in your web browser.
 TTK is deployed as part of the test harness.

Follow this link to open the TTK Ui. http://localhost:6060


# Conclusion