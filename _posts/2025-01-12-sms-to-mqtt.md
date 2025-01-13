# SMS Alarm to Ignition
## Introduction
This is a two part blog series walking through the steps to bring an sms alarm into Ignition and trigger an alarm. Part 1 (this post) will walk through the steps to get the incoming SMS into a broker using Twilio. Part 2 will walk through the steps to configure Ignition to read the message and trigger an alarm.

## Steps
### Prerequisites
1. Create a Twilio account
2. Create a virtual phone number

### Create and Deploy a Function
1. Go to [Twilio Console](https://www.twilio.com/console/functions/overview)
2. [Create a Service](https://www.twilio.com/console/functions/overview/services), fill name field, and click **Next**
  ![image](https://github.com/user-attachments/assets/f862a478-0f86-4790-89c4-a2166ed07bd2)
  ![image](https://github.com/user-attachments/assets/60e5548c-d44f-4443-81bf-e660ecc2d79f)
3. When redirected to the new Service, click the **Add +** button and select **Add Function** from the dropdown and update function name
  ![image](https://github.com/user-attachments/assets/07458e62-e402-4d1b-8cef-6729f6a66b14)
  ![image](https://github.com/user-attachments/assets/39cd2618-a41b-406c-8b6f-9557b02c648b)
4. Click **Dependencies** to import mqtt npm module, fill in the fields and click **Add** (see image)
  ![image](https://github.com/user-attachments/assets/7dee8341-0567-41f5-b95a-89d656d098f6)
5. Navigate to function and update script, click **Save**, then click **Deploy All**. Console will update when the process is complete. Below the screenshot is a sample script that takes the body of the incoming SMS message and publishes it to the [HiveMQ Public Broker]([url](https://www.hivemq.com/mqtt/public-mqtt-broker/)) `mqtt://broker.hivemq.com` 
  ![image](https://github.com/user-attachments/assets/1ca1f9a3-0b92-469d-852e-37a91fad1713)

#### Sample Script
```
const mqtt = require('mqtt');

exports.handler = (context, event, callback) => {

  const incomingMessage = event.Body;

  const client = mqtt.connect('mqtt://broker.hivemq.com');

  client.on('connect', function () {
    console.log('Connected')
      // Publish a message to a topic
      client.publish('sms2mqtt/messages', incomingMessage);
      return callback();
  })
	console.log(incomingMessage);
};
```

### Set a Function as a webhook
Source: [Set a Function as a webhook](https://www.twilio.com/docs/serverless/functions-assets/quickstart/receive-sms#set-a-function-as-a-webhook)
1. Twilio Console's [Phone Numbers page](https://www.twilio.com/console/phone-numbers/incoming)
2. Click on the phone number you'd like to have connected to your Function
3. Navigate to **A Message Comes In option** under **Messaging**
4. Select **Function** from the **A Message Comes In**
5. Select the **Service** that you are using, then the **Environment** (this will default to ui unless you have created custom domains), and finally **Function Path** of your Function from the respective dropdown menus
   ![image](https://github.com/user-attachments/assets/16e80ef1-0675-4268-bd9b-b529d5bbeb8e)

### Testing using HiveMQ Websocket Client
1. Navigate to [HiveMQ Websocket Client](https://www.hivemq.com/demos/websocket-client/)
2. Fill in the **Host** and **Port** settings then click **Connect**. I am using `broker.hivemq.com` and `8884` for testing purposes only.
   ![image](https://github.com/user-attachments/assets/dc009228-c312-4b15-bc69-bfe7f1b6911a)
3. Click **Add New Topic Subscription**
   ![image](https://github.com/user-attachments/assets/124f1e99-ba5a-43d8-a53b-295b9882a704)
4. Add the same topic that is used in the Twilio Function and clik **Subscribe**. I added `sms2mqtt/messages/#`
   ![image](https://github.com/user-attachments/assets/886dbc51-dd0e-4114-8fcd-bca3e14dd406)
5. Send a test text message to the Twilio Phone Number
6. Monitor the Web Client and you should see a message come in for the subscribed topic
   ![image](https://github.com/user-attachments/assets/f551b8ad-5f89-46ea-b74c-db628e11048b)

## Resources
* [Twilio Console](https://www.twilio.com/console/functions/overview)
* [Receive an inbound SMS example](https://www.twilio.com/docs/serverless/functions-assets/quickstart/receive-sms)
* [The Ultimate Guide on How to Use MQTT with Node.js](https://www.hivemq.com/blog/ultimate-guide-on-how-to-use-mqtt-with-node-js/)
