## Chaos Testing

MTTF: mean time to failure 

MTTR: mean time to recovery 

Compare a restaurant system with the following two situations:

1. MTTF: 3 years MTTR: 12 hours 
2. MTTF: daily MTTR: a couple of seconds

Step 1:

Planed Crash

Proposeful crash the production system:

1. Developers know the importance of checks and testing
2. More accurate observations and monitoring 
3. increasing the awareness of everyone in the organization on the need to focus on resiliency.
4. Improve resilience 

Step 2:

Chaos Testing 

At a high level, *chaotic testing* is simply creating the capability to continuously, but randomly, cause failures in your production system.

This practice is meant to test the resiliency of the systems and the environment, as well as determine MTTR.

**As Defined by a Netflix Engineer:**

"Chaos engineering is the discipline of experimenting on a software system in production to build confidence in the system's capability to withstand turbulent and unexpected conditions."

Note that you can get creative and generate targeted yet random failures aimed at only a particular aspect of your environment, such as degrading system performance, shutting down access to part of the network, or killing off a microservice.

It also helps teams simulate real-world conditions needed to uncover the hidden issues, monitoring blind spots and performance bottlenecks that are difficult to find in distributed systems. This method is quite effective in preventing downtime or production outages before their occurrence.

Chaos engineering is the process of exposing a software system by introducing disruptive events, such as server outages or API throttling. In this process, we present failure scenarios faults to test the system's capability of surviving against unstable and unexpected conditions.

**The process:**

1. Define system normal behaviour 
2. Creating a hypothesis 
3. Apply real-world events like terminating servers, network failures, latency, dependency failure, memory malfunction, etc.
4. Observe results 

As more companies move toward microservices and other distributed technologies, the complexity of these systems increases. You can't remove the complexity, but through Chaos Engineering, you can discover vulnerabilities and prevent outages before they impact your customers.

Chaos Monkey: This is a simple app that would go through a list of clusters, pick one instance at random from each cluster, and at some point during business hours, turn it off without warning. It would do this every workday.

Chaos Kong works on large scale vanishing regions.

> Chaos engineering is when we break things in production on purpose. This isn't about creating chaos. Chaos Engineering is about making the chaos inherent in the system visible.

Chaos Engineering is the discipline of experimenting on a distributed system to build confidence in the system's capability to withstand
turbulent conditions in production. So Chaos testing is a form of *experimentation*, which sits apart from *testing*.

approached issues of resilience, failure injection, fault testing, disaster recovery testing