Advanced Experiments
====================

.. image:: _static/project/IOT/1.WEB.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

While foundational experiments taught you the basics of hands-on hardware interaction, this chapter introduces you to network connectivity. Centered on the Internet of Things (IoT), we will guide you in equipping your development board with Wi-Fi capabilities and implementing remote control via a web interface. Covering everything from TCP/IP fundamentals and HTTP request parsing to HTML page design and real-time status feedback, you will build a complete web-based control system—enabling your sensors and actuators to transcend physical distance and advance to a new level of smart connectivity.

----

1. TEMP And HUMI Meter
----------------------

This experiment is a core project in our introductory practical course on the Internet of Things (IoT). It aims to teach you how to set up an ESP32 as a Wi-Fi hotspot (AP mode) and build an embedded web server to display sensor data in real time on a webpage. You will master the following key skills:

 - DHT11 Temperature and Humidity Sensor Driver and Data Reading.

 - ESP32 Soft-AP Mode Configuration: Direct Device Connection Without Router.

 - WebServer Library for HTTP Server Construction and GET Request Handling.

 - JSON Data Assembly and Parsing for Front-End and Back-End Data Interaction.

 - AJAX Asynchronous Refresh Technology **(fetch + setInterval)** : Automatic Webpage Data Updates Without Manual Page Refresh.

 - Front-End UI Design: Responsive Card-Style Dashboard Adapted for Mobile and Desktop Screens.

**Materials Needed:**

 - ESP32 Development Board
 - DHT11
 - Breadboard and Jumper Wires

**Wiring Diagram:**

.. image:: _static/project/IOT/1.DHT11.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Wiring Table**

.. list-table:: 
   :header-rows: 1
   :widths: 10 20 20 25

   * - No.
     - Component
     - Pin
     - Connect to
   * - 1
     - DHT11 Sensor
     - VCC
     - 3.3V
   * - 1
     - DHT11 Sensor
     - GND
     - GND
   * - 1
     - DHT11 Sensor
     - DATA
     - GPIO 15

**Example code:**

.. raw:: html

   <div style="background: #f8f9fa; border: 1px solid #ddd; border-radius: 6px; overflow: hidden;">
   <div id="code-container-dht" style="max-height: 420px; overflow: hidden; position: relative; background: #f5f5f0;">

.. code-block:: cpp

 #include <WiFi.h>
 #include <WebServer.h>
 #include <DHT.h>
 #define DHTPIN 15
 #define DHTTYPE DHT11

 DHT dht(DHTPIN, DHTTYPE);

 const char* ap_ssid = "ESP32-DHT11";

 WebServer server(80);

 String getHTML()
 {
     String html = R"rawliteral(

 <!DOCTYPE html>
 <html lang="en">

 <head>

 <meta charset="UTF-8">

 <meta name="viewport"
 content="width=device-width, initial-scale=1.0">

 <title>ESP32 Climate Monitor</title>

 <style>

 *{
     margin:0;
     padding:0;
     box-sizing:border-box;
 }

 body{
     font-family:Arial, Helvetica, sans-serif;
     background:#f5f7fb;
     color:#222;
     height:100vh;
     display:flex;
     justify-content:center;
     align-items:center;
 }

 .container{
     width:360px;
     background:white;
     border-radius:28px;
     padding:30px;
     box-shadow:
     0 10px 30px rgba(0,0,0,0.08);
 }

 .header{
     display:flex;
     justify-content:space-between;
     align-items:center;
     margin-bottom:25px;
 }

 .title{
     font-size:28px;
     font-weight:700;
     color:#111;
 }

 .status{
     width:12px;
     height:12px;
     background:#4cd964;
     border-radius:50%;
     box-shadow:0 0 10px #4cd964;
 }

 .subtitle{
     font-size:14px;
     color:#888;
     margin-top:5px;
     margin-bottom:25px;
 }

 .card{
     background:#f8fafc;
     border-radius:24px;
     padding:22px;
     margin-bottom:18px;
     transition:0.3s;
 }

 .card:hover{
     transform:translateY(-3px);
     box-shadow:
     0 8px 20px rgba(0,0,0,0.06);
 }

 .label{
     font-size:16px;
     color:#666;
 }

 .value{
     margin-top:10px;
     font-size:48px;
     font-weight:700;
     color:#111;
 }

 .unit{
     font-size:20px;
     color:#666;
 }

 .footer{
     text-align:center;
     margin-top:18px;
     font-size:13px;
     color:#999;
 }

 </style>

 </head>

 <body>

 <div class="container">

 <div class="header">

 <div>

 <div class="title">
 ESP32 Monitor
 </div>

 <div class="subtitle">
 Real-time Climate Dashboard
 </div>

 </div>

 <div class="status"></div>

 </div>

 <div class="card">

 <div class="label">
 🌡 Temperature
 </div>

 <div class="value">
 <span id="temp">--</span>
 <span class="unit">°C</span>
 </div>

 </div>

 <div class="card">

 <div class="label">
 💧 Humidity
 </div>

 <div class="value">
 <span id="hum">--</span>
 <span class="unit">%</span>
 </div>

 </div>

 <div class="footer">
 ESP32 DHT11 Web Monitor
 </div>

 </div>

 <script>

 function updateData()
 {
     fetch('/data')
     .then(response => response.json())
     .then(data => {

         document.getElementById('temp').innerHTML =
         data.temperature;

         document.getElementById('hum').innerHTML =
         data.humidity;
     });
 }

 updateData();

 setInterval(updateData, 2000);

 </script>
 </body>
 </html>

 )rawliteral";

     return html;
 }

 // Main page
 void handleRoot()
 {
     server.send(200, "text/html", getHTML());
 }

 // Sensor data API
 void handleData()
 {
     float humidity = dht.readHumidity();

     float temperature = dht.readTemperature();

     if (isnan(humidity) || isnan(temperature))
     {
         server.send(
             200,
             "application/json",
             "{\"temperature\":\"--\",\"humidity\":\"--\"}"
         );

         return;
     }

     String json = "{";

     json += "\"temperature\":\"" +
             String(temperature,1) + "\",";

     json += "\"humidity\":\"" +
             String(humidity,1) + "\"";

     json += "}";

     server.send(200, "application/json", json);
 }

 void setup()
 {
     Serial.begin(115200);

     dht.begin();

     // Start WiFi hotspot
     WiFi.softAP(ap_ssid);

     IPAddress IP = WiFi.softAPIP();

     Serial.println();
     Serial.println("ESP32 Hotspot Started");

     Serial.print("SSID: ");
     Serial.println(ap_ssid);

     Serial.print("IP Address: ");
     Serial.println(IP);

     // Web routes
     server.on("/", handleRoot);

     server.on("/data", handleData);

     // Start web server
     server.begin();

     Serial.println("Web Server Started");
 }

 void loop()
 {
     server.handleClient();
 }

