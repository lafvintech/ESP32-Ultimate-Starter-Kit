Arduino IDE Getting Started Tutorial
====================================

.. image:: _static/arduino/1.arduinoide.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>


The Arduino IDE is an open-source programming tool officially provided by Arduino, supporting C/C++ development. 

It provides users with simple and intuitive code writing, compiling, and uploading functions, making it easy to burn programs to development boards such as Arduino and ESP32. 

The IDE includes extensive built-in example code and supports library file extensions, allowing developers to quickly access drivers for various sensors and modules, enabling a rich set of hardware interaction features.

With its cross-platform support (Windows, macOS, and Linux), the Arduino IDE is widely used in education, makerspaces, and IoT development, making it an essential tool for both beginners and advanced embedded development learners.

----

1. Install Arduino IDE
-------------------------

This section will guide you through installing the **Arduino IDE** on Windows, macOS, and Linux systems.  


Install Arduino IDE on Windows
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1.1 Visit the official Arduino website and go to the software download page: `Download Arduino IDE <https://www.arduino.cc/en/software/>`_

   .. image:: _static/arduino/2.arduino_install3.png
      :alt: Arduino IDE official website
      :align: center

1.2 Select the version that matches your computer configuration, then click the **Download** button to begin.  

.. note::

   - The Arduino IDE is updated frequently. To ensure compatibility, it is recommended to download the **latest official version**.

1.3 Run the installer by double-clicking the downloaded ``arduino-ide_xxxx.exe`` file.  

1.4 Read and accept the **License Agreement**.  

.. image:: _static/arduino/3.Install_Arduino_IDE.png
      :alt: License Agreement window
      :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

1.5 Select the desired installation options.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/4.Install_Arduino_IDE.png
      :alt: Installation options
      :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

1.6 Choose the installation path. It is recommended to install the software on a **non-system drive**.  

.. raw:: html

   <div style="margin-top: 30px;"></div>


.. image:: _static/arduino/5.Install_Arduino_IDE.png
      :alt: Installation path
      :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

1.7 Click **Install** and wait for the process to complete. Finally, click **Finish**.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/6.Install_Arduino_IDE.png
      :alt: Installation finished
      :align: center

----

Install Arduino IDE on MacOS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Double-click the downloaded ``arduino-ide_xxxx.dmg`` file.  

- Drag and drop **Arduino IDE.app** into the **Applications** folder.  

- After a few seconds, you will see Arduino IDE installed successfully.  

   .. image:: _static/arduino/7.Install_Arduino_IDE.png
      :width: 800
      :alt: Arduino IDE installation on macOS
      :align: center

----

Install Arduino IDE on Linux
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- For Linux users, please follow the official tutorial for Arduino IDE 2.0 installation: `Linux Installation Guide <https://docs.arduino.cc/software/ide-v2/tutorials/getting-started/ide-v2-downloading-and-installing#linux>`_

----

Open the Arduino IDE
~~~~~~~~~~~~~~~~~~~~

When you open Arduino IDE for the first time:  

- The software will automatically install the **Arduino AVR Boards**, built-in libraries, and other required files.  

.. image:: _static/arduino/8.Install_Arduino_IDE.png
      :alt: First startup window
      :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>


- During installation, your **firewall** or **security center** may ask for permission to install drivers. Please allow all requests.  

.. raw:: html

   <div style="margin-top: 30px;"></div>


.. image:: _static/arduino/9.Install_Arduino_IDE.png
      :alt: Driver installation prompt
      :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. note::

   - If some installations fail due to network issues, simply **reopen the Arduino IDE** and it will continue the remaining installation steps.  
   - The **Output Window** will not appear automatically after setup. It will only open when you click **Verify** or **Upload**.  

----

.. _Install Serial Port Tool:

2. Install Serial Port Tool
------------------------

