## Coupling in Microservices

## Domain Coupling
## Temporal Coupling:
This type of coupling occurs when two or more services need to be accessible at the same time. It is a situation where one service needs other services to do something at the same time.
For example order service needs warehouse service to reserve the product so the warehouse service should be up and accessible. If for any reason warehouse service is not reachable the whole process fails. 
Temporal coupling is not always bad, It's just something to be aware of. 
## Pass Through Coupling 
Pass-through coupling”9 describes a situation in which one microservice passes data to another microservice purely because the data is needed by some other microser‐ vice further downstream.
For example order service sends shipment data including address and shipping type to warehouse services and warehouse service passes it to shipment service.
The major issue with pass-through coupling is that a change to the required data downstream can cause a more significant upstream change.