.. raw:: html

   </div>
   <div style="display: flex; gap: 10px; padding: 12px 16px; background: #fff; border-top: 1px solid #ddd;">
     <button id="expand-btn-dht" onclick="toggleCode('code-container-dht', 'expand-btn-dht')" style="flex: 1; padding: 10px 16px; background: #2980B9; color: white; border: none; border-radius: 4px; cursor: pointer; font-weight: bold;">▼ Expand All Code</button>
   </div>
   </div>

   <style>
   #code-container-dht { transition: max-height 0.4s ease-in-out; }
   </style>

   <script>
   function toggleCode(containerId, buttonId) {
     const container = document.getElementById(containerId);
     const btn = document.getElementById(buttonId);
     if (container.style.maxHeight === '420px' || container.style.maxHeight === '') {
       container.style.maxHeight = 'none';
       btn.textContent = '✕ Collapse Code';
     } else {
       container.style.maxHeight = '420px';
       btn.textContent = '▼ Expand All Code';
     }
   }
   </script>

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Display Effect:**

.. image:: _static/project/IOT/1.dht112.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- The system will automatically create a Wi-Fi hotspot named **ESP32-DHT11**. 

- After connecting to this Wi-Fi network using your mobile phone or computer, enter the IP address **192.168.4.1** in your browser 

- To open a beautifully designed temperature and humidity monitoring panel to view real-time temperature and humidity data.

----

2. Ultrasonic Distance Meter
----------------------------

This experiment is an advanced project for IoT sensor applications, aiming to learn how to combine an ultrasonic ranging module (HC-SR04) with an ESP32 web server to build a real-time wireless ranging and monitoring system. You will master the following key skills:

- Driving principle and ranging implementation of the HC-SR04 ultrasonic sensor ( **pulseIn()** for precise echo time measurement) .

- Temperature-compensated ranging algorithm: Calculates the actual distance using the speed of sound (0.0343 cm/μs) and handles invalid data (out of range, no echo, etc.)

- Timed sampling mechanism: Uses a **millis()** non-blocking timer to collect data at fixed intervals (100ms) to maintain smooth system response

- Web server and JSON API design: Returns structured data through the /data interface, achieving complete separation of front-end and back-end

- AJAX real-time data refresh: The front-end automatically requests the latest data every 300ms, updating the page without page refresh

- Responsive UI design and visual feedback: Distance value animation, status prompts, threshold alarms (buzzer trigger + page warning for <20cm)

- Buzzer linkage control: The buzzer automatically sounds an alarm when an object gets too close, achieving a closed loop of "perception-judgment-execution".

**Materials Needed:**

 - ESP32 Development Board
 - HC-SR04 Ultrasonic Sensor
 - Active Buzzer
 - Breadboard and Jumper Wires

**Wiring Diagram:**

.. image:: _static/project/IOT/2.hcsr04.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Wiring Table**

.. list-table:: 
   :header-rows: 1
   :widths: 10 20 20 25

   * - No.
     - Component
     - Pin
     - Connect to
   * - 1
     - HC-SR04 Ultrasonic
     - VCC
     - 5V
   * - 1
     - HC-SR04 Ultrasonic
     - GND
     - GND
   * - 1
     - HC-SR04 Ultrasonic
     - TRIG
     - GPIO 5
   * - 1
     - HC-SR04 Ultrasonic
     - ECHO
     - GPIO 18
   * - 2
     - Buzzer
     - Positive (+)
     - GPIO 4
   * - 2
     - Buzzer
     - Negative (-)
     - GND


**Example code:**

.. raw:: html

   <div style="background: #f8f9fa; border: 1px solid #ddd; border-radius: 6px; overflow: hidden;">
   <div id="code-container-hcsr04" style="max-height: 420px; overflow: hidden; position: relative; background: #f5f5f0;">

