# Frankenstein Automation Gateway Test

##### Table of Contents  
[Headers](#headers)  
[Emphasis](#emphasis) 

## Introduction:
Several months ago I found the [Frankenstein Automation Gateway](https://github.com/vogler75/automation-gateway) and finally got around to testing it out. This gateway allows you to access data from an OPC UA server via MQTT and GraphQL.

Prior to implementing this gateway, I had very little experience working with APIs. Implementing this gateway was a great way to learn about APIs and it helped me learn more about OPCUA.

Below are the steps that I took to implement the Frankenstein Automation Gateway:

## What is the Frankenstein Automation Gateway:

"An OPC UA gateway which gives you access to your OPC UA values via MQTT or GraphQL (HTTP). If you have an OPC UA server in your PLC, or a SCADA system with an OPC UA server, you can query data from there via MQTT and GraphQL (HTTP). In addition, the gateway can also log value changes from OPC UA nodes in an InfluxDB, IoTDB, Kafka, and others."
- Excerpt from the [Frankenstein Automation Gateway GitHub Page](https://github.com/vogler75/automation-gateway) 

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

```
Opc/opc1/node:value/ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].iDeviceState
```

- Initially, I had a hard time determining the topic path. I found it very helpful to use [UaExpert](https://www.unified-automation.com/downloads/opc-ua-clients.html) to determine the correct NodeID
  #### 1. Browse to the data point:
  
  <kbd> <img src= "https://user-images.githubusercontent.com/48938478/138560344-276bec60-8600-493f-852f-674d9affc82a.png" /> </kbd>
  
  #### 2. Copy the nodeid:
  
  <kbd> <img src= "https://user-images.githubusercontent.com/48938478/138560375-af96ef61-bc35-415f-8f7f-96402d9d92be.png" /> </kbd>
  
  ```
  ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].iDeviceState
  ```
  
  #### 3. Append the nodeid to the following MQTT topic:
  
  `Opc/opc1/node:value/`
  
  - The topic path for my exambple is below:
  
  ```
  Opc/opc1/node:value/ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].iDeviceState
  ```

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

  - The Documentation Explorer is a helpful resource for determining how to access the data via GraphQL:

    <kbd> <img src= "https://user-images.githubusercontent.com/48938478/138581304-f662dfcc-79c7-4ebf-a7f6-5db3df06910f.png" /> </kbd>
    
  - Below is an example Query for a single NodeValue:

    ```
    DeviceState: NodeValue(
    Type: Opc,
    System: "opc1",
    NodeId: "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].iDeviceState"
    ) {
    Value
    }
    ```
    
  - Below is the return from the single NodeValue query:
    
    ```
    {
      "data": {
        "DeviceState": {
          "Value": "1"
        }
      }
    }
    ```
    
  - Below is an example Query for multiple NodeValues:

    ```
    {
      NodeValues(
        Type: Opc,
        System: "opc1",
        NodeIds: [
          "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].sDeviceName",
          "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].rRequested_Hz",
          "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].rActual_Hz",
          "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].rMotorCurrent",
          "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].iDeviceState"
        ]
      ) {
        NodeId
        SourceTime
        Value
      }
    }
    ```
    
  - Below is the return from the multiple NodeValues query:
    
    ```
    {
      "data": {
        "NodeValues": [
          {
            "NodeId": "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].sDeviceName",
            "SourceTime": "2021-10-24T05:08:57Z",
            "Value": "VFD-1"
          },
          {
            "NodeId": "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].rRequested_Hz",
            "SourceTime": "2021-10-24T05:08:57Z",
            "Value": "60.0"
          },
          {
            "NodeId": "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].rActual_Hz",
            "SourceTime": "2021-10-24T05:08:57Z",
            "Value": "8.199995"
          },
          {
            "NodeId": "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].rMotorCurrent",
            "SourceTime": "2021-10-24T05:08:57Z",
            "Value": "9.8"
          },
          {
            "NodeId": "ns=4;s=|var|c300.Application.GVL_AutomationGateway.g_arrDeviceData[1].iDeviceState",
            "SourceTime": "2021-10-24T05:08:57Z",
            "Value": "1"
          }
        ]
      }
    }
    ```

## Summary:
  - Overall, I am very impressed with how this Automation Gateway allows me to access an OPC UA Server via MQTT and GraphQL
  - The configuration file is quite easy to setup for docker
  - The developer was very accessible to assist me when I had questions
  - This gateway helped develop my API/GraphQL skills
  - This gateway helped me learn more about OPC UA.
    - I needed to learn how the OPC UA address space works and how to access the data I needed
