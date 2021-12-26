# **`Modern Java Enterprise Applications Notes`**

# **Table of contents**

- [**The Path to Cloud Native Java**](#the-path-to-cloud-native-java)
- [**Travel Light on Your Pathway**](#travel-light-on-your-pathway)
- [**Kubernetes-Based Software Development**](#kubernetes-based-software-development)
- [**Working with Legacy**](#working-with-legacy)
- [**Building Kubernetes-Native Applications**](#building-kubernetes-native-applications)
- [**Tomorrow’s Solutions: Serverless**](#tomorrows-solutions-serverless)

# **Introduction**



# **The Path to Cloud Native Java**

Microservices are an accepted and well-recognized practice nowadays. For JavaEE
developers, this means a lift-and-shift change of the paradigm, where a single application
server does not contain all our business logic. Instead, it gets split into different 
microservices running in their application servers, with a minimal footprint and optimizations
to keep this coexistence functional and performant also in the cloud native world.

## **Example App**

The ecommerce application _Coolstore_ is a typical web app containing three components:

* _Presentation layer:_ 
  a frontend to show available items to acquire
* _Model layer:_ 
  a backend providing the business logic to catalog and index all items to sell
* _Data layer:_ 
  a database storing all records about transactions and items

![Architecture overview](Resources/Coolstore_Architecture.PNG "Coolstore Architecture")

We map the three previously mentioned components into several microservices, each one responsible
for its layer:

* _Catalog Service_ uses a REST API to expose the content of a catalog stored in a relational database.
* _Inventory Service_ uses a REST API to expose the inventory of items stored in a relational database.
* _Gateway Service_ calls the Catalog Service and Inventory Service in an efficient way. 
* _WebUI Service_ calls Gateway Service to retrieve all the information.

The Presentation and the Model layers are represented by such microservices, with the latter 
having an interface to the Data layer delegated to some DBMS. The e-store implementation is called
_Coolstore_ and looks like the picture:

![Interface of the App](Resources/Coolstore_Interface.PNG "Coolstore Interface")

### **Inventory microservice**

![Inventory Microservice implemented with Quarkus](Resources/Inventory_Microservice.PNG "Inventory Microservice")

For the Inventory service we need to create our Domain Model and RESTful Service. Once we implemented 
these components we pack an app together with Quarkus, and we can deploy it with a Maven goal.

[Inventory Quarkus](https://github.com/SaladinoBelisario/inventory-quarkus)

You can start the app in dev mode with a built-in Maven goal named _**quarkus:dev**_. It enables hot 
deployment with background compilation, which means that when you modify your Java files or your 
resource files and refresh your browser, these changes will automatically take effect.

### **Catalog microservice**

![Catalog Microservice implemented with Spring Boot](Resources/Catalog_Microservice.PNG "Catalog Microservice")

For the Catalog Service we need to create our Domain Model and RESTful Service also, but a little trick
we can use to do a faster development is using Spring Data to implement a Data Repository

[Catalog Spring Boot](https://github.com/SaladinoBelisario/catalog-spring-boot)

Once we have both microservices implemented it's time to move towards the UI and start by integrating 
both microservices with an API Gateway.

### **API Gateway**

Eclipse Vert.x is an event-driven toolkit for building reactive applications on the Java Virtual
Machine (JVM). VertX does not impose a specific framework or packaging model; it can be used
within your existing applications and frameworks in order to add reactive functionality by just
adding the Vert.x jar files to the application classpath. 

Eclipse Vert.x enables building reactive systems. In our architecture, this microservice will act as 
an asynchronous software API gateway, developed as a reactive Java microservice that efficiently
routes and dispatches the traffic to the Inventory and Catalog component of our cloud native 
ecommerce website.

![API Gateway with VertX](Resources/VertX_Gateway.PNG "VertX Gateway")

we want to create an API gateway as the entry point for the web frontend of our website, to 
access all backend services from a single place. This pattern is predictably called **API gateway** 
and is a common practice in microservices architecture.

The unit of deployment in Vert.x is called a _verticle_. A _verticle_ processes incoming events
over an event loop, where events can be anything such as receiving network buffers, timing 
events, or messages sent by other _verticles_.

[API Gateway VertX](https://github.com/SaladinoBelisario/gateway-vertx)

### **Frontend**

Node.js is a popular open source framework for asynchronous event-driven JavaScript development.
Even if this is a book about modern Java development, in microservices architecture it is common
to have a heterogeneous environment with multiple programming languages and frameworks involved.
The challenge here is how to let them communicate efficiently. One solution is having a common 
interface like API gateway exchanging messages via REST calls or queue systems.

AngularJS is a JavaScript-based frontend web framework whose goal is to simplify both the 
development and the testing of such applications by providing a framework for client-side
model–view–controller (MVC) and model–view–viewmodel (MVVM) architectures.

![User Interface with Node and Angular](Resources/Node_UI.PNG "Node UI")

[Node/Angular UI](https://github.com/SaladinoBelisario/web-nodejs)

# **Travel Light on Your Pathway**

Let’s take a deeper look at what makes us think about modernization in general and where to 
start looking for opportunities.

Enterprise software is developed to put business value into code that can be executed within 
nonfunctional and functional requirements. Creating value depends on our ability to deliver
applications quickly. Not only with better quality but also ready to be changed quickly, 
enabling businesses to respond to new challenges or regulatory changes in the market. And these
challenges are multifaceted.

Additionally, you will find operational concerns influencing modernization needs. For example, expiring 
maintenance contracts or outdated technologies can drive technology updates. The continuously
evolving Java language with the shortened release cycles can also influence modernization 
decisions. Modernization can happen at any level of your project, ranging from the execution 
environment (e.g., virtual machines to container) to the Java Virtual Machine (JVM version or 
vendor), individual dependencies, external interfaces, and services.

## **The 6 Rs**

Now that you’ve learned the motivation behind application modernization, you want to identify 
general approaches to modernization and define a categorization for existing applications. 
Doing this helps you manage a variety of different applications, especially in a platform 
modernization project. Rather than looking at the details of a single application, consider 
the complete runtime architecture of traditional Enterprise Java applications. In that case, 
you’ll commonly identify on-premise hardware, which is usually virtualized and made available
to projects via an individual set of instances. Given that individual projects are rarely 
treated as islands without any integrated systems, you get to a situation where a coordinated
approach for more than just one project needs to be found.

![6R for Modernize an App](Resources/6R.PNG "6R")

### **Retain—Modernize later or not at all**
Systems with this classification need a particular integration approach that needs to be 
explicitly designed. Imagine a highly scalable mobile application backend that connects 
directly to a mainframe. In this scenario, the requests from the potentially many mobile 
devices would overload the costly mainframe. Retain, in this case, does not mean “untouched” but
rather “not moved.”

### **Retire—Turn system off**
Some candidates may clearly have reached end-of-life and are already migrated and replaced or
just a relic that isn’t needed going forward. Travel light and make sure to flag these systems. 
Subsequent housekeeping is as equally essential as building new things. Investing time to
validate and decide on retiring a system is as valuable as a redesign would be.

### **Repurchase—Buy new version**
In some cases, you can repurchase off-the-shelf software and get it ready-made for a new 
execution environment. That sounds straightforward but will most likely include a migration 
project and reevaluation of feature lists, mostly because it is unlikely that you can update 
without changing the product version or its APIs. In some rare cases, you might even find 
missing integration documentation to be a blocker. This is why it is essential to treat this as
a modernization project and not as a simple software update.

### **Rehost—Put into containers**
Often referred to as “lift and shift,” one option for containerizing an application is to 
simply port the existing architecture as-is to run inside a container. While this can be as 
simple as it sounds, there are some challenges on the way. In particular, there can be
difficulties when it comes to optimizing the JVM for constrained container runtimes. Some 
existing middleware application servers come with their vendor supported base images and make
it convenient to switch runtimes. This step is addressing infrastructure modernization and not
concerned with application code directly. Existing applications that qualify for such an
approach are those that need to move to a container runtime before a refactoring can occur or as
an interim step toward switching data center concepts.

### **Replatform—Make some slight adjustments**
As an extension to rehosting, replatforming categorizes applications that undergo a conceptual
or functional change while switching runtimes. It can also be referred to by its own “lift”
name variation, “lift and adjust.” It can be related to a strangled functionality, which might
be implemented on top of a new technology stack or a change in data storage or integration systems. 
We recommend using this approach as an initial step toward refactoring and decoupling a
monolithic application. Prepending this step leads to smoother operations executing on 
subsequent extensions and decoupling stages. By choosing to replatform, you are allowing a 
gentle start to modernizing your applications and pragmatically evolving them.

### **Refactor—Build new**
Refactoring is a disciplined technique for restructuring an existing body of code, altering
its internal structure without changing its external behavior. Refactoring is the most
time-consuming and costly way to move existing applications onto a new runtime or platform. 
It may or may not include a switch to different architecture styles or on-premise or cloud hosting.

## **Divide and Containerize**
Now that we have looked at different modernization strategies for existing applications, and 
we know how and when to apply them. It is time to think about other prerequisites for our
target platform.

The word “platform” in the Enterprise Java world normally refers to the application server. 
Application servers follow a guardrail software development approach with standardized APIs.
The vertical layers are defined by what is commonly referred to as technical layers of a 
three-tier system. Presentation on top of business on top of data access and/or integration.
Horizontally to this we usually find business components or domains. While the vertical layers
are usually well separated and decoupled, it is common to find shared classes and violated 
access rules between the horizontal components. If this happens frequently across the code base,
we talk about entangled designs that turn into unmaintainable monoliths over time.

If we fast-forward to distributed architectures of today, where applications consist of many 
small services, we observe two things: there is no longer a shortcut to a good component design, 
and the standard application server features are no longer available to our components.

Let’s take a detailed look at some of the most critical functionalities, we call them 
_microservicilities_. The term refers to a list of cross-cutting concerns that a service 
**must** implement **apart from the business logic** to resolve these concerns.

![Cross-cutting concerns to develop distributed applications](Resources/Microservicilities.PNG "Microservicilities")

### **Discovery and configuration**
Container images are immutable. Storing configuration options inside them for different 
environments or stages is discouraged. Instead, the configuration has to be externalized and 
configured by instance. An externalized configuration is also one of the critical principles 
of cloud native applications. Service discovery is one way to get configuration information 
from the runtime environment instead of being hardcoded in the application. Other approaches
include using ConfigMaps and Secrets.

### **Basic invocation**
Applications running inside containers are accessed through Ingress controllers.
Ingress exposes HTTP and HTTPS routes from outside the cluster to services within the
cluster. Traffic routing is controlled by rules defined on the Ingress resource. You can use 
the routing capabilities to do rolling deployments as the basis for a sophisticated CI/CD 
strategy. For one-time jobs, such as batch processes, Kubernetes provides job and cron-job 
functionality.

### **Elasticity**
Kubernetes’s ReplicaSets control scaling of pods. It is a way to reconcile a desired state:
you tell Kubernetes what state the system should be in, so it can figure out how to reach the 
outcome. A ReplicaSet controls the number of replicas, or exact copies, of a container that 
should be running at any time. What sounds like a largely static operation can be automated. 
The Horizontal Pod Autoscaler scales the number of pods based on observed CPU utilization. It 
is possible to use a custom metric or almost any other application-provided metric as input.

### **Logging**
One of the more challenging aspects of a distributed application is the correlation of logs 
from each active part. This is an area where the difference from traditional application 
servers becomes very visible because it used to be so simple and isn’t in the new world. 
Storing them individually, per container, is not recommended because you lose sight of the 
bigger picture and have a hard time debugging side effects and root causes for issues. There 
are various approaches to this, with most of them extensively using the ELK (Elasticsearch, 
Logstash, Kibana) stack or a variant. In those stacks, Elasticsearch is the object store, where
all logs are stored. Logstash gathers logs from nodes and feeds them to Elasticsearch. Kibana is
the web UI for Elasticsearch, which is used to search the aggregated log files from various 
sources.

### **Monitoring**
Monitoring in a distributed application is an essential ingredient to make sure all the
bits and pieces continue working. In contrast to logging, monitoring is an active observation
often paired with alerting rather than simply recording events. Prometheus is the de facto 
standard for storing the generated information. Essentially, it is a complete open source 
monitoring system that includes a time-series database. Prometheus’s web UI gives you access 
to metric querying, alerting, and visualizations and helps you gain insights into your systems.

### **Build and deployment pipelines**
CI/CD (Continuous Integration/Continuous Delivery) isn’t anything new to Enterprise Java 
applications or distributed applications. As a good software development practice, every
production code should follow a strict and automated release cycle. With a potentially large 
number of services that compose an application, the automation should at least aim for 100% 
coverage. Traditionally a job for the open source tool Jenkins, modern container platforms 
have moved away from a centralized build system and embrace a distributed approach to CI/CD. 
The goal is to create reliable software releases through build, test, and deployment.

### **Resilience and fault tolerance**
Psychologists define “resilience” as the process of adapting well in the face of adversity, 
trauma, tragedy, threats, or significant sources of stress. In distributed applications, it is
the concept of recovering from failure or load scenarios without human interaction. Kubernetes
provides resilience options for the cluster itself, but only sparsely supports application 
resiliency and fault tolerance. For example, application-level resiliency can be facilitated
through PersistentVolumes that support replicated volumes or with ReplicaSets ensuring a 
consistent number of pod replicas across the cluster. On an application level, there is 
resilience and fault-tolerance support through Istio or various frameworks like Cloudstate. You
want to use features such as retry rules, circuit breaker, and pool ejection.

### **Security**
Authentication or Authorization between services is not part of Kubernetes itself. There are 
two ways to implement it. Using Istio, each service is provided with a strong identity that 
represents its role and enables interoperability across clusters and clouds. It secures 
service-to-service communication, as well as providing a key management to automate key and 
certificate generation, distribution, rotation, and revocation.

### **Tracing**
Tracing gives you a way to follow request paths and events throughout the system across
individual application parts by still allowing you to trace back to an origin. You can find
different approaches across the community today. Independent of languages, frameworks, or 
technologies you intend to use, Istio can enable distributed tracing. There are other commercial
and open source projects available helping with distributed tracing across your application 
components. Zipkin and Jaeger are two possible solutions.

## **Define Your Target Platform**
It’s important to note that the nine elements mentioned previously are focused on application
development and do not capture all the necessities of a modern container platform. Just
looking at this narrow focus leaves important areas unaddressed. A container platform needs to
provide features and capabilities for the complete team from Dev to Ops. Depending on specific
needs, there is no one-size-fits-all solution. A comprehensive way to define your target 
platform is to start with the three main layers: Core, Customer Experience, and Integration, 
then build your application landscape on an optimized technology stack. What sounds like a 
ready-to-use checklist is anything but. Companies are different in culture, technologies, and 
requirements, and the following lists are a recommended starting point without any claim to 
comprehensiveness.

### **Define the core**
Start with evaluating the core part of the platform. This category includes basic capabilities
like **container orchestration, storage mapping, rolling upgrades, site reliability engineering(SRE)
requirements, out-of-the-box support for the desired deployment models, and might even include
further support for virtual machines**. This category represents the technical foundation for 
your target platform:

* **Existing core capabilities** 
* **Functional gap assessment** 
* **Hybrid-cloud support** 
* **Security integration**
* **Managed services support** 
* **Operators/marketplace available (e.g., OperatorHub, Red Hat Marketplace)** 
* **Available support levels** 
* **Target deployment model** 
* **Core modernization approach**

### **Define the customer experience layer**
When thinking about platforms, one part gets too little attention: the customer experience 
layer, which contains a technical definition for the customer channels to the platform. A 
channel can be one of the B2X (business to something) portals or various other specific 
frontends. A cohesive platform that can host various applications also needs to include a clear
definition for the technical composition of the individual services:

* **Define customer-centric requirements** 
* **Assess existing cx framework versus build** 
* **Micro frontends (e.g., Entando)** 
* **Integration requirements** 
* **Data gap analysis** 
* **Mobile support**

### **Define the integration**
In a containerized world, integration becomes a new challenge. Coming from a traditional 
enterprise landscape, it has either been a centralized solution (Enterprise Service Bus or 
similar) or been part of the individual applications using some common integration framework 
like Apache Camel. Neither approach fits perfectly into a stateless container-oriented 
platform. What you are looking for in a target platform is the smooth integration between 
messaging components, data transformation logic, and service integration. All the relevant
parts need to scale well in a stateless environment for distributed systems, and it should be 
easy to extend a composed application with new capabilities:

* **Existing integration capabilities** 
* **Evaluate partner solution ecosystem** 
* **Define integration requirements (data sources, service integration, messaging, APIs)**
* **Define standards and frameworks (e.g., Camel K)** 
* **Evaluate serverless/knative integration (e.g., Camel K)**

### **Define the technology stack**
The remaining category focuses on individual technologies and frameworks. Think of it as a
blueprint repository defining the relevant technologies, services, and methodologies for a
productive environment. An underestimated influence on the requirements in this category is 
the available development skill in an organization. With a traditional Enterprise Java
background, it is not easy to completely switch to a reactive development approach and a 
stateless application design. Also, familiarity with existing APIs and time to productivity on
a new platform play a crucial role in picking the most suitable technology stack:

* **Technology stack assessment across core, CX, and external services** 
* **Microservices framework (e.g, Quarkus, Spring Boot)** 
* **Implementation recommendation (reactive, imperative, message-driven, etc.)** 
* **Deployment model (IaaS, PaaS, hybrid)** 
* **Define target development platform** 
* **Development skills gap analysis**

## **Mandatory Migration Steps and Tools**
Following the basic assumption that you have an existing application landscape in place and
cannot start everything as a green-field project, we emphasize moving existing applications 
into containers. Coming back to the 6 Rs from earlier, the first application you are taking
a look at should fall into one of the following Rs: Rehost, Replatform, and Refactor. While 
they look similar in their description, the most significant difference between the three 
approaches is business value versus migration time and cost.

![Migration cost vs Business value](Resources/Migration_Costs.PNG "Migration Costs")

# **Kubernetes-Based Software Development**

# **Working with Legacy**

# **Building Kubernetes-Native Applications**

# **Tomorrow’s Solutions: Serverless**

# **References/Bibliography**

> [1]
> M. Eisele y N. Vinto , Modernizing Enterprise Java A Concise Cloud Native Guide for
> Developers, 1.st ed. O’Reilley, 2021.
> 
> [2]
> M. Eisele, Modern Java EE Design Patterns Building Scalable Architecture for Sustainable 
> Enterprise Development, 2nd ed. O’Reilly Media, 2016.


