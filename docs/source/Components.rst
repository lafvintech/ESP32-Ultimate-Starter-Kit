Components Introduction
=======================

.. image:: _static/Component/1.fenmian.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

----


1. ESP32 Development Board
--------------------------

.. image:: _static/Component/2.esp32_1.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The ESP32 is a series of low-cost and low-power System on a Chip (SoC) microcontrollers developed by Espressif that include Wi-Fi and Bluetooth wireless capabilities and a dual-core processor.

The ESP32 development board included in this kit provides a complete platform for rapid IoT prototyping and embedded learning. Its core features include:

- **CPU:** Dual-core Tensilica LX6 processor, up to 240 MHz.
- **RAM:** 520 KB internal SRAM and external PSRAM support on some modules.
- **Flash:** On-board SPI flash memory, typically 4 MB or 8 MB depending on the module.
- **Wireless:** Integrated 802.11 b/g/n Wi-Fi and Bluetooth v4.2 / BLE.
- **I/O interfaces:** Multiple GPIO pins, ADC, DAC, SPI, I2C, UART, PWM, and touch sensor inputs.
- **Sensors support:** Compatible with temperature/humidity sensors, motion sensors, ultrasonic sensors, and other kit components.
- **Power:** Flexible power input from USB or external supply, with voltage regulator and power management features.
- **Development:** Supports Arduino IDE, ESP-IDF, MicroPython, and other popular firmware frameworks.

This board is the heart of your starter kit. It allows you to connect sensors, drives, displays, and communication modules, while giving you easy access to Wi‑Fi and Bluetooth for wireless control, data logging, and IoT applications.

**ESP32 Pinout**

The ESP32 peripherals include：18 Analog-to-Digital Converter (ADC) channels

 - 3 SPI interfaces
 - 3 UART interfaces
 - 2 I2C interfaces
 - 16 PWM output channels
 - 2 Digital-to-Analog Converters (DAC)
 - 2 I2S interfaces
 - 10 Capacitive sensing GPIOs

.. image:: _static/Component/3.esp32_2.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The ADC (analog to digital converter) and DAC (digital to analog converter) features are assigned to specific static pins. However, you can decide which pins
are UART, I2C, SPI, PWM, etc – you just need to assign them in the code. This is possible due to the ESP32 chip’s multiplexing feature. Although you can define the pins properties on the software, there are pins
assigned by default.

- For more information, click here to view the ESP32 datasheet: `ESP32 Datasheet <https://documentation.espressif.com/esp32_datasheet_en.pdf>`_

----

2. MPU6050 Attitude Sensor
--------------------------

.. image:: _static/Component/4.mpu6050.png
   :width: 300
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The MPU6050 is an Inertial Measurement Unit (IMU) sensor that combines a 3-axis accelerometer and 3-axis gyroscope on a single chip. It is one of the most popular motion sensors for robotics, drones, motion tracking, and gesture recognition applications.

**Core Features:**

- **Accelerometer:** 3-axis acceleration measurement with selectable ranges (±2g, ±4g, ±8g, ±16g).
- **Gyroscope:** 3-axis angular velocity measurement with selectable ranges (±250°/s, ±500°/s, ±1000°/s, ±2000°/s).
- **Temperature Sensor:** Integrated temperature sensor for thermal compensation.
- **Communication:** I2C interface for easy connection to microcontrollers like ESP32.
- **Resolution:** 16-bit analog-to-digital converters (ADC) for precise measurements.
- **Low Power:** Operating voltage 3.0V to 3.6V, making it ideal for battery-powered applications.
- **Built-in DMP:** Digital Motion Processor (on some models) for computing quaternions and orientation angles.

**Applications:**

- Motion tracking and orientation detection (e.g., smartphone motion sensing)
- Robotics and drone control
- Gesture recognition and hand tracking
- Vibration monitoring and shock detection
- Game controller input and VR applications
- Postural analysis and human activity recognition

**Pinout:**

The MPU6050 uses the I2C protocol with two main pins:

- **SDA (Serial Data):** Data line connected to ESP32's I2C SDA pin
- **SCL (Serial Clock):** Clock line connected to ESP32's I2C SCL pin
- **VCC:** Power supply (3.3V from ESP32)
- **GND:** Ground connection

.. note::

   - The MPU6050 sensor is a powerful tool for measuring motion and orientation, but it can be sensitive to noise and requires proper calibration for accurate readings. It is recommended to use a stable power supply and minimize vibrations during measurements for best performance.       

----

3. DHT11 Sensor
---------------

.. image:: _static/Component/5.dht11.png
   :width: 200
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The DHT11 is a basic, low-cost digital temperature and humidity sensor. It uses a capacitive humidity sensor and a thermistor to measure the surrounding air, and outputs a digital signal on the data pin.

