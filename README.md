# Quarkus Qpid JMS Quickstart

This project illustrates how you can use the [Apache Qpid JMS](https://qpid.apache.org/components/jms/) client with [Quarkus](https://quarkus.io/) to interact with AMQP 1.0 servers using the [Quarkus Qpid JMS extension](https://github.com/amqphub/quarkus-qpid-jms).

## AMQP server

To use the quickstart, you first need a running AMQP 1.0 server. For example, you can follow the instructions from the [Apache ActiveMQ Artemis web site](https://activemq.apache.org/components/artemis/), or run the broker using docker:
```bash
docker run -it --rm -p 8161:8161 -p 61616:61616 -p 5672:5672 -e ARTEMIS_USERNAME=quarkus -e ARTEMIS_PASSWORD=quarkus vromero/activemq-artemis:2.11.0-alpine
```

## Start the application

The application can be started using:

```bash
mvn quarkus:dev
```

Then, open your browser to [http://localhost:8080/prices.html](http://localhost:8080/prices.html), and you should see a button to fetch the latest price.

## Anatomy

In addition to the [prices.html](src/main/resources/META-INF/resources/prices.html) page, the application is composed by 3 components:

* `PriceProducer` - the [PriceProducer](src/main/java/org/acme/jms/PriceProducer.java) sends random prices to a JMS queue.
* `PriceConsumer` - the [PriceConsumer](src/main/java/org/acme/jms/PriceConsumer.java) receives the JMS message and stores the last price.
* `PriceResource`  - the [PriceResource](src/main/java/org/acme/jms/PriceResource.java) gets the latest price from the PriceConsumer and returns it to the browser.

The configuration is located in the application configuration.

## Running in native

You can compile the application into a native binary using:

`mvn clean package -Pnative`

Or, if you dont have GraalVM installed, you can instead use Docker to build the native executable using:

`mvn clean package -Pnative -Dquarkus.native.container-build=true`.

and then run with:

`./target/qpid-jms-quickstart-1.0-SNAPSHOT-runner`
