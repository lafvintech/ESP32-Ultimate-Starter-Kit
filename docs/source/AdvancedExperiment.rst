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

