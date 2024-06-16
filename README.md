Author Profiles blog
https://www.sivalabs.in/


Download the example code files
The code bundle for the book is hosted on GitHub at https://github.com/PacktPublishing/
Microservices-with-Spring-Boot-and-Spring-Cloud-Third-Edition. We also have other code
bundles from our rich catal

Kubernetes to CNCF (https://www.cncf.io/).
During 2018, Kubernetes became kind of a de facto standard, available both pre-packaged for
on-premises use and as a service from most of the major cloud providers

Source Code
https://github.com/PacktPublishing/Microservices-with-Spring-Boot-and-Spring-Cloud-Third-Edition.git

As explained in https://kubernetes.io/blog/2015/04/borg-predecessorto-
kubernetes/, Kubernetes is actually an open source-based rewrite of an internal
container orchestrator, named Borg, used by Google for more than a decade
before the Kubernetes project was founded


shared-nothing architecture; that is, microservices don’t share data in
databases with each other!


communicate through well-defined interfaces, either using APIs and synchronous
services or preferably by sending messages asynchronously. The APIs and message formats
used must be stable, well documented, and evolve by following a defined versioning strategy.
• It must be deployed as separate runtime processes. Each instance of a microservice runs in a
separate runtime process, for example, a Docker container.


Microservice instances are stateless so that incoming requests to a microservice can be handled
by any of its instances.


Rules of Thumb
Small enough to fit in the head of a developer

Decompose a monolithic application into a group of cooperating autonomous software components

Challenges with microservices


The 8 fallacies of distributed computing: Essentially everyone, when they first build
a distributed application, makes the following eight assumptions. All prove to be false
in the long run and all cause big trouble and painful learning experiences:

1. The network is reliable
2. Latency is zero
3. Bandwidth is infinite
4. The network is secure
5. The topology doesn’t change
6. There is one administrator
7. The transport cost is zero
8. The network is homogeneous


Design Patterns for Microservices (minimal list of design patterns)
• Service discovery
Keep track of currently available microservices and the IP addresses of its instances.

• Edge server
• Reactive microservices
• Central configuration
• Centralized log analysis
• Distributed tracing
• Circuit breaker
• Control loop
• Centralized monitoring and alarms


Solution requirements
Automatically register/unregister microservices
The client must be able to make a request to a logical endpoint for the microservice. The request will be routed to one of the available microservice instances.
 Requests to a microservice must be load-balanced over the available instances.
 We must be able to detect instances that currently are unhealthy so that requests will not be routed to them.

Client-side routing: The client uses a library that communicates with the service discovery
service to find out the proper instances to send the requests to.
• Server-side routing: The infrastructure of the service discovery service also exposes a reverse proxy that all requests are sent to. The reverse proxy forwards the requests to a proper microservice instance on behalf of the client.



Edge Server Problem
Expose some of the microservices to the outside of the system landscape and hide the remaining microservices from external access. The exposed microservices must be protected against requests from malicious clients

Client => edgeserver => Microsevice A, B, C, D
Implementation notes: An edge server typically behaves like a reverse proxy and can be integrated with a discovery service to provide dynamic load-balancing capabilities.

Edge Server Solution Requirements
Hide internal services that should not be exposed outside their context; that is, only route
requests to microservices that are configured to allow external requests
• Expose external services and protect them from malicious requests; that is, use standard
protocols and best practices such as OAuth, OIDC, JWT tokens, and API keys to ensure that
the clients are trustworthy


Reactive microservices Problem
Using a blocking I/O means that a thread is allocated from the operating system for the length of the request.
If the number of concurrent requests goes up, a server might run out of available threads in the operating system, causing problems ranging from longer response times to crashing servers. Using a microservice architecture typically makes this problem even worse, where typically a chain of cooperating microservices is used to serve a request. The more microservices involved in serving a request, the faster the available threads will be drained

Reactive microservices Solution
non-blocking I/O to ensure that no threads are allocated while waiting for processing to occur in another service, that is, a database or another microservice

Reactive microservices Solution Requirements
Whenever feasible, use an asynchronous programming model, sending messages without
waiting for the receiver to process them.
• If a synchronous programming model is preferred, use reactive frameworks that can execute synchronous requests using non-blocking I/O, without allocating a thread while waiting for a response. This will make the microservices easier to scale in order to handle an increased workload.
• Microservices must also be designed to be resilient and self-healing. Resilient meaning being capable of producing a response even if one of the services it depends on fails; self-healing meaning that once the failing service is operational again, the microservice must be able to resume using it.


Reactive systems is that they are message-driven;They use asynchronous communication. This allows them to be elastic, that is, scalable, and resilient, that is, tolerant to failures. Elasticity and resilience together enable a reactive system to always respond in a timely fashion


• Central configuration Problem
An application is, traditionally, deployed together with its configuration, for example, a set of environment variables and/or files containing configuration information. Given a system landscape based on a microservice architecture, that is, with a large number of deployed microservice instances, some queries arise:
• How do I get a complete picture of the configuration that is in place for all the running microservice instances How do I update the configuration and make sure that all the affected microservice instances are updated correctly?

• Central configuration Problem
Config server

• Central configuration Requirements
possible to store configuration information for a group of microservices in one place, with
different settings for different environments (for example, dev, test, qa, and prod).


Centralized log analysis Problem
How do I get an overview of what is going on in the system landscape when each microservice
instance writes to its own local log file?
• How do I find out if any of the microservice instances get into trouble and start writing error messages to their log files?
• If end users start to report problems, how can I find related log messages; that is, how can  
I dentify which microservice instance is the root cause of the problem? The following diagram illustrates the problem:

Centralized logging Solution
Detecting new microservice instances and collecting log events from them
• Interpreting and storing log events in a structured and searchable way in a central database
• Providing APIs and graphical tools for querying and analyzing log events

Centralized logging Requirements
Microservices stream log events to standard system output, stdout. This makes it easier for
a log collector to find the log events compared to when log events are written to microservice-specific log files.
• Microservices tag the log events with the correlation ID described in the next section regarding the Distributed tracing design pattern.
• A canonical log format is defined, so that log collectors can transform log events collected
from the microservices to a canonical log format before log events are stored in the central
database. Storing log events in a canonical log format is required to be able to query and analyze the collected log events.

Distributed Log Tracing Problem
possible to track requests and messages that flow between microservices while processing
an external request to the system landscape.
• If end users start to file support cases regarding a specific failure, how can we identify the
microservice that caused the problem, that is, the root cause?
• If one support case mentions problems related to a specific entity, for example, a specific order number, how can we find log messages related to processing this specific order – for example, log messages from all microservices that were involved in processing it?
• If end users start to file support cases regarding an unacceptably long response time, how can we identify which microservice in a call chain is causing the delay?



Distributed Log Tracing Solution
To track the processing between cooperating microservices, we need to ensure that all related requests
and messages are marked with a common correlation ID and that the correlation ID is part of all log events. Based on a correlation ID, we can use the centralized logging service to find all related log events. If one of the log events also includes information about a business-related identifier, for example, the ID of a customer, product, or order, we can find all related log events for that business identifier using the correlation ID.
To be able to analyze delays in a call chain of cooperating microservices, we must be able to
Collect timestamps for when requests, responses, and messages enter and exit each microservice. 

Distributed Log Tracing Solution requirements
Assign unique correlation IDs to all incoming or new requests and events in a well-known
place, such as a header with a standardized name
• When a microservice makes an outgoing request or sends a message, it must add the correlation ID to the request and message
• All log events must include the correlation ID in a predefined format so that the centralized logging service can extract the correlation ID from the log event and make it searchable
• Trace records must be created for when requests, responses, and messages both enter and exit a microservice instance


Circuit breaker problem
A system landscape of microservices that uses synchronous intercommunication can be exposed to a chain of failures. If one microservice stops responding, its clients might get into problems as well and stop responding to requests from their clients. The problem can propagate recursively throughout a system landscape and take out major parts of it.
This is especially common in cases where synchronous requests are executed using blocking I/O, that is, blocking a thread from the underlying operating system while a request is being processed. Combined with a large number of concurrent requests and a service that starts to respond unexpectedly slowly, thread pools can quickly become drained, causing the caller to hang and/or crash. This failure can spread unpleasantly quickly to the caller’s caller, and so on


Circuit breaker problem solution
Add circuit breaker is the solution

Circuit breaker requirements
Open the circuit and fail fast (without waiting for a timeout) if problems with the service are
detected.
• Probe for failure correction (also known as a half-open circuit); that is, allow a single request to go through on a regular basis to see whether the service is operating normally again.
• Close the circuit if the probe detects that the service is operating normally again. This capability is very important since it makes the system landscape resilient to these kinds of problems; in other words, it self-heals.




Control loop problem
In a system landscape with a large number of microservice instances spread out over a number of servers, it is very difficult to manually detect and correct problems such as crashed or hung microservice instances.


Control loop problem solution
Observer > Analyse > Act

Control loop problem requirements
It must be able to collect metrics from all the servers that are used by the system landscape,
which includes autoscaling servers
• It must be able to detect new microservice instances as they are launched on the available
servers and start to collect metrics from them
• It must be able to provide APIs and graphical tools for querying and analyzing the collected
metrics
• It must be possible to define alerts that are triggered when a specified metric exceeds a specified threshold value
The following screenshot shows Grafana, which visualizes metrics from Prometheus, a 
Monitoring tool that we will look at in Chapter 20, Monitoring Microservices

Software enablers
• Spring Boot, an application framework
• Spring Cloud/Netflix OSS, a mix of application framework and ready-to-use services
• Docker, a tool for running containers on a single server
• Kubernetes, a container orchestrator that manages a cluster of servers that run containers
• Istio, a service mesh implementation

Decomposing a monolithic application into microservices

One of the most difficult decisions
(and expensive if done wrong) is how to decompose a monolithic application into a set
of cooperating microservices. If this is done in the wrong way, you will end up with problems
such as the following:
• Slow delivery: Changes in the business requirements will affect too many of the microservices,
resulting in extra work.
• Bad performance: To be able to perform a specific business function, a lot of requests
have to be passed between various microservices, resulting in long response times.
• Inconsistent data: Since related data is separated into different microservices, inconsistencies can appear over time in data that’s managed by different microservices.

https://12factor.net


Spring Cloud Stream
Spring Cloud Stream provides a streaming abstraction over messaging, based on the publish and subscribe integration pattern. Spring Cloud Stream currently comes with built-in support for Apache Kafka and RabbitMQ. A number of separate projects exist that provide integration with other popular messaging systems. See https://github.com/spring-cloud?q=binder for more details.

Spring Cloud Stream comes with two programming models: one older and nowadays deprecated model
based on the use of annotations (for example, @EnableBinding, @Output, and @StreamListener) and
one newer model based on writing functions. In this book, we will use functional implementations.
To implement a publisher, we only need to implement the java.util.function.Supplier functional
interface as a Spring Bean


To make Spring Cloud Stream aware of these functions, we need to declare them using the spring.
cloud.function.definition configuration property. For example, for the three functions defined
previously, this would look as follows:
spring.cloud.function:
definition: myPublisher;myProcessor;mySubscriber


If we want to start and stop many containers with one command, Docker Compose is the perfect tool.
Docker Compose uses a YAML file to describe the containers to be managed.

In Chapter 18, Using a Service Mesh to Improve Observability and Management, and Chapter
19, Centralized Logging with the EFK Stack, we will learn about more powerful solutions to
track requests that are processed by the microservices.

Page 49 stop


Exercise folder
D:\Aung Aung\Microservices-with-Spring-Boot-and-Spring-Cloud-Third-Edition

Page 61 stop
Page 52 to start coding


Describing a RESTful API in a Java interface instead of directly in the Java class is, to me, a good way of separating
the API definition from its implementation. We

Page 61 stop at 30 March 2024

Page 71 stop at 30 March 2024

Page 76  need to learn about bash script
D:\LearningProjects\2-basic-rest-services\test-em-all.bash


Page 80 To page 102 will skip of  Deploying Our Microservices Using Docker as I don't have own laptop

Gradle build jar located in D:\LearningProjects\2-basic-rest-services\microservices\product-service\build\libs

Chapter 21 installation instructions for mac OS
• Chapter 22 installation instructions for Microsoft Windows with WSL 2 and Ubuntu
$BOOK_HOME/Chapter05.



Chapter 5
Adding an API Description Using OpenAPI

Page 112 stop => 31 March 2024



Before Page 131 skip => Mongo db and Mysql db are not available in my environment

Using Testcontainers
is a library that simplifies running automated
integration tests by running resource managers like a database or a message broker as a Docker container.
Testcontainers can be configured to automatically start up Docker containers when JUnit tests
are started and tear down the containers when the tests are complete.

page 132 using base class like easypay dev used ko than htike

Page 136 => optimisticLockError() test
same entity1 => save
another same entity2 => update holding the old version of same entity0


Page 142
Declaring a Java bean mapper
MapStruct is used to declare our mapper classes. The use of MapStruct is similar
in all three core microservices, so we will only go through the source code for the mapper object in
the product microservice.


MapStruct can use annotation based without coding to map
@Mapping(target = "rate", source="entity.rating"),
Recommendation entityToApi(RecommendationEntity entity);


Page 144 => Unit test of JPA and 

Page 145 => Page 162 
Extending the composite service API

Docket config => adding service to integration 
@DataMongoTest and @DataJpaTest

Page 145 => Page 162

Chapter 7
Page 163 => Developing Reactive Microservices => stop
how to develop non-blocking
synchronous REST APIs and asynchronous event-driven services

Page 166
Project Reactor is fundamental – it is what Spring WebFlux, Spring WebClient,
and Spring Data rely on to provide their reactive and non-blocking features

the core data types in Project
Reactor are Flux and Mono

A Flux object is used to process a stream of 0...n elements and 

a Mono object is used to process a stream that either is empty or returns at most one element


To actually get the stream processed,
we need someone to subscribe to it. The final call to the block method will register a subscriber
that waits for the processing to complete.


List<Integer> list = Flux.just(1, 2, 3, 4)
.filter(n -> n % 2 == 0)
.map(n -> n * 2)
.log()
.collectList().block();
the blocking code can call the block() method on the Flux or Mono object to get the response in a blocking way.

 

Page 169
the fluent API in Project Reactor

return repository.findByProductId(productId)
.switchIfEmpty(Mono.error(new NotFoundException("No product found
for productId: " + productId)))
.log(LOG.getName(), FINE)
.map(e -> mapper.entityToApi(e))
.map(e -> setServiceAddress(e));


Page 171
Dealing with blocking code
Using a thread pool for the blocking code avoids draining the available threads in the microservice and avoids affecting concurrent non-blocking processing in the microservice, if there is any.


Mono.fromCallable(() -> internalGetReviews(productId))
.flatMapMany(Flux::fromIterable)
.log(LOG.getName(), FINE)
.subscribeOn(jdbcScheduler);

Here, the blocking code is placed in the internalGetReviews() method and is wrapped
in a Mono object using the Mono.fromCallable() method. The getReviews() method uses
the subscribeOn() method to run the blocking code in a thread from the thread pool of
jdbcScheduler

Page 173
Non-blocking REST APIs in the composite services


Mono<Void>
instead of a void; otherwise, we can’t propagate any error responses back to the callers of the API

Page 174
Changes in the service implementation
To be able to call the three APIs in parallel, the service implementation uses the static zip() method
on the Mono class.
The zip method is capable of handling a number of parallel reactive requests and zipping them together once they all are complete


To create a WebClient instance, a builder pattern is used. If customization is required, for example,
setting up common headers or filters, it can be done using the builder. For the available configuration
options, see https://docs.spring.io/spring/docs/current/spring-framework-reference/webreactive.
html#webflux-client-builder.


Page 175
Changes in the integration layer
In the ProductCompositeIntegration integration class, we have replaced the blocking HTTP client,
RestTemplate, with a non-blocking HTTP client, WebClient, that comes with Spring 5.
To create a WebClient instance, a builder pattern is used. If customization is required, for example,
setting up common headers or filters, it can be done using the builder. For the available configuration
options, see https://docs.spring.io/spring/docs/current/spring-framework-reference/webreactive.
html#webflux-client-builder.

This completes the implementation of our non-blocking synchronous REST APIs.

Page 175
Developing event-driven asynchronous services
The composite service will publish create and delete events on each core service
topic and then return an OK response back to the caller without waiting for processing to take place
in the core services

Handling challenges with messaging
To implement the event-driven create and delete services, we will use Spring Cloud Stream.
A functionlal paradigm where functions implementing one of
the functional interfaces Supplier, Function, or Consumer in the java.util.function package can
be chained together to perform decoupled event-based processing

Page 178
To trigger such functional-based
processing externally, from non-functional code, the helper class StreamBridge can be used

streamBridge.send("topic", body);

The helper class StreamBridge is used to trigger the processing. It will publish a message on a topic. A
function that consumes events from a topic (not creating new events) can be defined by implementing
the functional interface java.util.function.Consumer as:

@Bean
public Consumer<String> mySubscriber() {
return s -> System.out.println("ML RECEIVED: " + s);
}


Adding configuration for publishing events and Adding configuration for consuming
events.



Page 184 stop


page 200 => look like can use sit Nova amq console 


Page 205
Using Kafka with two partitions per topic


Page 190
Publishing events in the integration layer
To publish
