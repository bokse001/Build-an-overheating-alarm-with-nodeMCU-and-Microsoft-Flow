# Build an overheating alarm with nodeMCU and Microsoft Flow

Workshop documentation on building your own nodeMCU based overheating alarm using Microsoft flow.

## Connect hardware on breadboard

[![Connecting the hardware](https://github.com/bokse001/Build-you-own-weather-station/raw/master/images/nodemcu%20weatherstation_bb.jpg)](https://github.com/bokse001/Build-you-own-weather-station/blob/master/images/nodemcu%20weatherstation_bb.jpg)

- DHT pin 1 to 3,3V
- DHT pin 2 to D4
- DHT pin 4 to GND
- Resistor to 3,3V and DHT pin 2

[![Arduino IDE](https://github.com/bokse001/Build-you-own-weather-station/raw/master/images/IDE.png)](https://github.com/bokse001/Build-you-own-weather-station/blob/master/images/IDE.png)

## Prepare your laptop for ESP development

1. Install the Arduino.cc IDE software and launch the IDE and allow firewall access
2. File > Preferences, check the “Display line numbers option“
3. Add the URL [http://arduino.esp8266.com/stable/package_esp8266com_index.json](http://arduino.esp8266.com/stable/package_esp8266com_index.json) to the “Addition Boards Manager URLs” field and press the “Ok” button

[![Settings](https://github.com/bokse001/Build-you-own-weather-station/raw/master/images/settings.png)](https://github.com/bokse001/Build-you-own-weather-station/blob/master/images/settings.png)

4. Tools > Boards > Boards Manager, install the "esp8266" board package

[![Boards manager](https://github.com/bokse001/Build-you-own-weather-station/raw/master/images/boards.png)](https://github.com/bokse001/Build-you-own-weather-station/blob/master/images/boards.png)

5. Sketch > Include Library > Manage Libraries, install the "DHT sensor library" and the "Adafruit Unified Sensor" libraries

[![Libraries](https://github.com/bokse001/Build-you-own-weather-station/raw/master/images/libraries.png)](https://github.com/bokse001/Build-you-own-weather-station/blob/master/images/libraries.png)

6. Close and re-open Arduino IDE 1. Select the proper board from Tools->Board->Board ”nodeMCU 1.0 (ESP-12E Module)”

When Connectivity to the board does not work:

- Install the CP210x USB to UART driver and make sure you reboot your machine afterward (I suffered strange behaviors using the driver without rebooting) [https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers](https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers)
- After rebooting open the Arduino IDE and select the proper board from Tools->Board->Board ”nodeMCU 1.0 (ESP-12E Module)”

[![Examples](https://github.com/bokse001/Build-you-own-weather-station/raw/master/images/examples.png)](https://github.com/bokse001/Build-you-own-weather-station/blob/master/images/examples.png)

## Test your laptop and nodeMCU setup

- Load the example sketch “Blink” into the Arduino IDE from File->Examples->01 Basics->Blink

[![Blink](https://github.com/bokse001/Build-you-own-weather-station/raw/master/images/blink.png)](https://github.com/bokse001/Build-you-own-weather-station/blob/master/images/blink.png)

\- Try compiling it by pressing the Verify button:

[![Verify](https://github.com/bokse001/Build-you-own-weather-station/raw/master/images/verify.png)](https://github.com/bokse001/Build-you-own-weather-station/blob/master/images/verify.png)

\- When ok, connect the nodeMCU to your laptop: - Connect the nodeMCU with the USB cable - Via Tools -> Port, check if the correct COMxx port is selected - Upload the sketch by pressing the Upload button:

[![Upload](https://github.com/bokse001/Build-you-own-weather-station/raw/master/images/upload.png)](https://github.com/bokse001/Build-you-own-weather-station/blob/master/images/upload.png)

\- Wait till the sketch is compiled and uploaded - The blue led on the nodeMCU should now start blinking

## Test to see if the nodeMCU can measure temperature and humidity

- Load the example sketch “DHTtester” into the Arduino IDE from File->Examples->DHT sensor library->DHTtester
- Try compiling it by pressing the Verify button
- When ok:
  - Connect the nodeMCU with the USB cable
  - Via Tools -> Port, check if the correct COMxx port is selected
  - Upload the sketch by pressing the Upload button:
  - Wait till the sketch is compiled and uploaded
  - Open the monitor Tools->Serial Monitor
- Message with the temperature and humidity should appear within the monitor

## Create a Microsoft flow to send an alarm via email

Microsoft Flow is a brand new SaaS offering, for automating workflows across the growing number of applications and SaaS services that business users rely on. How much time do we spend sifting through streams of messages for the few notifications that matter? How much manual labor is spent transferring information from one place to another, or entering data into tracking systems? Too often these tasks are done manually, or not done at all.

We will pass the telemetry to an email address provided by you.

*Note: in this workshop, you are introduced into Microsoft Flow. See for more information.*

Follow these steps to create an endpoint in Microsoft Flow to send telemetry data to. From there we can use the data in an "If Then Else" flow.

*Note: If you have no account yet, please sign up first (You can sign up for free using the button at the top of the Microsoft Flow portal).*

1. `Log into` the [Microsoft flow portal](https://flow.microsoft.com/). You will be asked to provide Microsoft Flow credentials if needed

![alt tag](https://github.com/bokse001/Build-an-overheating-alarm-with-nodeMCU-and-Microsoft-Flow/raw/master/images/flow_init.png)

1. Select `My flows`

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-my-flows.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-my-flows.png)

2. Normally, all your flows will appear here. Because there is no flow yet, a short introduction is shown

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-my-flows-create.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-my-flows-create.png)

3. Select `Create from blank`

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-create-from-blank.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-create-from-blank.png)

4. An empty flow is shown

5. First give the flow a proper name. Name it `Mail Temperature trigger` (by replacing 'Untitled')

   [![alt tag](https://github.com/bokse001/Build-an-overheating-alarm-with-nodeMCU-and-Microsoft-Flow/raw/master/images/flow_create.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-input-more-with-name.png)

6. You are invited to select one of many flow steps. `Scroll down` until a step called 'Request' is shown

   [![alt tag](https://github.com/bokse001/Build-an-overheating-alarm-with-nodeMCU-and-Microsoft-Flow/raw/master/images/flow_request.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-input-request2.png)

7. Then select the `Request` step. The following fields are shown

   [![alt tag](https://github.com/bokse001/Build-an-overheating-alarm-with-nodeMCU-and-Microsoft-Flow/raw/master/images/flow_request2.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-request-step-init.png)

8. This is an incoming API call and we will use our nodeMCU to trigger this flow. *Note: The URL will be generated after save*

9. The Azure Function will post a Json object to this Request URL. Flow can not handle this Json object directly. Therefore, enter the following 'Request Body JSON Schema'. This is used to transform the Json object into an entity. This way, Microsoft Flow can handle the separate fields in the message

   ```
   {
     "type": "object",
     "properties": {
       "temperature": {
         "type": "integer"
       },
       "deviceId": {
         "type": "string"
       },
       "time": {
         "type": "string"
       }
     },
     "required": [
       "deviceId"
     ]
   }
   ```

10. The Request step is ready now. We will retrieve the URL after creation of the Flow

11. Select `New step`

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-new-step.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-new-step.png)

12. In this flow, we will mail conditionally. So select `Add a condition`

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-add-a-condition.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-add-a-condition.png)

13. This is the heart of the Flow. We provide a condition (like 'Temperature is higher than 24'). And if it's true, a certain step will be executed. Otherwise, the other step will be executed. *Note: The first or the latter are optional*

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-condition-init.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-condition-init.png)

14. Enter the left field with 'Choose a value'. The previous Request step will output an entity with fields like 'deviceId', 'time' and 'temperature'. So here you can compare one of the fields with another value

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-condition-fields.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-condition-fields.png)

15. Select the `temperature` field

16. Because we want to be warned when the temperature is higher than a certain value, select `is greater than` operator

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-condition-less-then.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-condition-less-then.png)

17. Finally, enter `23` in the right field

    [![alt tag](https://github.com/bokse001/Build-an-overheating-alarm-with-nodeMCU-and-Microsoft-Flow/raw/master/images/flow_request3.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-condition-less-then-42.png)

18. We have created a condition, now we can fill in the 'if then else' blocks with steps. In the left block, Change 'IF YES, DO NOTHING' into 'IF YES' by selecting `Add an Action`

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-condition-true-add-action.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-condition-true-add-action.png)

19. Select the `Mail - Send email` step

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-condition-true-add-mail.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-condition-true-add-mail.png)

20. Create a connection for Mail. `Accept` the SendGrid terms and privacy policy *Note: SendGrid is a third party email provider*

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-condition-true-mail-step.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-condition-true-mail-step.png)

21. Enter `your own email address` in the 'To' field

22. Enter `Overheating alarm for device ` plus the entity field 'deviceId' in the 'Subject' field

23. Enter `Hurry up, the temperature just got to` plus the entity field 'temperature' in the 'Email body' field

    [![alt tag](https://github.com/bokse001/Build-an-overheating-alarm-with-nodeMCU-and-Microsoft-Flow/raw/master/images/flow_request4.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-condition-true-mail-step-filled-in.png)

24. This Mail step is ready, and so is the flow. Select `Create flow`

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-create-flow.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-create-flow.png)

25. The flow is now being created. We have to wait a moment to get it starting up. Select `Done`

    [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-flow-creation-done.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-flow-creation-done.png)

Although the flow is created and almost available we still need to do one thing more.

## Getting the API endpoint of the Request step

By this time, the endpoint of the Request step is created. Before we can pass telemetry to this flow, we need to have that API endpoint.

1. Select `My flows`

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-my-flows.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-my-flows.png)

2. All your flows will appear here. The flow we just created is shown

   [![alt tag](https://github.com/bokse001/Build-an-overheating-alarm-with-nodeMCU-and-Microsoft-Flow/raw/master/images/flows_list.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-my-flows-list.png)

3. Edit the flow by clicking on it. 

4. The flow is shown, in detail. Select the first step, the `Request step` so the fields are shown

   ![](https://github.com/bokse001/Build-an-overheating-alarm-with-nodeMCU-and-Microsoft-Flow/raw/master/images/flows_details2_url.png)

5. The URL of the Request step endpoint is now available to copy

6. **Copy and write down** the Url in the `HTTP POST to this URL` field

7. Select `X Close' to close this page. Do *not* save any changes

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-close.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-close.png)

Now we have the URL of the endpoint. We will call it inside the Azure Function.

## Setup the nodeMCU to send the temperature to Flow

- Load the example sketch “HTTPSRequest” into the Arduino IDE from File->Examples->ESP8266Wifi->HTTPSRequest

- Find the line with the *ssid and change it to the ssid given to you in the workshop

- Find the line with the *password and change it to password given to you in the workshop

- Find the line with the *host and change it to the host in the URL from Flow you got before (like prod-08.westeurope.logic.azure.com)

- Below the host line add this declaration to hold the json payload:

- ```
  // data string to hold json payload
  String data;
  ```

- Find the line with url and change it to the remainder of the URL you got from Flow. (like /workflows/40665.....)

- Find the part with the code:

- - ?

           client.print(String("GET ") + url + " HTTP/1.1\r\n" +
           "Host: " + host + "\r\n" +
           "User-Agent: BuildFailureDetectorESP8266\r\n" +
           "Connection: close\r\n\r\n");

- And replace it with:

- 
  ?
        int temperature = 24; 				// Temperature so alarm is triggered
        data = "\{    \"temperature\": ";
        data = data + String(temperature);
        data = data + ",    \"deviceId\": \""; 
        data = data + "my_device";			// your device name
        data = data + "\",    \"time\": \"";
        data = data + "my_time";				// your device time
        data = data + "\"    \}";
      
        Serial.print("Posting data: ");
        Serial.println(data);
      
        client.print(String("POST ") + url + " HTTP/1.1\r\n" +
                     "Host: " + host + "\r\n" +
                     "User-Agent: BuildFailureDetectorESP8266\r\n" +
                     "Content-Length: " + data.length() + "\r\n" +
                     "Content-Type: application/json\r\n" +
                     "\r\n" + 
                     data +
                     "\r\n");

- Try compiling it by pressing the Verify button

- When ok:

  - Connect the nodeMCU with the USB cable
  - Via Tools -> Port, check if the correct COMxx port is selected
  - Upload the sketch by pressing the Upload button
  - Wait till the sketch is compiled and uploaded
  - Open the monitor

- ```
  connecting to <your ssid>
  ...
  WiFi connected
  IP address: 
  192.168.1.30
  connecting to prod-08.westeurope.logic.azure.com
  certificate doesn't match
  requesting URL: /workflows/<your_url>
  Posting data: {    "temperature": 24,    "deviceId": "my_device",    "time": "my_time"    }
  request sent
  headers received
  esp8266/Arduino CI has failed
  reply was:
  ==========

  ==========
  closing connection
  ```


- As you can see above the nodeMCU connects to the wifi router and then opens a connection to the flow server posting the json data.  (There are some errors here as you can see, not important for now, we will go into these further down the line)

## Receiving mail from your device

Microsoft Flow is passing the telemetry to the email address you provided. Microsoft flow provides a list of runs. We can check how our flow is doing.

1. Go to the Microsoft Flow portal

2. Select `My flows`

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-my-flows.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-my-flows.png)

3. All your flows will appear here. The flow we created will be shown

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-my-flows-list.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-my-flows-list.png)

4. Select the run list of the flow by selecting `the i` (for Information) icon

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-portal-run-list.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-portal-run-list.png)

5. By now, one or more runs must be listed

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-mail-runs-list.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-mail-runs-list.png)

6. Select a row and `click' on it

7. The execution of the flow is shown in detail. Green ticks will mark which steps are executed successfully. In the following example, the telemetry is received, the condition is checked but the 'IF NO, DO NOTHING' block is executed (the 'IF YES' block has no green tick)

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-mail-sent-mail-false-condition.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-mail-sent-mail-false-condition.png)

8. But is the condition is right, the level is too low, both steps will have a green tick

   [![alt tag](https://github.com/JeeWeetje/ttn-azure-workshop/raw/master/img/flow-mail-sent-mail-true-condition.png)](https://github.com/JeeWeetje/ttn-azure-workshop/blob/master/img/flow-mail-sent-mail-true-condition.png)

9. Finally, open an email client and check your mail. Did you receive any mail?




## Challenge 1: get live temperature data from the DHT-22 and send it through to flow

Now we are able to measure the temperature from the DHT-22 sensor and send data to Flow it is time to combine the two sketches into one.

Hints:

- Arduino's use 2 standard functions to handle the **setup** of the device and sensors and a **loop** that it runs through indefinitely. 
- Use the **setup** to initialize the DHT-22 and connect to the wifi router
- Use **loop** to retrieve the temperature and send it to Flow
- **Warning**: do not overrun Flow... Use a delay(10000); in your loop so there is a pause of 10 seconds in between your sends...

## Challenge 2: Resolve the SSL certificate error

Within the monitor you have seen this error: certificate doesn't match. This indicates us that the certificate SHA1 fingerprint we set is not the same as the one we receive back from the Flow backend. 

Now the challenge is to resolve this error by checking with the correct fingerprint.

Hints:

- Enable debugging by adding under the #include <WiFiClientSecure.h>:

- ```
  #define DEBUG_SSL
  #define DEBUGV
  ```

- And the following line within setup:

- ```
    Serial.setDebugOutput(true);
  ```

- Get the Flow server certificate by checking the url with your browser and retrieve the SHA1 fingerprint. Put that in the fingerprint variable within the sketch.

## Challenge 3: Get the proper time and pass it on to Flow

Within the sketch we do not have the proper time set and passed on to Flow. Now retrieve the time somehow and use it to send the proper time and date alongside the temperature.

Hints:

- Why not use NTP servers to retrieve the time? (use a NTPclient example?)
- Calculate the date and time and put this within the payload replacing my_time

## Challenge 4: Get the MAC address and pass it on as device id to Flow

Up till now we used my_device as the identifier of the nodeMCU. What if we could use a proper identification which is unique like the MAC address?

Hints:

- Use Google and find example code to retrieve the nodeMCU's MAC address
- Pass the MAC address to Flow replacing my_time
- Example code below>>>>>>> (scroll way down)























































































```
#include <ESP8266WiFi.h>

uint8_t MAC_array[6];
char MAC_char[18];

void setup() {
    Serial.begin(115200);
    Serial.println();

    WiFi.macAddress(MAC_array);
    for (int i = 0; i < sizeof(MAC_array); ++i){
      sprintf(MAC_char,"%s%02x:",MAC_char,MAC_array[i]);
    }
  
    Serial.println(MAC_char);
}

void loop() {
  
}
```
