Blynk Protocol
##############################


Introduction
============


.. uml::

   node "\nBlynk Server\n" as BKSrv {
   }

   node "Mobile Apps \n(iOS Apps, \nAndroid Apps)" as BKApp {
   }

   node "Device \n(TA65)\n" as BKDev {
   }

   BKSrv <-down-> BKApp : **BLYNK Protocol**
   BKSrv <-down-> BKDev : **BLYNK Protocol**


Protocol
==========

See :ref:`blynk-main-operations`.


Virtual Pins
-------------

Virtual Pins is designed  to send any data from your microcontroller to the Blynk App and back.

See :ref:`virtual-pins`, :ref:`digital_analog_pins_control`.


* Virtual Pin data types
   * All Virtual Pin values are always sent as Strings and there are no practical limits on the data that can be sent. 
   * To interpret incoming data as **Integers**, **Floats**, **Doubles** and **Strings** use.

   See :ref:`virtual-pin-data-types`.

* Widget properties
   * Four widget properties are supported - **color**, **label**, **min**, **max** for all widgets.
   * Widget specific properties.

   See :ref:`change-widget-properties`.



Send data from app to hardware
-------------------------------

See :ref:`send-data-from-app-to-hardware`.


.. uml::

   /'title  Send data from app to hardware'/
   hide footbox

   participant "Mobile Apps"   as BKApp  order 10
   participant "Blynk Server"  as BKSrv order 20 
   participant "Device (TA65)" as BKDev order 30

   /'== Send data from app to hardware =='/

   BKApp  ->  BKDev: Send data from app to hardware
   BKDev  ->  BKDev: BLYNK_WRITE(Virtual Pin)

Get data from hardware
------------------------

See :ref:`get-data-from-hardware`.


Perform requests by Widget
**************************

.. uml::

   /'title  Perform requests by Widget'/
   hide footbox

   participant "Mobile Apps"   as BKApp  order 10
   participant "Blynk Server"  as BKSrv order 20 
   participant "Device (TA65)" as BKDev order 30

   /'== Get data from hardware =='/

   BKApp  ->  BKDev: Perform requests by Widget(Virtual Pin)
   BKDev  ->  BKDev: BLYNK_READ(Virtual Pin)
   BKApp  <-  BKDev: Send data to App(Virtual Pin)


Pushing data from hardware
**************************

.. uml::

   /'title  Pushing data from hardware'/
   hide footbox

   participant "Mobile Apps"   as BKApp  order 10
   participant "Blynk Server"  as BKSrv order 20 
   participant "Device (TA65)" as BKDev order 30

   /'== Pushing data from hardware =='/

   BKDev  <-]  : Timeout of timer or Value changed
   BKApp  <-  BKDev: Pushing data from hardware(Virtual Pin)