**Core Features:**

- **Temperature Range:** 0 to 50°C with ±2°C accuracy
- **Humidity Range:** 20% to 90% RH with ±5% accuracy    
- **Data Output:** Digital signal via a single-wire interface
- **Power Supply:** 3.3V to 5V
- **Low Power Consumption:** Ideal for battery-powered applications
- **Compact Size:** Small form factor for easy integration into projects   

**Applications:**

- Environmental monitoring and weather stations
- Home automation and smart thermostats
- Greenhouse and agricultural monitoring
- HVAC systems and air quality monitoring
- Data logging and IoT sensor networks
- Educational projects and STEM learning

**Pinout:** 

- **VCC:** Power supply (3.3V or 5V)
- **GND:** Ground connection     
- **DATA:** Digital output pin connected to ESP32 GPIO for reading temperature and humidity data

.. note::
   - The DHT11 sensor is suitable for basic temperature and humidity measurements, but it has limited accuracy and a slow response time compared to more advanced sensors. For applications requiring higher precision or faster updates, consider using the DHT22 or other digital sensors.

For more information, click here to view the DHT11 datasheet: `DHT11 Datasheet <https://www.mouser.com/datasheet/2/758/DHT11-Technical-Data-Sheet-Translated-Version-1143054.pdf?srsltid=AfmBOooi92JcYF4XryfVGxzN6rWYdG9Y2aPWRpiw7p7HqvUF11LQB6y_>`_

----

4. PIR Motion Sensor
--------------------

.. image:: _static/Component/6.pir.png
   :width: 300
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The PIR (Passive Infrared) Motion Sensor is a widely used device for detecting motion in various applications. It works by sensing the infrared radiation emitted by objects in its field of view, particularly the heat signature of humans or animals.

**Core Features:**

- **Motion Detection:** Detects movement within a specified range (typically 3-7 meters)
- **Output Signal:** Provides a digital output signal when motion is detected
- **Power Supply:** Operates on 5V DC power supply
- **Operating Temperature:** Functions within a temperature range of -20°C to +70°C
- **Sensitivity Adjustment:** Allows adjustment of detection sensitivity and delay time

.. image:: _static/Component/6.pir2.png
   :width: 500
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Applications:**

- Security systems and alarm installations
- Automatic lighting control in residential and commercial buildings
- Energy-efficient lighting solutions
- Surveillance and monitoring systems
- Smart home automation projects
- Industrial automation and safety applications

**Pinout:**

- **VCC:** Power supply (3.3V or 5V DC)
- **GND:** Ground connection
- **OUT:** Digital output pin connected to ESP32 GPIO for reading motion detection status

.. note::

   - The PIR sensor is ideal for detecting human or animal movement but may be affected by environmental factors such as temperature changes or direct sunlight. Proper placement and calibration are essential for optimal performance.

----

5. Light Sensor
----------------

.. image:: _static/Component/7.light.png
   :width: 500
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The Light Sensor, often based on a photoresistor (LDR) or photodiode, is a device that detects and measures the intensity of ambient light. It is commonly used in various applications to enable automatic adjustments based on lighting conditions.

**Core Features:**

- **Light Intensity Measurement:** Provides an analog output that varies with the intensity of light
- **Wide Detection Range:** Can detect a broad range of light levels, from dim to bright  
- **Power Supply:** Typically operates on 3.3V or 5V DC power supply
- **Compact Size:** Small form factor for easy integration into projects

**Applications:**

- Automatic lighting control based on ambient light levels
- Light-sensitive alarms and security systems
- Solar tracking systems for optimizing solar panel orientation
- Environmental monitoring and weather stations
- Smart home automation projects
- Educational projects and STEM learning

**Pinout:**

- **VCC:** Power supply (3.3V or 5V DC)
- **GND:** Ground connection
- **AO:** Analog output pin connected to ESP32 ADC for reading light intensity levels

.. note::

  - The sensor board typically features a blue adjustable resistor (potentiometer); turning it with a screwdriver allows you to adjust the trigger threshold:
  
  - Counter-clockwise: More sensitive (triggers even when it is only slightly dark)
  
  - Clockwise: Less sensitive (requires darker conditions to trigger)

----

6. Water Level Sensor
-----------------------

.. image:: _static/Component/8.water.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The Water Level Sensor included in this kit is a simple resistive/analog probe-type sensor module that can detect water presence and provide an analog voltage proportional to the conductivity (used as a proxy for level) 

**Working principle:**

- The probe consists of a series of metal traces that change the effective resistance when immersed in water. The sensor board uses this change in resistance as part of a voltage divider to produce an analog voltage at the `S` pin.

**Core features:**

