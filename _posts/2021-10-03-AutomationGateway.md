# Frankenstein Automation Gateway Test

## Introduction:
Several months ago I found the [Frankenstein Automation Gateway](https://github.com/vogler75/automation-gateway) and finally got around to testing it out. This gateway allows you to access data from an OPC UA server via MQTT and GraphQL.

Prior to implementing this gateway, I had very little experience working with APIs. Implementing this gateway was a great way to learn about APIs and it helped me learn more about OPCUA.

Below are the steps that I took to implement the Frankenstein Automation Gateway:

## Steps taken:
### 1. Go to [Frankenstein Automation Gateway](https://github.com/vogler75/automation-gateway)
- Review the documentation

### 2. Install [Docker Desktop](https://docs.docker.com/desktop/)
- I am using the [Docker Desktop for Windows](https://docs.docker.com/desktop/windows/install/)

### 3. Go to the [rocworks/automation-gateway Docker Page](https://hub.docker.com/r/rocworks/automation-gateway) 
- Copy the Docker Pull Command or copy below

```
docker pull rocworks/automation-gateway
```

### 4. Create a `config.yaml` file and add the needed configurations
- See the [rocworks/automation-gateway Docker Page](https://hub.docker.com/r/rocworks/automation-gateway) for an example
- Below is the `config.yaml` file that I created:

```
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
    EndpointUrl: "opc.tcp://192.168.1.250:4840"
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

### 5. Run Docker Desktop 
### 6. Open command prompt and call the docker run command 
- Ensure that you have navigated to the folder where you saved the config.yaml file
- ***`%PWD` in the [Docker Hub example](https://hub.docker.com/r/rocworks/automation-gateway) will not work on [Windows](https://docs.docker.com/desktop/windows/troubleshoot/#path-conversion-on-windows), use `%cd%` instead. See example below:***

```
docker run -p 4000:4000 -p 1883:1883 -v %cd%\config.yaml:/app/config.yaml rocworks/automation-gateway
```

### 7. Accessing OPC UA data via MQTT 
- ***MQTT.fx 1.7.1 subscribe did not work for me, I changed to MQTT Explorer and it worked***
- See MQTT example topics from [vogler75's github page](https://github.com/vogler75/automation-gateway#example-topics)
- Using nodeid to get the value of the node:
  - `Opc/opc1/node:value/ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].iDeviceState`
- Initially, I had a hard time determining the topic path. I found it very helpful to use [UaExpert](https://www.unified-automation.com/downloads/opc-ua-clients.html) to determine the correct NodeID
  #### 1. Browse to the data point:
  
  ![image](https://user-images.githubusercontent.com/48938478/138560344-276bec60-8600-493f-852f-674d9affc82a.png)
  
  #### 2. Copy the nodeid:
  
  ![image](https://user-images.githubusercontent.com/48938478/138560375-af96ef61-bc35-415f-8f7f-96402d9d92be.png)
  
  `ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].iDeviceState`
  
  #### 3. Append the nodeid to the following MQTT topic:
  `Opc/opc1/node:value/` + `ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].iDeviceState`

### 8. Accessing OPC UA data via GraphQL:
  - Similar method as accessing the OPC UA data via MQTT
  - I used [UaExpert](https://www.unified-automation.com/downloads/opc-ua-clients.html) to determine the NodeId
  - I used GraphiQL to query the GraphQL server. Accessed via the following URL:
    - `http://localhost:4000/graphiql/`
    - Esure that the `GraphQLServer` section in the `config.yaml` file has `GraphiQL: true` in. See below:
   
    ```
    GraphQLServer:
      Listeners:
      Port: 4000
      LogLevel: INFO
      GraphiQL: true 
    ```

  - The Documentation Explorer is a helpful resource for determining how to access the data via GraphQL
  - Below is an example Query:
  - Below is the 

## Summary:
  - Need to write up a summary
