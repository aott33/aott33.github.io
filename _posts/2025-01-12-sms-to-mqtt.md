# Prerequisites
1. Create a Twilio account
2. Create a virtual phone number

# Create and Deploy a Function
1. Go to [Twilio Console](https://www.twilio.com/console/functions/overview)
3. [Create a Service](https://www.twilio.com/console/functions/overview/services), fill name field, and click **Next**
  ![image](https://github.com/user-attachments/assets/f862a478-0f86-4790-89c4-a2166ed07bd2)
  ![image](https://github.com/user-attachments/assets/60e5548c-d44f-4443-81bf-e660ecc2d79f)
4. When redirected to the new Service, click the **Add +** button and select **Add Function** from the dropdown and update function name
  ![image](https://github.com/user-attachments/assets/07458e62-e402-4d1b-8cef-6729f6a66b14)
  ![image](https://github.com/user-attachments/assets/39cd2618-a41b-406c-8b6f-9557b02c648b)
5. Click **Dependencies** to import mqtt npm module, fill in the fields and click **Add** (see image)
  ![image](https://github.com/user-attachments/assets/7dee8341-0567-41f5-b95a-89d656d098f6)
6. Navigate to function and update script, click **Save**, then click **Deploy All**. Console will update when the process is complete. Below the screenshot is a sample script that takes the body of the incoming SMS message and publishes it to the HiveMQ test broker `mqtt://broker.hivemq.com`
  ![image](https://github.com/user-attachments/assets/1ca1f9a3-0b92-469d-852e-37a91fad1713)

## Sample Script
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

# Set a Function as a webhook


# Resources
* [Twilio Console](https://www.twilio.com/console/functions/overview)
* [Receive an inbound SMS example](https://www.twilio.com/docs/serverless/functions-assets/quickstart/receive-sms)
* [The Ultimate Guide on How to Use MQTT with Node.js](https://www.hivemq.com/blog/ultimate-guide-on-how-to-use-mqtt-with-node-js/)
