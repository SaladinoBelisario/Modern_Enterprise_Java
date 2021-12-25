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

![Architecture overview](Resources\Coolstore_Architecture.PNG "Coolstore Architecture")

We map the three previously mentioned components into several microservices, each one responsible
for its layer:

* _Catalog Service_ uses a REST API to expose the content of a catalog stored in a relational database.
* _Inventory Service_ uses a REST API to expose the inventory of items stored in a relational database.
* _Gateway Service_ calls the Catalog Service and Inventory Service in an efficient way. 
* _WebUI Service_ calls Gateway Service to retrieve all the information.

The Presentation and the Model layers are represented by such microservices, with the latter 
having an interface to the Data layer delegated to some DBMS. The e-store implementation is called
_Coolstore_ and looks like the picture:

![Interface of the App](Resources\Coolstore_Interface.PNG "Coolstore Interface")

### **Inventory microservice**

![Inventory Microservice implemented with Quarkus](Resources\Inventory_Microservice.PNG "Inventory Microservice")

For the Inventory service we need to create our Domain Model and RESTful Service. Once we implemented 
these components we pack an app together with Quarkus, and we can deploy it with a Maven goal.

[Inventory Quarkus](https://github.com/SaladinoBelisario/inventory-quarkus)

You can start the app in dev mode with a built-in Maven goal named _**quarkus:dev**_. It enables hot 
deployment with background compilation, which means that when you modify your Java files or your 
resource files and refresh your browser, these changes will automatically take effect.

### **Catalog microservice**

![Catalog Microservice implemented with Spring Boot](Resources\Catalog_Microservice.PNG "Catalog Microservice")

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

![API Gateway with VertX](Resources\VertX_Gateway.PNG "VertX Gateway")

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

![User Interface with Node and Angular](Resources\Node_UI.PNG "Node UI")

[Node/Angular UI](https://github.com/SaladinoBelisario/web-nodejs)

# **Travel Light on Your Pathway**

# **Kubernetes-Based Software Development**

# **Working with Legacy**

# **Building Kubernetes-Native Applications**

# **Tomorrow’s Solutions: Serverless**

# **References/Bibliography**

> [1]
> M. Eisele y N. Vinto , Modernizing Enterprise Java A Concise Cloud Native Guide for
> Developers, 1.st ed. O’Reilley, 2021.

