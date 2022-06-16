## Chaos Engineering

We all try our best to design and develop the best possible software systems. We use unit testing, functional (integration) testing, and many other tests to make sure that our system works the way it should. But what if there was something that we, as developers, missed to consider? What if that unknown bug crashes our service? What about cascading failure?

> Chaos Engineering is the discipline of experimenting on a system in order to build confidence in the system’s capability to withstand turbulent conditions in production.

### Why is this important?
The availability of a software system plays a critical role in customer satisfaction. Just consider that your banking app is unresponsive and there is no way to access your account. We always try to prevent such situations. This is the reason behind many best practices and design patterns, such as the circuit breaker or bulkhead pattern.

But after all, disasters happen. Sometimes by unhandled exception, other times by malicious user, or by hardware failure, or malformed response from third party services,...

At a high level, *chaotic engineering* is simply creating the capability to continuously, but randomly, cause failures in your production system. Here are two useful terms in CE:

MTTF: Mean Time To Failure 

MTTR: Mean Time To Recovery 

The practice of CE is meant to test the resiliency of the systems and the environment, as well as determine MTTR. To better understand the importance of MTTR, compare a banking system with the following two situations:

1. The mean time between system failures is 3 years, but when such an incident happens, it takes 12 hours to fix it. MTTF: 3 years MTTR: 12 hours

2. System failures occur every month, but when it takes just a couple of seconds to fix it: MTTF: monthly MTTR: a couple of seconds

Benefits of a proposed crash in the production system:

1. Developers know the importance of checks and testing.
2. More accurate observations and monitoring 
3. Increasing the awareness of everyone in the organization on the need to focus on resiliency.
4. Improvement of resilience
5. Improvement of MTTR

As Defined by a Netflix Engineer:

> Chaos engineering is the discipline of experimenting on a software system in production to build confidence in the system's capability to withstand turbulent and unexpected conditions.

Note that you can get creative and generate targeted yet random failures aimed at only a particular aspect of your environment, such as degrading system performance, increasing response time, shutting down access to part of the network, or killing off a microservice.

It also helps teams simulate real-world conditions needed to uncover hidden issues, monitoring blind spots, and performance bottlenecks that are difficult to find in distributed systems. This method is quite effective in preventing downtime or production outages before they occur.
In this process, we present failure scenarios to test the system's capability of surviving against unstable and unexpected conditions.

### Chaos Testing Steps:

1. Defining the system's normal behavior
2. Creating a Hypothesis
3. Applying real-world events like terminating servers, network failures, latency, dependency failures, memory malfunctions, etc.
4. Observing the results
5. Improving
6. Iterate 

As more companies move toward microservices and other distributed technologies, the complexity of these systems increases exponentially. You can't remove the complexity, but through Chaos Engineering, you can discover vulnerabilities and prevent outages before they impact your customers.

### Chaos Monkey: 
There are platforms developed to enhance the process of CE. Some of them are open source, and others are commercial products.  
[Chaos Monkey](https://netflix.github.io/chaosmonkey/) is an application developed by NetFlix and is responsible for randomly terminating instances in production to ensure that engineers implement their services to be resilient to instance failures.
This is a simple app that would go through a list of clusters, pick one instance at random from each cluster, and at some point during business hours, turn it off without warning. It would do this every workday.

**Chaos Kong ** is another product by NetFlix that works on large scale vanishing regions.

[Gremline](https://www.gremlin.com/) is another chaos engineering platform. 

> Chaos engineering is when we break things in production on purpose. This isn't about creating chaos. Chaos engineering is about making the chaos inherent in the system, visible.

For anyone who is interested in learning more about this topic [here](https://github.com/dastergon/awesome-chaos-engineering) is a list of awesome resources.
