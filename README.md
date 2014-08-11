Consul Client for Java
======================

Simple client for the Consul HTTP API.  For more information about the Consul HTTP API, go [here](http://www.consul.io/docs/agent/http.html).

Installation
-----------

TBD.

Basic Usage
-----------

Example 1: Register and check your service in with Consul.  Note that you need to continually check in before the TTL expires, otherwise your service's state will be marked as "critical".

```java
Consul consul = Consul.newClient(); // connect to Consul on localhost
AgentClient agentClient = consul.agentClient();

String serviceName = "MyService";
String serviceId = "1";

agentClient.register(8080, 3L, serviceName, serviceId); // registers with a TTL of 3 seconds
agentClient.pass(); // check in with Consul
```

Example 2: Find available (healthy) services.

```java
Consul consul = Consul.newClient(); // connect to Consul on localhost
HealthClient healthClient = consul.healthClient();

<List<ServiceHealth> nodes = healthClient.getHealthyNodes("DataService").getResponse(); // discover only "passing" nodes
```

Example 3: Store key/values.

```java
Consul consul = Consul.newClient(); // connect to Consul on localhost
KeyValueClient kvClient = consul.keyValueClient();

keyValueClient.putValue("foo", "bar");

String value = keyValueClient.getValueAsString("foo").get(); // bar
```

Example 4: Blocking call for value.

```java
import static com.orbitz.consul.option.QueryOptionsBuilder;

Consul consul = Consul.newClient();
KeyValueClient kvClient = consul.keyValueClient();

keyValueClient.putValue("foo", "bar");

String value = keyValueClient.getValue("foo", builder().blockMinutes(10, 120).build()).get(); // will block (long poll) for 10 minutes or until "foo"'s value changes.
```