.. code-block:: cpp

 #include <WiFi.h>
 #include <WebServer.h>

 // WiFi hotspot
 const char* ssid = "ESP32-Distance-Meter";

 // Ultrasonic pins
 #define TRIG_PIN 5
 #define ECHO_PIN 18

 // Buzzer pin
 #define BUZZER_PIN 4

 WebServer server(80);

 // Distance variables
 float distance_cm = 0.0;

 unsigned long lastMeasurement = 0;

 const unsigned long MEASURE_INTERVAL = 100;

 bool measurementError = false;

 // Measure distance
 float measureDistance()
 {
     digitalWrite(TRIG_PIN, LOW);
     delayMicroseconds(2);

     digitalWrite(TRIG_PIN, HIGH);
     delayMicroseconds(10);

     digitalWrite(TRIG_PIN, LOW);

     unsigned long duration =
     pulseIn(ECHO_PIN, HIGH, 30000);

     if(duration == 0)
     {
         return -1.0;
     }

     float distance =
     duration * 0.0343 / 2;

     if(distance > 400.0 || distance < 2.0)
     {
         return -1.0;
     }

     return distance;
 }

 // HTML page
 const char* htmlPage = R"rawliteral(

 <!DOCTYPE html>
 <html lang="en">

 <head>

 <meta charset="UTF-8">

 <meta name="viewport"
 content="width=device-width, initial-scale=1.0">

 <title>Distance Meter</title>

 <style>

 *{
     margin:0;
     padding:0;
     box-sizing:border-box;
 }

 body{
     font-family:'Courier New', monospace;
     background:#f5f7fb;
     min-height:100vh;
     display:flex;
     justify-content:center;
     align-items:center;
 }

 .container{

     background:white;

     padding:50px;

     border-radius:32px;

     border:1px solid #e5e7eb;

     box-shadow:
     0 10px 30px rgba(0,0,0,0.06);

     text-align:center;
 }

 .title{

     font-size:20px;

     color:#666;

     margin-bottom:20px;

     letter-spacing:2px;
 }

 .distance{

     display:flex;

     justify-content:center;

     align-items:flex-end;

     flex-wrap:nowrap;

     white-space:nowrap;

     line-height:1;
 }

 .value{

     font-size:180px;

     font-weight:bold;

     color:#111;

     font-variant-numeric: tabular-nums;
 }

 .unit{

     font-size:42px;

     color:#666;

     margin-left:10px;

     margin-bottom:18px;
 }

 .status{

     margin-top:20px;

     font-size:14px;

     color:#888;
 }

 .alert{

     color:#ff3b30;

     font-weight:bold;
 }

 @keyframes blink{

     0%{opacity:1;}

     50%{opacity:0.6;}

     100%{opacity:1;}
 }

 .update{
     animation:blink 0.2s ease;
 }

 .footer{

     margin-top:25px;

     font-size:13px;

     color:#999;
 }

 @media (max-width:600px){

     .container{
         padding:35px;
     }

     .value{
         font-size:90px;
     }

     .unit{
         font-size:28px;

         margin-bottom:10px;
     }
 }

 </style>

 </head>

 <body>

 <div class="container">

 <div class="title">
 ESP32 DISTANCE METER
 </div>

 <div class="distance">

 <span id="value" class="value">0.0</span>

 <span class="unit">cm</span>

 </div>

 <div id="status" class="status">
 Monitoring...
 </div>

 <div class="footer">
 Real-time Ultrasonic Measurement
 </div>

 </div>

 <script>

 function updateDistance()
 {
     fetch('/data')

     .then(response => response.json())

     .then(data => {

         const valueSpan =
         document.getElementById('value');

         const status =
         document.getElementById('status');

         if(data.error)
         {
             valueSpan.innerHTML='--';

             status.innerHTML =
             'Out of Range';
         }
         else
         {
             valueSpan.innerHTML =
             data.distance.toFixed(1);

             valueSpan.classList.add('update');

             setTimeout(() =>
             valueSpan.classList.remove('update'),
             200);

             if(data.distance < 20)
             {
                 status.innerHTML =
                 '⚠ Object Too Close';

                 status.classList.add('alert');
             }
             else
             {
                 status.innerHTML =
                 'Monitoring...';

                 status.classList.remove('alert');
             }
         }
     })

     .catch(error => {

         document.getElementById('value')
         .innerHTML='--';
     });
 }

 updateDistance();

 setInterval(updateDistance, 300);

 </script>
 </body>
 </html>

 )rawliteral";

 // Main page
 void handleRoot()
 {
     server.send(200,
     "text/html",
     htmlPage);
 }

 // JSON API
 void handleData()
 {
     String json = "{";

     if(measurementError || distance_cm < 0)
     {
         json += "\"error\":\"Out of range\"";
         json += ",\"distance\":0";
     }
     else
     {
         json += "\"error\":null";

         json += ",\"distance\":" +
         String(distance_cm, 2);
     }

     json += "}";

     server.send(200,
     "application/json",
     json);
 }

 // 404
 void handleNotFound()
 {
     server.send(404,
     "text/plain",
     "404: Not Found");
 }

 void setup()
 {
     Serial.begin(115200);

     // Pin setup
     pinMode(TRIG_PIN, OUTPUT);

     pinMode(ECHO_PIN, INPUT);

     pinMode(BUZZER_PIN, OUTPUT);

     digitalWrite(TRIG_PIN, LOW);

     digitalWrite(BUZZER_PIN, LOW);

     // AP mode
     WiFi.mode(WIFI_AP);

     IPAddress local_ip(192,168,4,1);

     IPAddress gateway(192,168,4,1);

     IPAddress subnet(255,255,255,0);

     WiFi.softAPConfig(
         local_ip,
         gateway,
         subnet
     );

     // Start hotspot
     WiFi.softAP(ssid);

     Serial.println();
     Serial.println("ESP32 Hotspot Started");

     Serial.print("SSID: ");
     Serial.println(ssid);

     Serial.print("IP Address: ");
     Serial.println(WiFi.softAPIP());

     // Web routes
     server.on("/", handleRoot);

     server.on("/data", handleData);

     server.onNotFound(handleNotFound);

     // Start server
     server.begin();

     Serial.println("Web Server Started");
 }

 void loop()
 {
     server.handleClient();

     unsigned long currentMillis =
     millis();

     if(currentMillis - lastMeasurement
        >= MEASURE_INTERVAL)
     {
         float measuredDistance =
         measureDistance();

         if(measuredDistance > 0)
         {
             distance_cm =
             measuredDistance;

             measurementError = false;

             // Buzzer alert
             if(distance_cm < 20)
             {
                 digitalWrite(BUZZER_PIN, HIGH);
             }
             else
             {
                 digitalWrite(BUZZER_PIN, LOW);
             }
         }
         else
         {
             measurementError = true;

             digitalWrite(BUZZER_PIN, LOW);
         }

         lastMeasurement =
         currentMillis;
     }

     delay(10);
 }

.. raw:: html

   </div>
   <div style="display: flex; gap: 10px; padding: 12px 16px; background: #fff; border-top: 1px solid #ddd;">
     <button id="expand-btn-hcsr04" onclick="toggleCode('code-container-hcsr04', 'expand-btn-hcsr04')" style="flex: 1; padding: 10px 16px; background: #2980B9; color: white; border: none; border-radius: 4px; cursor: pointer; font-weight: bold;">▼ Expand All Code</button>
   </div>
   </div>

   <style>
   #code-container-hcsr04 { transition: max-height 0.4s ease-in-out; }
   </style>

   <script>
   function toggleCode(containerId, buttonId) {
     const container = document.getElementById(containerId);
     const btn = document.getElementById(buttonId);
     if (container.style.maxHeight === '420px' || container.style.maxHeight === '') {
       container.style.maxHeight = 'none';
       btn.textContent = '✕ Collapse Code';
     } else {
       container.style.maxHeight = '420px';
       btn.textContent = '▼ Expand All Code';
     }
   }
   </script>

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Display Effect:**

.. image:: _static/project/IOT/2.hcsr042.png
   :width: 500
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- After flashing the program, the ESP32 will automatically create a Wi-Fi hotspot named **ESP32-Distance-Meter** .

- After connecting to this Wi-Fi network using your mobile phone or computer, enter **192.168.4.1** in your browser to open a minimalist real-time distance measurement dashboard:

- The buzzer will sound an alarm when the distance is less than 20cm, and a notification will be displayed on the page.

----

3. Web-Control Servo
---------------------

This experiment is a comprehensive project integrating IoT remote control and servo driving, designed to teach you how to remotely control the rotation of an SG90 servo motor via an ESP32 using a web interface. You will master the following core skills:

- ESP32Servo Library Usage: Learn the principles of driving servos via PWM signals and master key functions such as `attach()` and `write()`. 