- **Analog output (AO):** Variable voltage proportional to water contact/conductivity (connect to ESP32 ADC)
- **Power:** 3.3V - 5V compatible (use 3.3V with ESP32 to avoid ADC overrange)

**Pinout:**

- **VCC:** 3.3V (or 5V) power input — use 3.3V when reading AO with ESP32 ADC
- **GND:** Ground
- **AO:** Analog output — connect to an ESP32 ADC-capable pin (e.g., `GPIO34`, `GPIO35`, etc.)

.. note::

  - When immersing in water, lower it slowly and vertically; under no circumstances should the end with the circuit board come into contact with the water.

----

7. Ultrasonic Distance Sensor 
-----------------------------

.. image:: _static/Component/9.hcsr04.png
   :width: 400
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The HC-SR04 is an ultrasonic distance sensor commonly used to measure the distance to an object by sending an ultrasonic pulse and measuring the echo time.

**Working principle:**

- The module transmits a short 40 kHz ultrasonic burst via the `TRIG` pin. The sound wave travels until it hits an object and reflects back. The module's `ECHO` pin goes high for the duration between sending and receiving the pulse. 

**Core features:**

- **Range:** Typically 2 cm to 400 cm (depending on environment)
- **Accuracy:** 3 mm to 1 cm under good conditions
- **Interface:** Digital `TRIG` (input) and `ECHO` (output) pins
- **Operating voltage:** 5V 

**Pinout and connections:**

- **VCC:** 5V power (HC-SR04 requires 5V)
- **GND:** Ground
- **TRIG:** Trigger input (digital output from ESP32)
- **ECHO:** Echo output 

.. note::

 - The HC-SR04 expects a 10 µs HIGH on `TRIG` to start a measurement, then the `ECHO` pin will go HIGH for a time proportional to distance.
 - For stable readings, take multiple samples and average; filter out spikes and timeouts.
 - Avoid using the sensor outdoors in heavy wind, rain, or when surfaces are highly absorbent to ultrasound.


8. IR Receiver Module and Remote Control
----------------------------------------

.. image:: _static/Component/10.ir.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

This kit includes an infrared (IR) receiver module  and a simple IR remote. The receiver demodulates the 38 kHz carrier used by most remote controls and outputs a digital pulse train representing the encoded button commands.

**Working principle:**

- The remote sends coded IR signals (commonly NEC, RC5, or other protocols) by modulating a 38 kHz IR carrier. The receiver module filters and demodulates the carrier and outputs the raw pulse timings on its `OUT` pin. A microcontroller decodes these timings into button codes using an IR decoding library.

**Core features:**

- **Interface:** Simple digital `OUT` pin with active-low pulses
- **Carrier:** Typically 38 kHz (module tuned to this frequency)
- **Power:** 3.3V – 5V 

**Pinout and connections:**

- **VCC:** 3.3V or 5V supply
- **GND:** Ground
- **OUT / DATA:** Digital output — connect to an ESP32 GPIO (with interrupt capability) to capture pulse timings

.. note::

 - Use an IR decoding library (e.g., `IRremote` for Arduino/ESP32 or `esp32_ir` ports) to decode protocols like NEC.
 - The output is typically active LOW (pulse when carrier detected); you should capture pulse lengths to decode the command.
 - Avoid pointing multiple remotes at the receiver simultaneously.
 - Infrared works best in line-of-sight; sunlight and strong IR sources can interfere.


9. Joystick Module
------------------

.. image:: _static/Component/11.joystick.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

A typical joystick module in the kit provides two potentiometers (X and Y axes) and a push-button (switch) on the joystick knob. It behaves like two analog inputs plus one digital switch.

**Working principle:**

- Each axis is a variable resistor (potentiometer) that outputs a voltage between 0V and VCC proportionally to the joystick position. The button is a simple momentary switch.

**Core features:**

- **Axes:** Two analog outputs (`VRx`, `VRy`) for X and Y
- **Switch:** One digital output (`SW`) activated when the joystick is pressed
- **Power:** 3.3V or 5V compatible 

.. image:: _static/Component/11.joystick2.png
   :width: 500
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Pinout and connections:**

- **VCC:** 3.3V
- **GND:** Ground
- **VRx:** Analog output for X-axis → ESP32 ADC pin (e.g., `GPIO32`)
- **VRy:** Analog output for Y-axis → ESP32 ADC pin (e.g., `GPIO33`)
- **SW:** Digital switch output → any GPIO with pull-up (e.g., `GPIO14`)

.. note::

 - Read `VRx` and `VRy` using ADC and map the values to a joystick range (e.g., 0..1023 or 0..4095). Center position often yields a mid-scale value; apply deadzone filtering to ignore small variations.
 - Configure the `SW` pin with an internal pull-up and detect LOW for pressed state.
 - For better stability, apply simple smoothing (moving average) to analog readings.

----
