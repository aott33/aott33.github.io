# Prerequisites
1. Create a Twilio account
2. Create a virtual phone number

# Create and Deploy a Function
1. Go to [Twilio Console](https://www.twilio.com/console/functions/overview)
3. [Create a Service](https://www.twilio.com/console/functions/overview/services), fill name field, and click **Next**
  ![image|690x225, 75%](upload://pSNch02KMY3BKsf87Zw1UI5kjdA.png)
  ![image|690x278, 75%](upload://wFBUNV5V7XbIKH3bQ4F2zAorPqe.png)
4. When redirected to the new Service, click the **Add +** button and select **Add Function** from the dropdown and update function name
  ![image|690x388, 75%](upload://zGDFLd8qg1H9WlMxYPcVDluLcZ.png)
  ![image|690x384, 75%](upload://z2R3CZxXr9W5dfrv4AVNYGv4iiV.png)
5. Click **Dependencies** to import mqtt npm module, fill in the fields and click **Add** (see image)
  ![image|689x391, 75%](upload://gjBZyXYta9nATZ5n8ePPucX3qCm.png)
6. Navigate to function and update script, click **Save**, then click **Deploy All**. Console will update when the process is complete. Below the screenshot is a sample script that takes the body of the incoming SMS message and publishes it to the HiveMQ test broker `mqtt://broker.hivemq.com`
![image|690x378, 75%](upload://gSnEaL31uxIqK2gH6XziDQsuvGz.png)

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