- RESTful API Design: Implement state setting and querying through endpoints like `/set?angle=xx` and `/status`, while understanding the communication architecture of frontend-backend separation. 

- Frontend Development (HTML/CSS/JavaScript): Design a responsive interactive interface featuring a slider, angle display, synchronized 3D servo visualization, and input debouncing. 

- Real-time Visual Feedback: Synchronize the rotation of the frontend servo visualization with the actual angle and implement two-way binding between the slider and the numerical display to enhance the user experience. 

- JSON Data Interaction: Use the `fetch()` API for asynchronous requests to retrieve the current angle status, ensuring synchronization during page initialization.

**Materials Needed:**

 - ESP32 Development Board
 - SG90 Servo
 - Power Supply 
 - Breadboard and Jumper Wires

**Wiring Diagram:**

.. image:: _static/project/IOT/3.SG90.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Wiring Table**

.. list-table:: 
   :header-rows: 1
   :widths: 10 20 20 25

   * - No.
     - Component
     - Pin
     - Connect to
   * - 1
     - SG90 Servo
     - Red (VCC)
     - 5V
   * - 1
     - SG90 Servo
     - Brown (GND)
     - GND
   * - 1
     - SG90 Servo
     - Orange (Signal)
     - GPIO 13

.. note::

    The servo requires a 5V power supply for stable operation; therefore, a breadboard power supply module used in conjunction with a battery is needed to provide a stable 5V supply.

**Example code:**

.. raw:: html

   <div style="background: #f8f9fa; border: 1px solid #ddd; border-radius: 6px; overflow: hidden;">
   <div id="code-container-servo" style="max-height: 420px; overflow: hidden; position: relative; background: #f5f5f0;">

