Connect TA65 to Blynk server by end users
=========================================

.. tip::

   Only for end users.


See `Dynamic Auth Token Provisioning`__.

.. __: http://help.blynk.cc/en/articles/511238-dynamic-auth-token-provisioning

This article is about **Blynk.Inject™️** – a framework for device provisioning that includes **Firmware API** and a **Mobile Connection Wizard** to simplify life for your customers when getting device online and for you, as a manufacturer (or developer).

Dynamic Auth Token Provisioning
#################################

Dynamic Auth Token Provisioning is used for devices that can connect to the Internet over WiFi and can act as an WiFi Access Point (AP). For example: ESP8266 or ESP32. 

Major advantage of Dynamic Auth Token Provisioning is that **you don't have to flash Auth Token** to every device before it's delivered to your customers. Which means that you can use same firmware on every device  while all the provisioning processes is handled by Blynk.Inject™️.

Blynk.Inject™️ Firmware API covers
###################################

* connection management
* obtaining Auth Token from the server
* status indication of hardware state (AP, connecting, connected, error, etc)
* credentials storage
* credentials reset 


How Blynk.Inject works for your customers
#########################################

Experience starts when your customer unboxes your product and turn it on for the first time. They would also need your app installed on their smartphones. 

* Product automatically switches into a WiFi Access Point (AP) mode and starts broadcasting its' own WiFi network. This state is indicated with an LED color and animation (or you can provide your own indication)
* User opens your app and follows the **Connection Wizard UI** which guides them through the steps to connect the product to their WiFi network.

.. figure:: /_static/intro/connect_ta65_to_blynk_by_end_user/wi-fi-wizard.png
    :alt: Connection Wizard UI example
    :align: center

    Connection Wizard UI example

All the steps of Connection Wizard are pre-designed for you. You can customize product icons, provide your own instructions and translate them to a different language.

**This wizard was extensively tested for usability and connectivity failures and provides 99.9% reliability.**

**Wizard is included in every app** `made with Blynk`__

.. __: http://help.blynk.cc/publishing-apps-made-with-blynk/export-project-made-with-blynk-into-a-standalone-app


.. note::

    Due to differences with WiFi connection management on Android and iOS, Connection Wizard has different UI on these operating systems

* Your customers will choose the WiFi network they want to connect the product to (for example, their home WiFi network) and input password.
* Then they will connect their smartphones to the WiFi network your product broadcasts.


After that Device Provisioning starts
######################################

1. Your app passes WiFi credentials (SSID and password) to the device
2. Your app requests a new Auth Token from the server and passes it to the device
3. Credentials and Auth Token is stored in the device's memory (EEPROM)
4. Device automatically reboots itself
5. Device uses the stored WiFi credentials to connect to the user's WiFi network. It no longer acts as an Access Point.
6. Device uses the stored Auth Token to connect to Blynk Cloud
7. New Device appears in the app and is ready to be used


Examples
###########

Here is a couple of examples on how Blynk works with real products: **Plaato** and **CamaCooler**

.. raw:: html

    <iframe width="640" height="360" src="https://www.youtube.com/embed/4RGAn0tlrHs" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

.. raw:: html

    <iframe width="640" height="360" src="https://www.youtube.com/embed/GPneDxWvp4U" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