This kit uses an ESP32 board with a CP2102 USB-to-UART bridge. Ensure the CP2102 driver is installed on your computer before connecting the board, or the serial port will not be detected. Connect the board, press Win+X to open Device Manager, and verify the driver is installed. If not, use the link below to download and install it.

.. image:: _static/arduino/10.CP2102.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Click here to access the official driver download page `download CP210X <https://www.silabs.com/software-and-tools/usb-to-uart-bridge-vcp-drivers?tab=downloads>`_

.. video:: _static/arduino/driver_ins.mp4
    :width: 100%

- Please watch the video above for detailed download and installation instructions.

----

3. Install ESP32 Core Board 
--------------------------

Add Additional Boards Manager URL
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Open the Arduino IDE, click **File → Preferences** in the upper left corner, and copy and paste the following address into the *Additional Board Manager URLs* input box.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

- After entering the URL, click **OK**.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Copy and paste the following link into it:

.. raw:: html

   <div style="margin-top: 10px; margin-bottom: 15px;">
     <div style="margin-bottom: 6px;"><code>https://espressif.github.io/arduino-esp32/package_esp32_index_cn.json</code></div>
     <button type="button"
             onclick="navigator.clipboard.writeText('https://espressif.github.io/arduino-esp32/package_esp32_index_cn.json').then(() => alert('Link copied!')).catch(() => alert('Copy failed, please copy manually'))"
             style="padding: 6px 12px; cursor: pointer; border: 1px solid #d62728; border-radius: 4px; background: #d62728; color: #ffffff;">
       Copy Link
     </button>
   </div>

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/18.URL.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/19.URL.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/20.URL.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. admonition:: Precaution
   :class: note

   - After completing this step, you need to close and reopen the Arduino IDE.

----

Download the  ESP32 Core Package 
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Open the Arduino IDE, click the second icon on the left to open the **BOARDS MANAGER** page.  

   .. image:: _static/arduino/21.ESP32_CORE.png
      :width: 600
      :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Enter **ESP32** in the search box and press Enter.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Find the core package titled *esp32 by Espressif Systems*, select version **3.2.0** from the drop-down menu, and click **Install** to download and install it.  


.. image:: _static/arduino/22.ESP32_CORE.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Please wait for the download progress bar in the lower right corner to complete.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/23.ESP32_CORE.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- When the download is complete, the message **Successfully installed platform esp32:3.2.0** will be displayed.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/24.ESP32_CORE.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Check if the installation is successful:  

 Click **Tools → Board → esp32** to check whether an ESP32 development board is available for selection.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/25.ESP32_CORE.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. admonition:: Precaution
   :class: note

   - We recommend installing ESP32 Core Package version **3.2.0**, or using version **3.0 or later**.  

   - Older versions may be incompatible with the libraries used in this tutorial, causing program errors.  

   - If you have an earlier version installed, uninstall it and then reinstall version **3.2.0** of the ESP32 Core Package.  

----

.. _Add Libraries:

4. Add Libraries
----------------

- Arduino libraries can significantly simplify the development process.  
- They encapsulate commonly used functions and hardware driver code, allowing users to simply call ready-made functions without writing complex low-level code from scratch.  
- For example, the **LiquidCrystal_I2C** library allows users to drive an LCD1602 display with just a few lines of code.  
- A wealth of community-provided third-party libraries also allows for quick integration with various sensors and modules.  
- These library functions make it easy to interact with hardware and expand Arduino's functionality.

----

Download Libraries
~~~~~~~~~~~~~~~~~~

- We've compiled all the libraries necessary to run this suite. 

- Please click the link below to download them and follow the instructions to complete the installation:  `Download libraries <https://www.dropbox.com/scl/fo/syf1zstu58f4xlcld2nss/ACJOi93PcIafo5yGabrprDA?rlkey=hoc2undykymrxac7z8nclpk9u&st=el86zaw9&dl=1>`_

- Unzip the downloaded library file. The library file storage path is **Code and Libraries** → **Libraries** . Open it and confirm that it contains the library file shown in the figure below. 

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/26.lib.png
   :width: 700
   :align: center