.. code-block:: cpp

 #include <WiFi.h>
 #include <WebServer.h>
 #include <ESP32Servo.h>

 // ========== WiFi AP Configuration ==========
 const char* ap_ssid = "ESP32_Servo_Control";
 const char* ap_password = NULL;               

 // ========== Pin Definition ==========
 const int servoPin = 13;    // Servo signal pin (GPIO13)

 // ========== Web Server ==========
 WebServer server(80);

 // ========== Servo Object ==========
 Servo myServo;

 // ========== Current State ==========
 int currentAngle = 90;      // Starting at 90 degrees (center)

 // ========== HTML Page - Minimalist White Style ==========
 const char index_html[] PROGMEM = R"rawliteral(
 <!DOCTYPE html>
 <html lang="en">
 <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
     <title>Servo Control</title>
     <style>
         * {
             margin: 0;
             padding: 0;
             box-sizing: border-box;
         }
         
         body {
             font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
             background: #ffffff;
             min-height: 100vh;
             display: flex;
             justify-content: center;
             align-items: center;
             padding: 20px;
         }
         
         .container {
             background: #ffffff;
             text-align: center;
             max-width: 420px;
             width: 100%;
         }
         
         h1 {
             font-size: 28px;
             font-weight: 600;
             color: #000000;
             letter-spacing: -0.5px;
             margin-bottom: 8px;
         }
         
         .subtitle {
             color: #666666;
             font-size: 14px;
             font-weight: 400;
             margin-bottom: 40px;
         }
         
         /* Servo Model Container */
         .servo-model {
             background: #f5f5f5;
             border-radius: 24px;
             padding: 30px 20px;
             margin-bottom: 32px;
         }
         
         /* Servo Body */
         .servo-body {
             width: 200px;
             height: 140px;
             background: #e0e0e0;
             border-radius: 20px;
             margin: 0 auto;
             position: relative;
             box-shadow: none;
             border: 1px solid #cccccc;
         }
         
         /* Servo Top Cap */
         .servo-top {
             width: 160px;
             height: 30px;
             background: #d0d0d0;
             border-radius: 12px;
             position: absolute;
             top: -15px;
             left: 50%;
             transform: translateX(-50%);
             border: 1px solid #bbbbbb;
         }
         
         /* Servo Horn (Rotating Part) */
         .servo-horn {
             position: absolute;
             top: 50%;
             left: 50%;
             width: 60px;
             height: 60px;
             transform: translate(-50%, -50%) rotate(0deg);
             transition: transform 0.15s cubic-bezier(0.2, 0.9, 0.4, 1.1);
         }
         
         .horn-center {
             width: 16px;
             height: 16px;
             background: #333333;
             border-radius: 50%;
             position: absolute;
             top: 50%;
             left: 50%;
             transform: translate(-50%, -50%);
             z-index: 3;
         }
         
         .horn-arm {
             position: absolute;
             top: 0;
             left: 50%;
             width: 6px;
             height: 35px;
             background: #555555;
             transform: translateX(-50%);
             border-radius: 3px;
         }
         
         .horn-arm::before {
             content: '';
             position: absolute;
             top: -6px;
             left: -5px;
             width: 16px;
             height: 8px;
             background: #666666;
             border-radius: 3px;
         }
         
         .horn-arm::after {
             content: '';
             position: absolute;
             bottom: -6px;
             left: -5px;
             width: 16px;
             height: 8px;
             background: #666666;
             border-radius: 3px;
         }
         
         /* Screws */
         .screw {
             position: absolute;
             width: 8px;
             height: 8px;
             background: #999999;
             border-radius: 50%;
         }
         
         .screw-1 { top: 15px; left: 15px; }
         .screw-2 { top: 15px; right: 15px; }
         .screw-3 { bottom: 15px; left: 15px; }
         .screw-4 { bottom: 15px; right: 15px; }
         
         /* Label */
         .servo-label {
             position: absolute;
             bottom: 12px;
             left: 50%;
             transform: translateX(-50%);
             font-size: 10px;
             font-weight: 600;
             color: #555555;
             background: #e0e0e0;
             padding: 2px 8px;
             border-radius: 20px;
             font-family: monospace;
         }
         
         /* Angle Arc */
         .angle-arc {
             margin-top: 24px;
             display: flex;
             justify-content: space-between;
             align-items: center;
             padding: 0 10px;
         }
         
         .arc-label {
             font-size: 12px;
             color: #888888;
         }
         
         .arc-line {
             flex: 1;
             height: 3px;
             background: #e0e0e0;
             border-radius: 2px;
             margin: 0 12px;
             position: relative;
         }
         
         .arc-fill {
             position: absolute;
             height: 3px;
             background: #000000;
             border-radius: 2px;
             width: 50%;
             transition: width 0.15s ease;
         }
         
         /* Angle Display */
         .angle-display {
             margin-bottom: 28px;
         }
         
         .angle-value {
             font-size: 48px;
             font-weight: 600;
             color: #000000;
             font-family: monospace;
         }
         
         .angle-unit {
             font-size: 16px;
             color: #888888;
             margin-left: 4px;
         }
         
         /* Slider */
         .slider-container {
             margin-bottom: 28px;
         }
         
         input[type="range"] {
             width: 100%;
             height: 4px;
             -webkit-appearance: none;
             background: #e0e0e0;
             border-radius: 2px;
             outline: none;
         }
         
         input[type="range"]::-webkit-slider-thumb {
             -webkit-appearance: none;
             width: 20px;
             height: 20px;
             background: #000000;
             border-radius: 50%;
             cursor: pointer;
             border: none;
         }
         
         input[type="range"]::-webkit-slider-thumb:hover {
             transform: scale(1.1);
         }
         
         /* Button Group */
         .button-group {
             display: flex;
             gap: 12px;
             justify-content: center;
         }
         
         .btn {
             padding: 12px 24px;
             font-size: 15px;
             font-weight: 500;
             border: 1px solid #cccccc;
             background: #ffffff;
             color: #333333;
             border-radius: 30px;
             cursor: pointer;
             transition: all 0.2s ease;
         }
         
         .btn:hover {
             background: #f0f0f0;
             border-color: #999999;
         }
         
         .btn:active {
             transform: scale(0.96);
         }
         
         /* Divider */
         .divider {
             height: 1px;
             background: #eeeeee;
             margin: 32px 0 20px 0;
         }
         
         .info-text {
             font-size: 11px;
             color: #aaaaaa;
             margin-top: 20px;
         }
         
         @media (max-width: 480px) {
             .container { padding: 0; }
             h1 { font-size: 24px; }
             .servo-body { width: 180px; height: 125px; }
             .servo-top { width: 145px; }
             .angle-value { font-size: 40px; }
             .btn { padding: 10px 20px; font-size: 14px; }
         }
     </style>
     <script>
         let currentAngle = 90;
         let isUpdating = false;
         
         function setAngle(angle) {
             if (isUpdating) return;
             isUpdating = true;
             
             angle = Math.min(180, Math.max(0, angle));
             currentAngle = angle;
             
             updateUI();
             
             fetch('/set?angle=' + angle)
                 .catch(error => console.error('Error:', error))
                 .finally(() => {
                     setTimeout(() => { isUpdating = false; }, 100);
                 });
         }
         
         function updateUI() {
             const slider = document.getElementById('angleSlider');
             if (slider) slider.value = currentAngle;
             
             const angleSpan = document.getElementById('angleValue');
             if (angleSpan) angleSpan.textContent = currentAngle;
             
             const horn = document.getElementById('servoHorn');
             if (horn) {
                 const rotation = currentAngle - 90;
                 horn.style.transform = 'translate(-50%, -50%) rotate(' + rotation + 'deg)';
             }
             
             const arcFill = document.getElementById('arcFill');
             if (arcFill) {
                 const percent = (currentAngle / 180) * 100;
                 arcFill.style.width = percent + '%';
             }
         }
         
         function resetAngle() {
             setAngle(90);
         }
         
         function setMinAngle() {
             setAngle(0);
         }
         
         function setMaxAngle() {
             setAngle(180);
         }
         
         document.addEventListener('DOMContentLoaded', () => {
             const slider = document.getElementById('angleSlider');
             if (slider) {
                 slider.addEventListener('input', (e) => {
                     setAngle(parseInt(e.target.value));
                 });
             }
             
             fetch('/status')
                 .then(response => response.json())
                 .then(data => {
                     if (data.angle !== undefined) {
                         currentAngle = data.angle;
                         updateUI();
                     }
                 })
                 .catch(error => console.error('Status error:', error));
         });
     </script>
 </head>
 <body>
     <div class="container">
         <h1>Servo Control</h1>
         <div class="subtitle">SG90 Servo Motor</div>
         
         <!-- Servo Model -->
         <div class="servo-model">
             <div class="servo-body">
                 <div class="servo-top"></div>
                 <div class="servo-horn" id="servoHorn">
                     <div class="horn-arm"></div>
                     <div class="horn-center"></div>
                 </div>
                 <div class="screw screw-1"></div>
                 <div class="screw screw-2"></div>
                 <div class="screw screw-3"></div>
                 <div class="screw screw-4"></div>
                 <div class="servo-label">SG90</div>
             </div>
             
             <div class="angle-arc">
                 <span class="arc-label">0°</span>
                 <div class="arc-line">
                     <div class="arc-fill" id="arcFill"></div>
                 </div>
                 <span class="arc-label">180°</span>
             </div>
         </div>
         
         <!-- Angle Display -->
         <div class="angle-display">
             <span class="angle-value" id="angleValue">90</span>
             <span class="angle-unit">degrees</span>
         </div>
         
         <!-- Slider -->
         <div class="slider-container">
             <input type="range" id="angleSlider" min="0" max="180" value="90">
         </div>
         
         <!-- Buttons -->
         <div class="button-group">
             <button class="btn" onclick="setMinAngle()">0°</button>
             <button class="btn" onclick="resetAngle()">90°</button>
             <button class="btn" onclick="setMaxAngle()">180°</button>
         </div>
         
         <div class="divider"></div>
         <div class="info-text">ESP32 · SG90 Servo Controller</div>
     </div>
 </body>
 </html>
 )rawliteral";

 // ========== Servo Control Functions ==========
 void setServoAngle(int angle) {
     angle = constrain(angle, 0, 180);
     currentAngle = angle;
     myServo.write(currentAngle);
     Serial.print("Servo angle: ");
     Serial.print(currentAngle);
     Serial.println("°");
 }

 // ========== Web Request Handlers ==========
 void handleRoot() {
     server.send(200, "text/html", index_html);
 }

 void handleSet() {
     if (server.hasArg("angle")) {
         int newAngle = server.arg("angle").toInt();
         setServoAngle(newAngle);
         server.send(200, "text/plain", "OK");
     } else {
         server.send(400, "text/plain", "Bad Request");
     }
 }

 void handleStatus() {
     String json = "{\"angle\":" + String(currentAngle) + "}";
     server.send(200, "application/json", json);
 }

 void handleNotFound() {
     server.send(404, "text/plain", "404: Not Found");
 }

 // ========== Setup ==========
 void setup() {
     Serial.begin(115200);
     delay(100);
     
     myServo.attach(servoPin);
     myServo.write(currentAngle);
     
     Serial.println();
     Serial.println("=== ESP32 SG90 Servo Controller ===");
     Serial.print("Initial angle: ");
     Serial.print(currentAngle);
     Serial.println("°");
     
     Serial.print("Starting AP: ");
     Serial.println(ap_ssid);
     
     WiFi.softAP(ap_ssid, ap_password);
     
     IPAddress apIP = WiFi.softAPIP();
     Serial.print("AP IP: ");
     Serial.println(apIP);
     
     server.on("/", handleRoot);
     server.on("/set", handleSet);
     server.on("/status", handleStatus);
     server.onNotFound(handleNotFound);
     
     server.begin();
     Serial.println("Server started");
     Serial.print("Connect to: ");
     Serial.println(ap_ssid);
     Serial.print("Then visit: http://");
     Serial.println(apIP);
     Serial.println("====================================");
 }

 void loop() {
     server.handleClient();
     delay(10);
 }

