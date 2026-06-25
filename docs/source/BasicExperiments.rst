Basic Experiments
=================

.. image:: _static/project/BASIC/0.basic.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

This introductory chapter guides you through the process of building fun, interactive devices—capable of visual, physical, and auditory responses—using a carefully selected range of common and engaging components such as LEDs, buzzers, buttons, and sensors. Through a series of hands-on experiments that progress from simple to complex, you will quickly familiarize yourself with hardware wiring, programming logic, and debugging techniques, rapidly transforming from a mere observer into a maker.

----

1. LED Blinking 
----------------

- In this experiment, you will learn how to control an LED using the ESP32 microcontroller. You will write a simple program to make the LED blink at regular intervals, introducing you to basic programming concepts and GPIO pin control.

**Materials Needed:**

 - ESP32 Development Board
 - LED
 - Resistor (220Ω)
 - Breadboard and Jumper Wires

**Wiring Diagram:**

.. image:: _static/project/BASIC/1.BLINK_LED.png
   :width: 700
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Wiring Table**

.. list-table:: 
   :header-rows: 1
   :widths: 10 20 20 25 25

   * - No.
     - Component
     - Pin
     - Connect to
     - Note
   * - 1
     - LED
     - Anode (long leg)
     - GPIO 4
     - series 220Ω
   * - 1
     - LED
     - Cathode (short leg)
     - GND
     -

**Example code:**

.. code-block:: cpp

 // Define the LED connection pin
 #define LED_PIN 2

 void setup()
 {
 // Set GPIO2 to output mode
 pinMode(LED_PIN, OUTPUT);
 }

 void loop()
 {
 // Turn on the LED
 digitalWrite(LED_PIN, HIGH);

 // Delay for 1 second
 delay(1000);

 // Turn off the LED
 digitalWrite(LED_PIN, LOW);

 // Delay for 1 second
 delay(1000);
 }


**Display Effect:**

.. image:: _static/project/BASIC/1.blinking.gif
   :width: 500
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- The LED light will continuously turn on and off at one-second intervals—lighting up, dimming, lighting up again, dimming again—creating a ceaseless breathing or flashing rhythm.


----


2. PWM LED
----------

- This experiment is a classic introductory project on analog signal acquisition and PWM output control. It aims to teach how to combine the ESP32's ADC analog input with PWM pulse width modulation output to achieve stepless adjustment of light brightness using a physical knob.

**Materials Needed:**

 - ESP32 Development Board
 - LED
 - potentiometer 10k 
 - Breadboard and Jumper Wires

**Wiring Diagram:**

.. image:: _static/project/BASIC/3.PWM_LED.png
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
     - Potentiometer
     - Left
     - GND
   * - 1
     - Potentiometer
     - Middle (Wiper)
     - GPIO 4
   * - 1
     - Potentiometer
     - Right
     - 3.3V
   * - 2
     - LED
     - Anode (long leg)
     - GPIO 5
   * - 2
     - LED
     - Cathode (short leg)
     - GND

**Example code:**

.. code-block:: cpp

 // Define the LED connection pin
 #define LED_PIN 2

 void setup()
 {
 // Set GPIO2 to output mode
 pinMode(LED_PIN, OUTPUT);
 }

 void loop()
 {
 // Turn on the LED
 digitalWrite(LED_PIN, HIGH);

 // Delay for 1 second
 delay(1000);

 // Turn off the LED
 digitalWrite(LED_PIN, LOW);

 // Delay for 1 second
 delay(1000);
 }


**Display Effect:**

.. image:: _static/project/BASIC/3.PWM_LED2.gif
   :width: 500
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Rotating the potentiometer clockwise gradually brightens the LED until it reaches its brightest point.

- Rotating the potentiometer counter-clockwise gradually dims the LED until it goes completely off.
----


3. Button LED
-------------

- This experiment aims to teach two core programming techniques: key debouncing and state toggling. Two independent keys will control the on/off state of red and yellow LEDs respectively. 

- You will learn how to use **`digitalRead()`** to capture the rising edge trigger of a key **(i.e., the instant it's pressed)** , and how to implement the logic of "state toggling once per press" using a state flag **(bool variable)** . 

- Simultaneously, simple software delay debouncing is incorporated into the code, allowing you to understand the signal jitter problem caused by mechanical keys at the instant they are pressed and its solution.

**Materials Needed:**

 - ESP32 Development Board
 - Button (2 PCS)
 - LED (Yellow、Red)
 - Resistor (220Ω)
 - Breadboard and Jumper Wires

**Wiring Diagram:**

.. image:: _static/project/BASIC/2.buttonled.png
   :width: 700
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

**Wiring Table**

.. list-table:: 
   :header-rows: 1
   :widths: 10 20 20 25 25

   * - No.
     - Component
     - Pin
     - Connect to
     - Note
   * - 1
     - Red LED
     - Anode (long leg)
     - GPIO 13
     - series 220Ω
   * - 1
     - Red LED
     - Cathode (short leg)
     - GND
     -
   * - 2
     - Yellow LED
     - Anode (long leg)
     - GPIO 12
     - series 220Ω
   * - 2
     - Yellow LED
     - Cathode (short leg)
     - GND
     -
   * - 3
     - Button 1
     - Either pin
     - 3.3V
     -
   * - 3
     - Button 1
     - Other pin
     - GPIO 18
     - with 10kΩ to GND
   * - 4
     - Button 2
     - Either pin
     - 3.3V
     -
   * - 4
     - Button 2
     - Other pin
     - GPIO 19
     - with 10kΩ to GND



**Example code:**

.. raw:: html

   <div style="background: #f8f9fa; border: 1px solid #ddd; border-radius: 6px; overflow: hidden;">
   <div id="code-container-dht" style="max-height: 420px; overflow: hidden; position: relative; background: #f5f5f0;">

.. code-block:: cpp

 // LED pins
 #define RED_LED     13
 #define YELLOW_LED  12

 // Button pins
 #define BUTTON1     18
 #define BUTTON2     19

 // LED state variables
 bool redState = false;
 bool yellowState = false;

 // Save previous button states
 bool lastButton1 = LOW;
 bool lastButton2 = LOW;

 void setup()
 {
    pinMode(RED_LED, OUTPUT);
    pinMode(YELLOW_LED, OUTPUT);
    pinMode(BUTTON1, INPUT);
    pinMode(BUTTON2, INPUT);
 }

 void loop()
 {
    // Read current button states
    bool currentButton1 = digitalRead(BUTTON1);
    bool currentButton2 = digitalRead(BUTTON2);

    // ===== Button 1: Control red LED =====
    // Detect rising edge (LOW to HIGH: button pressed)
    if (lastButton1 == LOW && currentButton1 == HIGH)
    {
        redState = !redState;
        digitalWrite(RED_LED, redState);
        delay(200);  // Simple debounce
    }

    // ===== Button 2: Control yellow LED =====
    if (lastButton2 == LOW && currentButton2 == HIGH)
    {
        yellowState = !yellowState;
        digitalWrite(YELLOW_LED, yellowState);
        delay(200);  // Simple debounce
    }

    // Save current states
    lastButton1 = currentButton1;
    lastButton2 = currentButton2;
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

.. image:: _static/project/BASIC/2.buttonled2.png
   :width: 500
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Pressing button 1 (GPIO18): The red LED's state toggles—it turns on if currently off, and off if currently on, toggling each time it's pressed.

- Pressing button 2 (GPIO19): The yellow LED follows the same toggling logic, without interfering with each other.

----