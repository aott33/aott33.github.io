# Testing out the Frankenstein Automation Gateway

Several months ago I found the [Frankenstein Automation Gateway](https://github.com/vogler75/automation-gateway) and finally got around to testing it out. This gateway allows you to access data from an OPC UA server PLC via MQTT and GraphQL.

Prior to implementing this gateway, I had very little experience working with APIs. So, implementing this gateway was a great way to learn about APIs, specifically, GraphQL. See the [Resources Page](/Resources.md#apis) for helpful API resources.

## Steps taken:
1. Go to [Frankenstein Automation Gateway](https://github.com/vogler75/automation-gateway) and review the documentation 
2. Pull the [Docker image](https://hub.docker.com/r/rocworks/automation-gateway)

```
docker pull rocworks/automation-gateway
```

  - Note: I am using the [Docker Desktop for Windows](https://docs.docker.com/desktop/windows/install/)

3. Create a `config.yaml` file and add the needed configurations
  - See the [rocworks/automation-gateway docker page](https://hub.docker.com/r/rocworks/automation-gateway) for an example. I added the following:

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

4. Open Docker Desktop 
5. Open Command Prompt and type the following docker run command (Change 'filepath' to the filepath of the config.yaml file)

```
docker run -p 4000:4000 -p 1883:1883 -v 'filepath'\config.yaml:/app/config.yaml rocworks/automation-gateway
```

  - Note: I learned that the `%PWD` in the [Docker Hub example](https://hub.docker.com/r/rocworks/automation-gateway) will not work on [Windows](https://docs.docker.com/desktop/windows/troubleshoot/#path-conversion-on-windows)

6. Accessing OPC UA data via MQTT (MQTT.fx 1.7.1 subscribe did not work for me, I changed to MQTT Explorer and it worked as it should):
- Need to add images and topic name examples

7. Accessing OPC UA data via GraphQL:
- Need to add images and query examples
