.. Using-TA65-with-Blynk documentation master file, created by
   sphinx-quickstart on Fri Jul 17 18:11:15 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Using TA65 Thermostat with Blynk
================================

`Avantec`__ provides the latest ``TA65`` Thermostat networking solution. This solution needs to be used with the ``Blynk`` software platform.

.. __: http://www.avantec.com.hk/

.. uml::
   :align: center

   node "\nBlynk server\n" as BKSrv {
   }

   node "Mobile Apps \n(iOS Apps, \nAndroid Apps)" as BKApp {
   }

   node "Device \n (TA65) \n" as BKDev {
   }

   BKSrv <-down-> BKApp
   BKSrv <-down-> BKDev



* TA65
   The latest series of Thermostat produced by Avantec. The Wi-Fi models include ``TA65-FC`` and ``TA65-FH`` (Their firmware ID are ``TA65-FC-BK`` and ``TA65-FH-BK``).

.. image:: /_static/usage/ta65.png
   :width: 30%
   :align: center

* Blynk
   `Blynk`__ is a hardware-agnostic IoT platform with white-label mobile apps, private clouds, device management, data analytics, and machine learning.

   Join the most popular IoT platform to connect your devices to the cloud, design apps to control them, analyze telemetry data, and manage your deployed products at scale.

   **Drag-n-drop beautiful IoT apps in minutes**

   Choose from a variety of pre-designed widgets to build native iOS and Android apps that control electronics, monitor sensor data, get notifications, and much more.

   **Turn your prototypes into commercial IoT solutions**

   In one click: export your project into a beautiful branded app that already works with your products. Then publish it to App Store™️ and Google Play™️

   **Start building a prototype for free**, then choose a `subscription plan`__ to launch your commercial IoT product.

   *The only legal way to use Blynk products and services for commercial purposes is to sign up for a paid licensing subscription plan*.

.. __: https://blynk.io/
.. __: https://blynk.io/pricing

.. You can find out more about our all the :doc:`/features` in these pages.


.. toctree::
   :maxdepth: 3
   :caption: Getting Started

   /intro/get-started


.. toctree::
   :maxdepth: 3
   :caption: Usage

   Blynk </usage/blynk-usage>
   TA65-FC Wi-Fi Thermostat  </usage/ta65-fc-manual>
   TA65-FH Wi-Fi Thermostat  </usage/ta65-fh-manual>
   Avantec Thermostat Project  </usage/avantec-usage>


.. toctree::
   :maxdepth: 2
   :caption: Customization

   /customize/customization


.. toctree::
   :maxdepth: 4
   :caption: Protocol

   /protocol/blynk-protocol
   /protocol/ta65-blynk-protocol


.. toctree::
   :maxdepth: 2
   :caption: About

   avantec


Indices and tables
==================

* :ref:`genindex`
* :ref:`search`

.. * :ref:`modindex`
