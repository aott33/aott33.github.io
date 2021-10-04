# Testing out the Frankenstein Automation Gateway

Several months ago I found the [Frankenstein Automation Gateway](https://github.com/vogler75/automation-gateway) and thought it looked pretty interesting but I was a bit intimated to test it. Several months later and I am a little more confident and decided to test it out this weekend and turned out it was a great way to access my OPC UA server via MQTT and GraphQL.

Prior to this weekend, I had very little experience working with APIs. So, testing out this gateway was a great way to learn about APIs, specifically, GraphQL. See the [Resources Page](/Resources.md#apis) for helpful API resources.

## Steps taken:
* Go to [Frankenstein Automation Gateway](https://github.com/vogler75/automation-gateway) and review the documentation 
* Pull the [Docker image](https://hub.docker.com/r/rocworks/automation-gateway)

```
docker pull rocworks/automation-gateway
```

Note: I am using the [Docker Desktop for Windows](https://docs.docker.com/desktop/windows/install/)

* Create a `config.yaml` file and add the needed configurations
  * See the [rocworks/automation-gateway docker page](https://hub.docker.com/r/rocworks/automation-gateway) for an example. I added the following:

```yaml
MqttServer:
  Listeners:
    - Id: Mqtt
      Port: 1883
      Host: 0.0.0.0
      LogLevel: INFO # ALL | INFO

GraphQLServer:
  Listeners:
    - Port: 4000
      LogLevel: INFO
      GraphiQL: true

OpcUaClient:
  - Id: "opc1"
    Enabled: true
    LogLevel: INFO
    EndpointUrl: "opc.tcp://IPaddress:4840"
    UpdateEndpointUrl: true
    SecurityPolicyUri: http://opcfoundation.org/UA/SecurityPolicy#None
    ConnectTimeout: 5000
    RequestTimeout: 60000
    KeepAliveFailuresAllowed: 1
    SubscriptionSamplingInterval: 0.0
    WriteParameters:
      QueueSize: 10000
      BlockSize: 200
      WithTime: false
    MonitoringParameters:
      BufferSize: 10
      SamplingInterval: 0
      DiscardOldest: true
      DataChangeTrigger: StatusValue # Status | StatusValue | StatusValueTimestamp
```

* Open Docker Desktop 
* Open Command Prompt and type the following docker run command 

```
docker run -p 4000:4000 -p 1883:1883 -v 'Enter file path here'\config.yaml:/app/config.yaml rocworks/automation-gateway
```

Note: I learned that the `%PWD` in the [Docker Hub example](https://hub.docker.com/r/rocworks/automation-gateway) will not work on [Windows](https://docs.docker.com/desktop/windows/troubleshoot/#path-conversion-on-windows)


