

One of the features of the logging service is the ability to collect logs directly from the running instance using the existing Oracle Cloud Agent. The only thing you need to do is to enable the Custom Agent Plug-in and create an Agent Configuration.

**Custom Logs** Diagnostic information from custom applications, other cloud providers, or an on-premise environment. To ingest custom logs, call the API directly or configure the unified monitoring agent.

Go to Menu → Observability&Management → Agent Configurations

![](https://cdn-images-1.medium.com/max/1200/1*mdZdbXRp_9dFyV28toyVww.png)

Click Create Agent config:

![](https://cdn-images-1.medium.com/max/1200/1*Th8QF3jrmtr3IuzAmeRwOg.png)

![](https://cdn-images-1.medium.com/max/1200/1*MebXcJ8Dix6qN1y5aEdC3Q.png)

As you can see, there are 2 pre-requisites before the custom logs will work:

1- Configure the user group or Dynamic Group to use all instances from a compartment, or different instances to allow log collection

1.1 Create the Dynamic Group in Menu →Identity & Security → Dynamic Groups and press Create Dynamic Group

![](https://cdn-images-1.medium.com/max/1200/1*k-8D0bHps-APdUYcQKie0A.png)

1. 2 Add the matching rule Following the Service documentation:

[Agent Management (oracle.com)](https://docs.oracle.com/en-us/iaas/Content/Logging/Concepts/agent_management.htm)

An example is this:

![](https://cdn-images-1.medium.com/max/1200/1*KWtewCOrWDEefq-NY_FvfQ.png)

![](https://cdn-images-1.medium.com/max/1200/1*elJFqGnZFeLEXtvDZQweWA.png)

1. 3 Specify what you want to collect with the Agent

![](https://cdn-images-1.medium.com/max/1200/1*gOpnGb0ja1XuwnrVOa4sTg.png)

1. 4 If you want to monitor logs from files, you can also specify the location of the log.

![](https://cdn-images-1.medium.com/max/1200/1*7RWKGclmZo4GKHUAdiCMRQ.png)

![](https://cdn-images-1.medium.com/max/1200/1*n_i8A0VAKwdSUkUkQCCUkA.png)

Select the Log Group destination and press Create.

1. 5 Ensure that Logging&Monitoring agent is enabled on the monitored hosts

![](https://cdn-images-1.medium.com/max/1200/1*hQ4Jr5sa3dSWuPABHCT8dA.png)

After a few minutes go to one instance where you have the Custom Logging enabled and check if the logs are there.

![](https://cdn-images-1.medium.com/max/1200/1*5JtxZl_TbtecH2eLzSx3Xg.png)

You can also check the Logs in OCI Logging Service:

![](https://cdn-images-1.medium.com/max/1200/1*Y3oEZ7mhO0lRhqXA9svx5w.png)

![](https://cdn-images-1.medium.com/max/1200/1*gWErPH7efAgqOZZLs3JMjA.png)

On my next blog entry, I will show you how to use the collected Windows logs with Logging Analytics to do a basic Threat Hunting.
