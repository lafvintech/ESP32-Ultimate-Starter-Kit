Advanced Experiments
====================

.. image:: _static/project/IOT/1.WEB.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

While foundational experiments taught you the basics of hands-on hardware interaction, this chapter introduces you to network connectivity. Centered on the Internet of Things (IoT), we will guide you in equipping your development board with Wi-Fi capabilities and implementing remote control via a web interface. Covering everything from TCP/IP fundamentals and HTTP request parsing to HTML page design and real-time status feedback, you will build a complete web-based control system—enabling your sensors and actuators to transcend physical distance and advance to a new level of smart connectivity.

----

1. TEMP and HUMI Meter
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

.. image:: _static/project/IOT/1.dht11.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

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

.. image:: _static/project/IOT/1.DHT112.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- The system will automatically create a Wi-Fi hotspot named ESP32-DHT11. 

- After connecting to this Wi-Fi network using your mobile phone or computer, enter the IP address 192.168.4.1 in your browser 

- To open a beautifully designed temperature and humidity monitoring panel to view real-time temperature and humidity data.

----

2. Ultrasonic Distance Meter
----------------------------




