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

10. RC522 RFID Module
---------------------

.. image:: _static/Component/12.mfrc522.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The RC522 is a highly integrated 13.56MHz RFID reader/writer module based on the MFRC522 chip from NXP. It supports ISO/IEC 14443 A/MIFARE communication protocol and is widely used for contactless identification and access control applications. The module communicates with the host controller via SPI interface, enabling fast and reliable data exchange.

**Working principle:**

- The RC522 generates a 13.56MHz electromagnetic field through its antenna coil. When a compatible RFID card or tag enters this field, it draws power from the field and modulates the carrier wave to transmit its unique UID (Unique Identifier) and data. The RC522 demodulates the received signal, decodes the data, and makes it available through the SPI bus.

**Core features:**

- **Frequency:** 13.56MHz industrial, scientific, and medical (ISM) band
- **Supported cards:** MIFARE Classic 1K/4K, MIFARE Ultralight, NTAG series, and other ISO/IEC 14443 A compliant tags
- **Communication:** SPI interface (up to 10 MHz clock speed)
- **Read/Write:** Supports reading UID, reading and writing data blocks, and authentication
- **Operating distance:** Up to 5cm (depending on antenna design and tag type)
- **Power:** 3.3V supply (5V tolerant inputs with proper level shifting)

**Pinout and connections:**

- **SDA (SS):** SPI Slave Select → ESP32 GPIO (e.g., `GPIO5`)
- **SCK:** SPI Clock → ESP32 SCK (e.g., `GPIO18`)
- **MOSI:** SPI Master Out Slave In → ESP32 MOSI (e.g., `GPIO23`)
- **MISO:** SPI Master In Slave Out → ESP32 MISO (e.g., `GPIO19`)
- **IRQ:** Interrupt Request (optional) → ESP32 GPIO (e.g., `GPIO4`)
- **RST:** Reset pin → ESP32 GPIO (e.g., `GPIO22`)
- **3.3V:** 3.3V power supply
- **GND:** Ground

.. note::

 - The RC522 is a 3.3V logic device. While its inputs are 5V tolerant, it is recommended to use a level shifter for the SPI lines when interfacing with 5V logic microcontrollers to ensure reliable communication and long-term stability.
 - The module requires a stable 3.3V supply. Insufficient current (especially when reading/writing) may cause communication failures — ensure your power source can deliver at least 100mA.
 - Always call `PCD_Init()` in your initialization routine. If the `PCD_PerformSelfTest()` fails, check the wiring, especially the antenna connections.
 - When reading multiple cards, implement anti-collision logic (built into the MFRC522 library) to handle situations where multiple tags are in the field simultaneously.
 - The antenna tuning capacitors are pre‑adjusted on most modules — avoid modifying them unless you have proper RF equipment.

----

11. 0.96-inch OLED Display 
--------------------------

.. image:: _static/Component/13.0.96oled.png
   :width: 500
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

The 0.96-inch OLED display module based on the SSD1306 driver IC is a compact, high-contrast monochrome display widely used in embedded projects. Unlike traditional LCDs, OLED (Organic Light-Emitting Diode) technology emits its own light, eliminating the need for a backlight and enabling deeper blacks, faster response times, and lower power consumption. The module typically supports either I²C or SPI interface, with I²C being the most common variant in hobbyist kits.

**Working principle:**

- The SSD1306 is a single-chip CMOS OLED driver with a built-in controller and 128×64 pixels of SRAM display buffer. It receives pixel data from the host via I²C or SPI, stores it in its internal GDDRAM (Graphic Display Data RAM), and constantly refreshes the OLED panel by driving the individual pixels with the corresponding voltage levels. Each pixel is either ON (emitting light) or OFF, making the display inherently monochrome (usually white, blue, or yellow).

**Core features:**

- **Resolution:** 128 × 64 pixels (some variants are 128 × 32)
- **Display size:** 0.96 inches (diagonal)
- **Interface:** I²C (most common) or 4-wire SPI (4-Wire SPI)
- **Driver IC:** SSD1306
- **Viewing angle:** ~160° (excellent visibility from all directions)
- **Refresh rate:** Can exceed 100Hz (limited by communication speed)
- **Power consumption:** Extremely low — ~20mA during normal operation, less than 10µA in sleep mode
- **Operating voltage:** 3.0V ~ 5.5V (board usually includes a voltage regulator)


**Pinout and connections:**

- **VCC:** 3.3V ~ 5V power supply
- **GND:** Ground
- **SCL:** I²C Clock line → ESP32 SCL (e.g., `GPIO22`)
- **SDA:** I²C Data line → ESP32 SDA (e.g., `GPIO21`)

.. note::

 - I²C address is typically `0x3C` . 
 - For I²C, the maximum clock speed is 400kHz (Fast Mode) — 100kHz is more stable for longer wiring.
 - The display orientation can be changed via software — `setRotation()` or `flipScreenVertically()` functions are available in most libraries.

----


12. 4x4 Keypad
---------------

.. image:: _static/Component/14.keypad.png
   :width: 500
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

Microcontroller system, if the use of more keys such as electronic code lock, telephone keypad, etc. generally have at least 12 to 16 keys, usually using a matrix keyboard.


Matrix keypad is also called row keypad, it is a keypad with four I/O lines as row lines and four I/O lines as column lines. One key is set at each intersection of the row and column lines. Thus the number of keys on the keyboard is 4*4. This row and column keyboard structure can effectively improve the utilization of I/O ports in a microcontroller system.

Their contacts are accessed via a header suitable for connection with a ribbon cable or insertion into a printed circuit board. 
In some keypads, each button connects with a separate contact in the header, while all the buttons share a common ground.


More often, the buttons are matrix encoded, meaning that each of them bridges a unique pair of conductors in a matrix. 
This configuration is suitable for polling by a microcontroller, which can be programmed to send an output pulse to each of the four horizontal wires in turn. 
During each pulse, it checks the remaining four vertical wires in sequence, to determine which one, if any, is carrying a signal. 
Pullup or pulldown resistors should be added to the input wires to prevent the inputs of the microcontroller from behaving unpredictably when no signal is present.

----
