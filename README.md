# EurekaServer
EurekaServer @ http://localhost:8761/

Ribbon-based load balancing is sufficient for most of the microservices requirements. However, this approach falls short in a couple of scenarios:

a)If there is a large number of microservices, and if we want to optimize infrastructure utilization, we will have to dynamically change the number of service instances and the associated servers. It is not easy to predict and preconfigure the server URLs in a configuration file.
b)When targeting cloud deployments for highly scalable microservices, static registration and discovery is not a good solutionconsidering the elastic nature of the cloud environment.
c)In the cloud deployment scenarios, IP addresses are not predictable, and will be difficult to statically configure in a file. We will have to update the configuration file every time there is a change in address.

The Ribbon approach partially addresses this issue. With Ribbon, we can dynamically change the service instances, but whenever we add new service instances or shut down instances, we will have to manually update  the  Config  server.  
Though  the  configuration  changes  will  be  automatically  propagated  to  all required instances, the manual configuration changes will not work with large scale deployments. When managing large deployments, automation, wherever possible, is paramount.
To fix this gap, the microservices should self-manage their life cycle by dynamically registering service availability, and provision automated discovery for consumers.
With dynamic registration, when a new service is started, it automatically enlists its availability in a central service registry. Similarly, when a service goes out of service, it is automatically delisted from the service registry.

Dynamic discovery is where clients look for the service registry to get the current state of the services topology, and then invoke the services accordingly. In this approach, instead of statically configuring the service URLsin config server rather the URLs are picked up from the service registry.

There are a number of options available for dynamic service registration and discovery. Netflix Eureka, ZooKeeper, and Consul are available as part of Spring Cloud. In this chapter, we will focus on the Eureka implementation.

Eureka is primarily used for self-registration, dynamic discovery, and load balancing. Eureka uses Ribbon for load balancing internally.

As shown in the preceding diagram, Eureka consists of a server component and a client-side component. The server component is the registry in which all microservices register their availability. The registration typically includes service identity and its URLs. The microservices use the Eureka client for registering their availability. The consuming components will also use the Eureka client for discovering the service 


Conclusion
-----------
1) My experience with self-preservation is that it’s a false-positive most of the time where it incorrectly assumes a few down microservice instances to be a poor network partition.
2) Self-preservation never expires, until and unless the down microservices are brought back (or the network glitch is resolved).
3) If self-preservation is enabled, we cannot fine-tune the instance heartbeat interval, since self-preservation assumes heartbeats are received at intervals of 30 seconds.
4) Unless these kinds of network glitches are common in your environment, I would suggest to turn it off (even though most people recommend to keep it on).