.. raw:: html

   </div>
   <div style="display: flex; gap: 10px; padding: 12px 16px; background: #fff; border-top: 1px solid #ddd;">
     <button id="expand-btn-servo" onclick="toggleCode('code-container-servo', 'expand-btn-SG90')" style="flex: 1; padding: 10px 16px; background: #2980B9; color: white; border: none; border-radius: 4px; cursor: pointer; font-weight: bold;">▼ Expand All Code</button>
   </div>
   </div>

   <style>
   #code-container-servo { transition: max-height 0.4s ease-in-out; }
   </style>

   <script>
   function toggleCode(containerId, buttonId) {
     const container = document.getElementById(containerId);
     const btn = document.getElementById(buttonId);
     if (container.style.maxHeight === '420px' || container.style.maxHeight === '') {
       container.style.maxHeight = 'none';
       btn.textContent = '✕ Collapse Code';
     } else {
       container.style.maxHeight = '420px';
       btn.textContent = '▼ Expand All Code';
     }
   }
   </script>

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Display Effect:**

.. image:: _static/project/IOT/3.SG902.png
   :width: 700
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The ESP32 creates a Wi-Fi hotspot named **ESP32_Servo_Control**.

- After connecting to this Wi-Fi network via a smartphone or computer, accessing the address **192.168.4.1** opens a control page. This page features a 3D visualization of the servo, an angle display, a slider, and quick-action buttons (0°, 90°, and 180°).

- Dragging the slider or clicking the quick-action buttons causes the servo to immediately rotate to the specified angle; simultaneously, the 3D visualization rotates in sync and the angle value updates in real-time, delivering a "what-you-see-is-what-you-get" remote control experience.

----

4. Colorful RGB
---------------

This experiment is a comprehensive project on IoT-based smart lighting control. It aims to teach you how to drive WS2812B full-color RGB LED strips using an ESP32 and remotely control various dynamic lighting effects via a Wi-Fi-hosted web interface. You will master the following core skills:

- **FastLED Library Usage:** Learn the driving principles of WS2812B programmable LEDs and master key functions such as `addLeds()`, `show()`, `fadeToBlackBy()`, and `nscale8_video()`.

- **Multi-mode Lighting Algorithms:** Implement three dynamic effects—Chase, Gradient, and Flow—while gaining an understanding of the HSV color model and brightness control.

- **Custom RGB Color Mixing:** Independently adjust red, green, and blue channels via sliders to create any desired color, with support for real-time preview.

- **Hardware Button Integration:** Use GPIO buttons for physical mode switching (short-press to cycle through modes), enabling operation without a network connection.

**Materials Needed:**

 - ESP32 Development Board
 - RGB LED strip
 - Button
 - Resistor (10K)
 - Breadboard and Jumper Wires

**Wiring Diagram:**

.. image:: _static/project/BASIC/7.Thermometer.png
   :width: 700
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Wiring Table**

.. list-table:: 
   :header-rows: 1
   :widths: 10 20 20 25

   * - No.
     - Component
     - Pin
     - Connect to
   * - 1
     - WS2812B LED Strip
     - VCC
     - 5V
   * - 1
     - WS2812B LED Strip
     - GND
     - GND
   * - 1
     - WS2812B LED Strip
     - DATA
     - GPIO 5
   * - 2
     - Button
     - One pin
     - 3.3V
   * - 2
     - Button
     - Other pin
     - GPIO 4
   * - 3
     - 10kΩ Resistor
     - One pin
     - GPIO 4
   * - 3
     - 10kΩ Resistor
     - Other pin
     - GND

**Example code:**

.. raw:: html

   <div style="background: #f8f9fa; border: 1px solid #ddd; border-radius: 6px; overflow: hidden;">
   <div id="code-container-rgb" style="max-height: 420px; overflow: hidden; position: relative; background: #f5f5f0;">