----

Import Libraries
~~~~~~~~~~~~~~~~

- Open the Arduino IDE and click **Sketch → Include Library → Add .ZIP Library**.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/27.lib.png
   :width: 700
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- In the pop-up window, locate the folder of the library you just downloaded and unzipped, select it, and click **Open** to complete the import.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/28.lib.png
   :width: 700
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- If the library file is imported successfully, the Arduino IDE output window will display the message: *Library installed*.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/31.lib.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. admonition:: Precaution
   :class: note

   - Arduino IDE does not support importing multiple libraries at once; you must import one library at a time.  
   - If a library file already exists, a prompt will appear asking whether to overwrite it. It is recommended to confirm overwrite to avoid program errors caused by different library versions.  

.. image:: _static/arduino/29.lib.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Verify that the library was imported successfully:  

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Click **Sketch → Include Library**, scroll down to **Contributed Libraries**, and check whether the library files we provided are listed.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

.. image:: _static/arduino/28.lib3.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

----

Download Libraries Using Arduino IDE
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- You can also download required libraries directly using the Arduino IDE.  

.. list-table::
   :widths: 30 40 30
   :header-rows: 1

   * - Library Header
     - Description / Purpose
     - Installation Method
   * - ``#include <DHT.h>``
     - Driver for DHT11/DHT22 temperature and humidity sensors
     - Library Manager: search ``DHT sensor library`` (by Adafruit)
   * - ``#include <ESP32Servo.h>``
     - Servo control library optimized for ESP32 (replaces standard Servo library)
     - Library Manager: search ``ESP32Servo``
   * - ``#include <FastLED.h>``
     - High-performance library for controlling LED strips and matrices
     - Library Manager: search ``FastLED``
   * - ``#include <IRremote.h>``
     - Infrared signal sending and receiving library
     - Library Manager: search ``IRremote`` (check ESP32 compatibility)
   * - ``#include <Adafruit_GFX.h>``
     - Universal graphics rendering library for OLED/LCD displays
     - Library Manager: search ``Adafruit GFX Library``
   * - ``#include <Adafruit_SSD1306.h>``
     - Driver library for SSD1306 OLED displays
     - Library Manager: search ``Adafruit SSD1306``
   * - ``#include <AsyncTCP.h>``
     - Asynchronous TCP communication library for ESP32
     - Library Manager: search ``AsyncTCP``
   * - ``#include <ESPAsyncWebServer.h>``
     - Asynchronous web server library for ESP32
     - Library Manager: search ``ESPAsyncWebServer``
   * - ``#include <Adafruit_MPU6050.h>``
     - Driver for MPU6050 6-axis sensor (accelerometer + gyroscope)
     - Library Manager: search ``Adafruit MPU6050``
   * - ``#include <Adafruit_Sensor.h>``
     - Unified sensor abstraction layer for Adafruit sensors (dependency)
     - Automatically installed as a dependency with Adafruit sensor libraries
   * - ``#include <Arduino_JSON.h>``
     - Library for parsing and generating JSON data
     - Library Manager: search ``Arduino_JSON``
   * - ``#include <MFRC522.h>``
     - Driver library for MFRC522 RFID reader/writer module
     - Library Manager: search ``MFRC522`` (by miguelbalboa)


- On the right side of the Arduino IDE interface, click the **Library Manager** icon.  

.. raw:: html

   <div style="margin-top: 30px;"></div>

- Enter keywords in the search box to find the required library and click **Install** to download.  

.. image:: _static/arduino/32.lib.png
   :width: 600
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

----

5. Summary
-----------

In this chapter, you learned how to set up the Arduino IDE for ESP32 development。

- Install the necessary board package
- Configure the correct board and port
- Add the serial port driver, and manage libraries for your projects. 

These steps provide a solid foundation for writing, compiling, uploading, and debugging ESP32 programs successfully.

----