.. code-block:: cpp

 #include <WiFi.h>
 #include <WebServer.h>
 #include <FastLED.h>

 #define LED_PIN 5
 #define NUM_LEDS 8
 #define BUTTON_PIN 4

 CRGB leds[NUM_LEDS];
 WebServer server(80);

 bool power = false;
 int mode = 0;
 bool customMode = false;
 int r = 255, g = 100, b = 150;

 int hueOffset = 0;
 int chasePos = 0;

 const char* html = R"(
 <!DOCTYPE html>
 <html>
 <head>
 <meta charset="UTF-8">
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
 <title>RGB Light</title>
 <style>
 *{margin:0;padding:0;box-sizing:border-box}
 body{background:#f5f5f5;font-family:-apple-system,Helvetica Neue,Segoe UI,sans-serif;display:flex;justify-content:center;align-items:center;min-height:100vh;padding:20px}
 .container{background:#fff;border-radius:24px;padding:40px;max-width:500px;width:100%;box-shadow:0 10px 40px rgba(0,0,0,0.06);border:1px solid #f0f0f0}
 h1{font-size:24px;font-weight:300;letter-spacing:3px;text-align:center;margin-bottom:8px;color:#1a1a1a}
 .sub{text-align:center;font-size:13px;color:#999;letter-spacing:2px;margin-bottom:28px}
 .status{display:flex;justify-content:space-between;padding:12px 16px;background:#f8f8f8;border-radius:12px;margin-bottom:24px;font-size:13px;color:#666}
 .dot{display:inline-block;width:8px;height:8px;border-radius:50%;margin-right:8px}
 .dot.on{background:#2ecc71}
 .dot.off{background:#e74c3c}
 .btn-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;margin-bottom:20px}
 .btn{background:#f8f8f8;border:2px solid transparent;padding:14px 8px;border-radius:12px;cursor:pointer;font-size:13px;font-weight:500;transition:all .2s;font-family:inherit;color:#333}
 .btn:hover{background:#f0f0f0}
 .btn.active{background:#1a1a1a;color:#fff}
 .btn-power{grid-column:span 3;padding:16px}
 .btn-power.on{background:#1a1a1a;color:#fff;border-color:#1a1a1a}
 .sec{margin-bottom:20px}
 .sec-title{font-size:11px;text-transform:uppercase;letter-spacing:2px;color:#999;margin-bottom:10px;font-weight:500}
 .row{display:flex;align-items:center;gap:14px;margin-bottom:8px}
 .row .lbl{font-size:13px;color:#555;min-width:20px}
 .row .val{font-size:12px;color:#888;min-width:32px;text-align:right}
 input[type=range]{-webkit-appearance:none;flex:1;height:3px;border-radius:2px;background:#e0e0e0;outline:none}
 input[type=range]::-webkit-slider-thumb{-webkit-appearance:none;width:16px;height:16px;border-radius:50%;background:#1a1a1a;cursor:pointer}
 .toggle{display:flex;align-items:center;gap:12px;padding:12px 16px;background:#f8f8f8;border-radius:12px;margin-bottom:14px;cursor:pointer}
 .toggle-track{width:44px;height:24px;background:#ddd;border-radius:12px;position:relative;transition:.3s;flex-shrink:0}
 .toggle-track.on{background:#1a1a1a}
 .toggle-thumb{width:20px;height:20px;background:#fff;border-radius:50%;position:absolute;top:2px;left:2px;transition:.3s}
 .toggle-track.on .toggle-thumb{left:22px}
 .toggle-label{font-size:13px;color:#555}
 .rgb-ctrl{opacity:0.4;pointer-events:none;transition:.3s}
 .rgb-ctrl.active{opacity:1;pointer-events:auto}
 .rgb-ctrl .row .lbl{font-weight:500;font-size:12px}
 .rgb-ctrl .row .lbl.r{color:#e74c3c}
 .rgb-ctrl .row .lbl.g{color:#2ecc71}
 .rgb-ctrl .row .lbl.b{color:#3498db}
 .preview{display:flex;align-items:center;gap:16px;padding:12px 16px;background:#f8f8f8;border-radius:12px;margin-top:8px}
 .color-box{width:48px;height:48px;border-radius:12px;flex-shrink:0;border:2px solid #fff;box-shadow:0 2px 8px rgba(0,0,0,0.06)}
 .color-info{font-size:12px;color:#888}
 .color-info strong{color:#1a1a1a;font-weight:500}
 .footer{text-align:center;font-size:11px;color:#ccc;margin-top:24px;padding-top:20px;border-top:1px solid #f0f0f0}
 </style>
 </head>
 <body>
 <div class=container>
 <h1>✦ ESP32-RGB</h1>
 <div class=status><div><span class=dot id=dot></span><span id=status>Off</span></div><div>Mode: <span id=modeText>—</span></div></div>
 <div class=btn-grid>
 <button class=btn onclick=setMode(1)>▶ Chase</button>
 <button class=btn onclick=setMode(2)>◈ Gradient</button>
 <button class=btn onclick=setMode(3)>〰 Flow</button>
 <button class="btn btn-power" id=pwr onclick=togglePower()>⏻ Power</button>
 </div>
 <div class=sec>
 <div class=sec-title>Color</div>
 <div class=toggle onclick=toggleCustom()><div class=toggle-track id=tt><div class=toggle-thumb></div></div><div class=toggle-label>Custom RGB <span id=togStatus>Off</span></div></div>
 <div class=rgb-ctrl id=rgbCtrl>
 <div class=row><span class=lbl r>R</span><input type=range id=red min=0 max=255 value=255 oninput=updateRGB()><span class=val id=redVal>255</span></div>
 <div class=row><span class=lbl g>G</span><input type=range id=green min=0 max=255 value=100 oninput=updateRGB()><span class=val id=greenVal>100</span></div>
 <div class=row><span class=lbl b>B</span><input type=range id=blue min=0 max=255 value=150 oninput=updateRGB()><span class=val id=blueVal>150</span></div>
 </div>
 <div class=preview><div class=color-box id=colorBox></div><div class=color-info><strong id=hex>#FF6496</strong><br><span id=rgbText>RGB(255,100,150)</span></div></div>
 </div>
 <div class=footer></div>
 </div>
 <script>
 let custom=false;
 function upd(){
 fetch('/status').then(r=>r.json()).then(d=>{
 document.getElementById('dot').className='dot '+(d.power?'on':'off');
 document.getElementById('status').textContent=d.power?'On':'Off';
 document.getElementById('modeText').textContent=['—','Chase','Gradient','Flow'][d.mode]||'—';
 document.getElementById('pwr').className='btn btn-power'+(d.power?' on':'');
 document.querySelectorAll('.btn-grid .btn:not(.btn-power)').forEach((b,i)=>{b.className='btn'+(i+1==d.mode?' active':'')});
 if(d.custom){
 document.getElementById('red').value=d.r;document.getElementById('green').value=d.g;document.getElementById('blue').value=d.b;
 document.getElementById('redVal').textContent=d.r;document.getElementById('greenVal').textContent=d.g;document.getElementById('blueVal').textContent=d.b;
 custom=d.custom;updToggle();updColor(d.r,d.g,d.b);
 }
})}
 function togglePower(){fetch('/power',{method:'POST'}).then(()=>upd())}
 function setMode(m){fetch('/mode?mode='+m,{method:'POST'}).then(()=>upd())}
 function toggleCustom(){custom=!custom;fetch('/custom?state='+(custom?1:0),{method:'POST'}).then(()=>{updToggle();document.getElementById('rgbCtrl').className='rgb-ctrl'+(custom?' active':'');if(custom)updateRGB();upd()})}
 function updToggle(){document.getElementById('tt').className='toggle-track'+(custom?' on':'');document.getElementById('togStatus').textContent=custom?'On':'Off'}
 function updateRGB(){const r=parseInt(document.getElementById('red').value),g=parseInt(document.getElementById('green').value),b=parseInt(document.getElementById('blue').value);document.getElementById('redVal').textContent=r;document.getElementById('greenVal').textContent=g;document.getElementById('blueVal').textContent=b;updColor(r,g,b);fetch('/rgb?r='+r+'&g='+g+'&b='+b,{method:'POST'})}
 function updColor(r,g,b){const h='#'+[r,g,b].map(c=>c.toString(16).padStart(2,'0')).join('');document.getElementById('colorBox').style.background=h;document.getElementById('hex').textContent=h.toUpperCase();document.getElementById('rgbText').textContent='RGB('+r+','+g+','+b+')'}
 upd();setInterval(upd,800);
 </script>
 </body>
 </html>
 )";

 void chase() {
     if (!power) return;
     for (int i = 0; i < NUM_LEDS; i++) leds[i].fadeToBlackBy(20);
     CRGB color = customMode ? CRGB(r, g, b) : CHSV(hueOffset++, 200, 128);
     leds[chasePos] = color;
     chasePos = (chasePos + 1) % NUM_LEDS;
     FastLED.show();
     delay(150);
 }

 void gradient() {
     if (!power) return;
     if (customMode) {
         for (int i = 0; i < NUM_LEDS; i++) {
             float f = (float)i / NUM_LEDS;
             leds[i] = CRGB(r * (0.3 + 0.7 * (1 - f)), g * (0.3 + 0.7 * f), b * (0.3 + 0.7 * (1 - sin(f * 3.14))));
         }
     } else {
         hueOffset++;
         for (int i = 0; i < NUM_LEDS; i++) leds[i] = CHSV(hueOffset + i * 2, 200, 128);
     }
     FastLED.show();
     delay(20);
 }

 void flow() {
     if (!power) return;
     if (customMode) {
         int waveOffset = (millis() / 30) % 256;
         for (int i = 0; i < NUM_LEDS; i++) {
             leds[i] = CRGB(r, g, b);
             leds[i].nscale8_video((sin8(i * 16 + waveOffset * 2) * 128) / 255);
         }
     } else {
         hueOffset += 2;
         for (int i = 0; i < NUM_LEDS; i++) {
             leds[i] = CHSV(hueOffset + i * 4, 220, (sin8(i * 12 + hueOffset * 2) * 128) / 255);
         }
     }
     FastLED.show();
     delay(25);
 }

 void handleRoot() { server.send(200, "text/html", html); }

 void handleStatus() {
     String j = "{\"power\":" + String(power ? "true" : "false") + ",\"mode\":" + String(mode) + ",\"custom\":" + String(customMode ? "true" : "false") + ",\"r\":" + String(r) + ",\"g\":" + String(g) + ",\"b\":" + String(b) + "}";
     server.send(200, "application/json", j);
 }

 void handlePower() {
     power = !power;
     if (!power) { FastLED.clear(); FastLED.show(); mode = 0; }
     server.send(200, "text/plain", "OK");
 }

 void handleMode() {
     if (server.hasArg("mode")) { mode = server.arg("mode").toInt(); if (mode > 3) mode = 0; if (mode > 0) power = true; }
     server.send(200, "text/plain", "OK");
 }

 void handleCustom() {
     if (server.hasArg("state")) customMode = server.arg("state").toInt() == 1;
     server.send(200, "text/plain", "OK");
 }

 void handleRGB() {
     if (server.hasArg("r") && server.hasArg("g") && server.hasArg("b")) {
         r = constrain(server.arg("r").toInt(), 0, 255);
         g = constrain(server.arg("g").toInt(), 0, 255);
         b = constrain(server.arg("b").toInt(), 0, 255);
     }
     server.send(200, "text/plain", "OK");
 }

 void button() {
     static unsigned long last = 0;
     if (millis() - last < 300) return;
     last = millis();
     if (digitalRead(BUTTON_PIN) == HIGH) {
         mode = (mode + 1) % 4;
         if (mode == 0) { power = false; FastLED.clear(); FastLED.show(); } else power = true;
     }
 }

 void setup() {
     FastLED.addLeds<WS2812B, LED_PIN, GRB>(leds, NUM_LEDS);
     FastLED.clear();
     FastLED.show();
     pinMode(BUTTON_PIN, INPUT);
     WiFi.softAP("ESP32-RGB", NULL);
     server.on("/", handleRoot);
     server.on("/status", handleStatus);
     server.on("/power", HTTP_POST, handlePower);
     server.on("/mode", HTTP_POST, handleMode);
     server.on("/custom", HTTP_POST, handleCustom);
     server.on("/rgb", HTTP_POST, handleRGB);
     server.begin();
 }

 void loop() {
     server.handleClient();
     button();
     static unsigned long lastEffect = 0;
     if (millis() - lastEffect > 15) {
         lastEffect = millis();
         switch (mode) {
             case 1: chase(); break;
             case 2: gradient(); break;
             case 3: flow(); break;
         }
     }
     delay(1);
 }
.. raw:: html

   </div>
   <div style="display: flex; gap: 10px; padding: 12px 16px; background: #fff; border-top: 1px solid #ddd;">
     <button id="expand-btn-rgb" onclick="toggleCode('code-container-rgb', 'expand-btn-rgb')" style="flex: 1; padding: 10px 16px; background: #2980B9; color: white; border: none; border-radius: 4px; cursor: pointer; font-weight: bold;">▼ Expand All Code</button>
   </div>
   </div>

   <style>
   #code-container-rgb { transition: max-height 0.4s ease-in-out; }
   </style>

   <script>
   function toggleCode(containerId, buttonId) {
     const container = document.getElementById(containerId);
     const btn = document.getElementById(buttonId);
     if (container.style.maxHeight === '420px' || container.style.maxHeight === '') {
       container.style.maxHeight = 'none';
       btn.textContent = '✕ Collapse Code';
     } else {
       container.style.maxHeight = '420px';
       btn.textContent = '▼ Expand All Code';
     }
   }
   </script>

.. raw:: html

   <div style="margin-top: 30px;"></div>


**Display Effect:**

.. image:: _static/project/BASIC/7.Thermometer.png
   :width: 700
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

After flashing the firmware, the ESP32 creates a Wi-Fi hotspot named **ESP32-RGB** . Once a smartphone or computer connects to this network, accessing **192.168.4.1** opens the control page, which offers comprehensive lighting controls:

Power Control: Toggle the LED strip on or off; turning it off clears all LEDs and resets the mode.

Three Dynamic Modes: Chase, Gradient (rainbow fade), and Flow (flowing halo); switching modes takes effect immediately.

Custom RGB Color Mixing: Enable custom mode via the toggle switch to independently adjust the Red, Green, and Blue channels with real-time color preview.

Physical Button: A short press of the button connected to GPIO4 cycles through the modes (Chase → Gradient → Flow → Off → Cycle).

----

