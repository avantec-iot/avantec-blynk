TA65-Blynk Protocol
#######################


Overview
========

TA65-Blynk is an implementation of BLYNK protocol client.

.. uml::

  node "\nBlynk server\n" as BKSrv {
  }

  node "Mobile Apps \n(iOS apps, \nAndroid apps)" as BKApp {
  }

  node "Device \n(TA65-XX)\n" as BKDev {
  }

  BKSrv <-down-> BKApp : **TA65-Blynk Protocol**
  BKSrv <-down-> BKDev : **TA65-Blynk Protocol**


BLYNK Special
=============

* Curently support plain TCP
* Curently **NOT** support **SSL/TLS**

See :ref:`blynk-security`.


Flow Chart
==========

.. tip::

  The range of values for message fields is referred to in the following sections, see  `Virtual Pins`_.


APP.CTRL.01 Control Mode (System On/Off)
----------------------------------------

.. uml::
   
  caption   APP.CTRL.01 Control Mode (System On/Off)
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == System On/Off  in [Apps] ==
  BKApp  ->  BKDev: PIN_CTRL_MODE(int/1:system on/0:system off)

  == System On/Off in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_CTRL_MODE(Push, int/1:system on/0:system off)

See `PIN_CTRL_MODE`_.


APP.CTRL.02 Adjust Setpoint Temperature
----------------------------------------

.. uml::
   
  caption   APP.CTRL.02 Adjust Setpoint Temperature
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Adjust Setpoint Temperature in [Apps] ==
  BKApp  ->  BKDev: PIN_SETPOINT_VALUE(float)
  BKApp  <-  BKDev: PIN_SETPOINT_STATUS(Push, int/1:override/0:false)

  == Reset Setpoint Temperature in [Apps] ==
  BKApp  ->  BKDev: PIN_SETPOINT_STATUS(int--/1:override--/0:false)
  BKApp  <-  BKDev: PIN_SETPOINT_VALUE(Push, float)

  == Adjust Setpoint Temperature in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SETPOINT_VALUE(Push, float)
  BKApp  <-  BKDev: PIN_SETPOINT_STATUS(Push, int/1:override/0:false)

See `PIN_SETPOINT_VALUE`_, `PIN_SETPOINT_STATUS`_.


APP.CTRL.03 Display Temperatue
------------------------------

.. uml::
   
  caption   APP.CTRL.03 Display Temperatue
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  BKApp  <-  BKDev: PIN_ROOM_TEMP(Push, float)
  
  note over BKApp, BKDev
  **PIN_FLOOR_TEMP** is **floor temperature** in TA65-HP 
  and **change over sensor temperature** in TA65-FC.
  end note

See `PIN_ROOM_TEMP`_.


APP.CTRL.04 Fan Mode & Fan Status
---------------------------------

.. uml::
   
  caption   APP.CTRL.04 Fan Mode & Fan Status
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Fan Mode in [Apps] ==
  BKApp  ->  BKDev: PIN_FAN_MODE(int/0:auto/1:low/2:med/3:high)
  

  == Fan Mode in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_FAN_MODE(Push, int/0:auto/1:low/2:med/3:high)

  == Fan Status in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_FAN_STATUS(Push, int/0:off/1:low/2:med/3:high)

  note over BKApp, BKDev
  This feature does not apply to TA65-FH, only TA65-FC.
  end note

See `PIN_FAN_MODE`_, `PIN_FAN_STATUS`_.


APP.PRG.01 Time Program
-------------------------

.. uml::
   
  caption   APP.PRG.01 Time Program
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Program Mode in [Apps] ==
  BKApp  ->  BKDev: PIN_PRG_MODE \n (int/0:no program/1:1 day/2:sun_mon~fri_sat/3:7 days)
  BKApp  <-  BKDev: PIN_PRG_DAYS(int/0/0~1/0~6) \n PIN_PRG_TIMExx(int,0<=xx<=3) \n PIN_PRG_SETPOINTxx(float,0<=xx<=3)
  
  == Program Days in [Apps] ==
  BKApp  ->  BKDev: PIN_PRG_DAYS(int/0/0~1/0~6)
  BKApp  <-  BKDev: PIN_PRG_TIMExx(int,0<=xx<=3) \n PIN_PRG_SETPOINTxx(float,0<=xx<=3)

  == Program Time/Setpoint in [Apps] ==
  BKApp  ->  BKDev: PIN_PRG_TIMExx(int,0<=xx<=3) \n PIN_PRG_SETPOINTxx(float,0<=xx<=3)


  == Program Mode in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_PRG_MODEE \n (int/0:no program/1:1 day/2:sun_mon~fri_sat/3:7 days) \n PIN_PRG_DAYS(Push,int/0/0~1/0~6) \n PIN_PRG_TIMxx(Push,int,0<=xx<=3) \n PIN_PRG_SETPOINTxx(Push, float,0<=xx<=3)

  == Program a setpoint in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_PRG_TIMExx(Push,int,0<=xx<=3) \n PIN_PRG_SETPOINTxx(Push, float,0<=xx<=3)

  == Program four setpoints of a day in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_PRG_TIME0(Push,int), PIN_PRG_TIME1(Push,int), \n PIN_PRG_TIME2(Push,int), PIN_PRG_TIME3(Push,int) \n PIN_PRG_SETPOINT0(Push,float), PIN_PRG_SETPOINT1(Push,float), \n PIN_PRG_SETPOINT2(Push,float), PIN_PRG_SETPOINT3(Push,float)

  note over BKApp, BKDev
  The number of setpoints is 28 (7 days * 4).
  end note

See `PIN_PRG_MODE`_, `PIN_PRG_DAYS`_.

See `PIN_PRG_SETPOINTxx`_, `PIN_PRG_SETPOINT0`_, `PIN_PRG_SETPOINT1`_, `PIN_PRG_SETPOINT2`_, `PIN_PRG_SETPOINT3`_.

See `PIN_PRG_SETPOINTxx`_, `PIN_PRG_SETPOINT0`_, `PIN_PRG_SETPOINT1`_, `PIN_PRG_SETPOINT2`_, `PIN_PRG_SETPOINT3`_.


APP.PRG.02 Next Schedule Status
----------------------------------

.. uml::
   
  caption   APP.PRG.02 Next Schedule Status
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  BKApp  <-  BKDev: PIN_PRG_NEXT_ENABLED(Push, int)
  BKApp  <-  BKDev: PIN_PRG_NEXT_DAYSTIME(Push, string/DaysOfWeek+Time+AM/PM)
  BKApp  <-  BKDev: PIN_PRG_NEXT_SETPOINT(Push, float)

See `PIN_PRG_NEXT_ENABLED`_, `PIN_PRG_NEXT_DAYSTIME`_, `PIN_PRG_NEXT_SETPOINT`_.


APP.Energy.01 Energy Counter
----------------------------

.. uml::
   
  caption   APP.Energy.01 Energy Counter
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Push Energy Expense in [TA65-XX] ==
  BKDev  <-]      : Timer(Energy Inquire, 10 seconds)
  BKApp  <-  BKDev: PIN_ENERGY_CONSUMPTION(Push,float)
  BKApp  <-  BKDev: PIN_ENERGY_EXPENSE(Push,float)

See `PIN_ENERGY_CONSUMPTION`_, `PIN_ENERGY_EXPENSE`_.


APP.Energy.02 Energy Settings
------------------------------

.. uml::
   
  caption   APP.Energy.02 Energy Settings
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Adjust Energy Settings in [Apps] ==
  BKApp  ->  BKDev: PIN_ENERGY_POWER(float) \n PIN_ENERGY_POWER_UNIT(string) \n PIN_ENERGY_PRICE(float) \n PIN_ENERGY_CURRENCY_UNIT(string)

  == Adjust Energy Settings in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_ENERGY_POWER(Push, float) \n PIN_ENERGY_POWER_UNIT(Push,string) \n PIN_ENERGY_PRICE(Push, float) \n PIN_ENERGY_CURRENCY_UNIT(Push,string)

See `PIN_ENERGY_POWER`_, `PIN_ENERGY_POWER_UNIT`_.

See `PIN_ENERGY_PRICE`_, `PIN_ENERGY_CURRENCY_UNIT`_.


APP.SET.01 Modify Device Name / Description
--------------------------------------------

.. uml::
   
  caption   APP.SET.01  Modify Device Name/Description
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Modify device name  in [Apps] ==
  BKApp  ->  BKSrv: {Set Device Name(string)}

  == Get device name from [Blynk Server] ==
  BKApp  <-  BKSrv: {Get Device Name(string)}


APP.SET.02 DateTime (Weekdays & Time) Sync
-------------------------------------------

.. uml::
   
  caption   APP.SET.02  DateTime(Weekdays&Time) Sync
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Set timezone in [Apps] ==
  BKApp  ->  BKSrv: {Set Timeone}

  == Sync time from server in [TA65-XX] ==
  BKApp  ->  BKDev: PIN_REQUEST_SYNC_TIME
  BKSrv  <-  BKDev: { RTC sync }
  BKSrv  ->  BKDev: InternalPinRTC{datetime}

  == Sync time from server in in [TA65-XX] ==
  BKDev  <-]      : Timer(Datetime_sync, 24 hours)
  BKSrv  <-  BKDev: { RTC sync }
  BKSrv  ->  BKDev: InternalPinRTC{datetime}

See `PIN_REQUEST_SYNC_TIME`_.


APP.SET.03 Temperature Unit
----------------------------

.. uml::
   
  caption   APP.SET.03 Temperature Unit
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Temperature Unit in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_TEMP_UNIT(int/0:℃/1:℉)

  == Configure Temperature Unit in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_TEMP_UNIT(Push, int/0:℃/1:℉)

See `PIN_SET_TEMP_UNIT`_.


APP.SET.04 Time Display Format (12/24 Hours)
---------------------------------------------

.. uml::
   
  caption   APP.SET.04 Time Display Format(12/24Hours)
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Time Display Format in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_TIME_FORMAT(int/0:12hours/1:24hours)

  == Configure Time Display Format in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_TIME_FORMAT(Push, int/0:12hours/1:24hours)

See `PIN_SET_TIME_FORMAT`_.


APP.SET.05 System Mode (Heating / Cooling)
-------------------------------------------

.. uml::
   
  caption   APP.SET.05 System Mode (Heating / Cooling)
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure System Mode in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_SYSTEM_MODE(int/0:heat/1:cool)

  == Configure System Mode in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_SYSTEM_MODE(Push, int/0:heat/1:cool)

  note over BKApp, BKDev
  This feature is only applicable to TA65-FH only, not TA65-FC.
  end note

See `PIN_SET_SYSTEM_MODE`_.


APP.SET.06 Sensor Mode
----------------------

.. uml::
   
  caption   APP.SET.06 Sensor Mode
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Sensor Mode in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_SENSOR_MODE(int/0:Internal/1:External/2:Combined)

  == Configure Sensor Mode in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_SENSOR_MODE(Push,int/0:Internal/1:External/2:Combined)

  note over BKApp, BKDev
  This feature is only applicable to TA65-FH only, not TA65-FC.
  end note

See `PIN_SET_SENSOR_MODE`_.


APP.SET.07 Internal Sensor Temperature Offset
---------------------------------------------

.. uml::
   
  caption   APP.SET.07 Internal Sensor Temperature Offset
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Internal Temperature Offset in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_INTERNAL_OFFSET(float/-5℃~5℃, Step:0.5)

  == Configure Internal Temperature Offset in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_INTERNAL_OFFSET(push, float/-5℃~5℃, Step:0.5)

See `PIN_SET_INTERNAL_OFFSET`_.


APP.SET.08 Floor Temperature Limited
-------------------------------------

.. uml::
   
  caption   APP.SET.08 Floor Temperature Limited
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Floor Temperature Offset in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_FLOOR_LIMITED(float/20~40℃, Step:0.5)

  == Configure Floor Temperature Offset in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_FLOOR_LIMITED(Push,float/20~40℃, Step:0.5)

  note over BKApp, BKDev
  This feature is only applicable to TA65-FH only, not TA65-FC.
  end note

See `PIN_SET_FLOOR_LIMITED`_.


APP.SET.09.01 Switching Differential Heating
---------------------------------------------

.. uml::
   
  caption   APP.SET.09.01 Switching Differential Heating
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Switching Differential Heating in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_SWITCHING_DIFF(float/0.5~4℃, Step:0.5)

  == Configure Switching Differential Heating in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_SWITCHING_DIFF(Push, float/0.5~4℃, Step:0.5)

  note over BKApp, BKDev
  This feature is only applicable to TA65-FH only, not TA65-FC.
  end note

See `PIN_SET_SWITCHING_DIFF`_.


APP.SET.09.02 Switching Differential Cooling
---------------------------------------------

.. uml::
   
  caption   APP.SET.09.02 Switching Differential Cooling
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Switching Differential Cooling in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_SWITCHING_DIFF_COOL(float/0.5~4℃, Step:0.5)

  == Configure Switching Differential Cooling in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_SWITCHING_DIFF_COOL(Push, float/0.5~4℃, Step:0.5)

  note over BKApp, BKDev
  This feature is only applicable to TA65-FH only, not TA65-FC.
  end note

See `PIN_SET_SWITCHING_DIFF_COOL`_.


APP.SET.10 Adaptive Control
----------------------------

.. uml::
   
  caption   APP.SET.10 Adaptive Control
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Adaptive Control in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_ADAPTIVE_CTRL(int/0:disable/1:enable)

  == Configure Adaptive Control in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_ADAPTIVE_CTRL(Push, int/0:disable/1:enable)

  note over BKApp, BKDev
  This feature is only applicable to TA65-FH only, not TA65-FC.
  end note

See `PIN_SET_ADAPTIVE_CTRL`_.


APP.SET.11 Froce Ventilation
----------------------------

.. uml::
   
  caption   APP.SET.11 Froce Ventilation
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Froce Ventilation in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_FROCE_VENTILATION(int/0:disabled/1:enabled)

  == Configure Froce Ventilation in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_FROCE_VENTILATION(Push, int/0:disabled/1:enabled)

  note over BKApp, BKDev
  This feature does not apply to TA65-FH, only TA65-FC.
  end note

See `PIN_SET_FROCE_VENTILATION`_.


APP.SET.12 Change Over Mode
----------------------------

.. uml::
   
  caption   APP.SET.12 Change Over Mode
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Change Over Mode in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_CHANGEOVER_MODE(int/0:Heat /1:Cool/2:Auto)

  == Configure Change Over Mode in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_CHANGEOVER_MODE(Push, int/0:Heat /1:Cool/2:Auto)

  note over BKApp, BKDev
  This feature does not apply to TA65-FH, only TA65-FC.
  end note

See `PIN_SET_CHANGEOVER_MODE`_.


APP.SET.13.01 Change Over Temp Heating
-----------------------------------------

.. uml::
   
  caption   APP.SET.13.01 Change Over Temp Heating
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Change Over Temp Heating in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_CHANGEOVER_TEMP_HEAT(float/27~40℃, Step:0.5)

  == Configure Change Over Temp Heating in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_CHANGEOVER_TEMP_HEAT(Push, float/27~40℃, Step:0.5)

  note over BKApp, BKDev
  This feature does not apply to TA65-FH, only TA65-FC.
  end note

See `PIN_SET_CHANGEOVER_TEMP_HEAT`_.


APP.SET.13.02 Change Over Temp Cooling
-----------------------------------------

.. uml::
   
  caption   APP.SET.13.02 Change Over Temp Cooling
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  == Configure Change Over Temp Cooling in [Apps] ==
  BKApp  ->  BKDev: PIN_SET_CHANGEOVER_TEMP_COOL(float/10~25℃, Step:0.5)

  == Configure Change Over Temp Cooling in [TA65-XX] ==
  BKApp  <-  BKDev: PIN_SET_CHANGEOVER_TEMP_COOL(Push, float/10~25℃, Step:0.5)

  note over BKApp, BKDev
  This feature does not apply to TA65-FH, only TA65-FC.
  end note

See `PIN_SET_CHANGEOVER_TEMP_COOL`_.


APP.MISC.01 Wi-Fi RSSI(Signal strength)
----------------------------------------

.. uml::
   
  caption   APP.MISC.01 Wi-Fi RSSI(Signal strength)
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  BKDev  <-]      : Timer(WiFi Status, 5 seconds)
  BKApp  <-  BKDev: PIN_MISC_WIFI_STATUS(Push, rssi/int)

See `PIN_MISC_WIFI_STATUS`_.


APP.MISC.02 Firmware version
------------------------------

.. uml::
   
  caption   APP.MISC.02 Firmware version
  hide footbox

  participant "Mobile Apps"     as BKApp order 10
  participant "Blynk Server"    as BKSrv order 20 
  participant "Device(TA65-XX)" as BKDev order 30

  BKDev  <-]      : Power On
  BKApp  <-  BKDev: PIN_MISC_FW_VERSION(Push, String/ESP version+ LAPIS version)

See `PIN_MISC_FW_VERSION`_.


Virtual Pins
============

PIN_CTRL_MODE
-------------

PIN_SETPOINT_VALUE
------------------

PIN_SETPOINT_STATUS
-------------------

PIN_FAN_STATUS
--------------

PIN_FAN_MODE
------------

PIN_ROOM_TEMP
-------------

PIN_FLOOR_TEMP
--------------

.. list-table:: TA65 Virtual Pins about Control
   :widths: auto
   :header-rows: 1

   * - Virutal Pin Name
     - Pin |br| Value
     - Data |br| Type
     - Data |br| Value
     - Widget |br| Property
     - Timer
     - Push |br| Rquest
     - TA65 |br| -FC
     - TA65 |br| -FH
     - Memo

   * - PIN_CTRL_MODE
     - V1
     - integer
     - 0:system off, |br| 1:system on
     - 
     - 
     - Push
     - ●
     - ●
     - Control |br| mode

   * - PIN_SETPOINT_VALUE
     - V2
     - float
     - 
     - min, |br| max, |br| label(°C/°F), |br| step
     - 
     - Push
     - ●
     - ●
     - Adjust |br| Setpoint |br| Temperature

   * - PIN_SETPOINT_STATUS
     - V3
     - integer
     - 0:false |br| 1:override
     - 
     - 
     - Push
     - ●
     - ●
     - Adjust |br| Setpoint |br| Temperature

   * - PIN_FAN_STATUS
     - V4
     - integer
     - 0:off |br| 1:low |br| 2:med |br| 3:high
     - 
     - 
     - Push
     - ●
     - 
     - 

   * - PIN_FAN_MODE
     - V5
     - integer
     - 0:auto |br| 1:low |br| 2:med |br| 3:high
     - 
     - 
     - Push
     - ●
     - 
     - 

   * - PIN_ROOM_TEMP
     - V16
     - float
     - 
     - min, |br| max, |br| label(°C/°F)
     - 10 seconds
     - Push
     - ●
     - ●
     - Display |br| Temperature

   * - PIN_FLOOR_TEMP
     - V17
     - float
     - 
     - min, |br| max, |br| label(°C/°F)
     - 10 seconds
     - Push
     - ●
     - ●
     - Display |br| Temperature

.. # define a hard line break for HTML
.. |br| raw:: html

   <br/>


PIN_PRG_NEXT_ENABLED
--------------------

PIN_PRG_NEXT_SETPOINT
---------------------

PIN_PRG_NEXT_DAYSTIME
---------------------

PIN_PRG_MODE
------------

PIN_PRG_DAYS
------------

PIN_PRG_TIMExx
--------------

PIN_PRG_TIME0
-------------

PIN_PRG_TIME1
-------------

PIN_PRG_TIME2
-------------

PIN_PRG_TIME3
-------------

PIN_PRG_SETPOINTxx
------------------

PIN_PRG_SETPOINT0
-----------------

PIN_PRG_SETPOINT1
-----------------

PIN_PRG_SETPOINT2
-----------------

PIN_PRG_SETPOINT3
-----------------

.. list-table:: TA65 Virtual Pins about Program
   :widths: auto
   :header-rows: 1

   * - Virutal Pin Name
     - Pin |br| Value
     - Data |br| Type
     - Data |br| Value
     - Widget |br| Property
     - Timer
     - Push |br| Rquest
     - TA65 |br| -FC
     - TA65 |br| -FH
     - Memo

   * - PIN_PRG_NEXT_ENABLED
     - V40
     - integer
     - 0:false, |br| 1:true
     - 
     - 
     - Push
     - ●
     - ●
     - Next |br| Schedule |br| Status

   * - PIN_PRG_NEXT_SETPOINT
     - V41
     - float
     - 
     - label(°C/°F)
     - 
     - Push
     - ●
     - ●
     - Next |br| Schedule |br| Status

   * - PIN_PRG_NEXT_DAYSTIME
     - V42
     - string
     - DaysOfWeek + |br| Time + |br| AM/PM
     - 
     - 
     - Push
     - ●
     - ●
     - Next |br| Schedule |br| Status

   * - PIN_PRG_MODE
     - V48
     - integer
     - 0:no programe |br| 1:1 day |br| 2:1+5+1 |br| 3:7 days
     - 
     - 
     - Push
     - ●
     - ●
     - Time |br| Programs

   * - PIN_PRG_DAYS
     - V49
     - integer
     - 0/0~1/0~6
     - labels
     - 
     - Push
     - ●
     - ●
     - Time |br| Programs

   * - PIN_PRG_TIME0 |br| PIN_PRG_TIME1 |br| PIN_PRG_TIME2 |br| PIN_PRG_TIME3
     - V50 |br| V51 |br| V52 |br| V53
     - integer
     - 
     - labels
     - 
     - Push
     - ●
     - ●
     - Time |br| Programs

   * - PIN_PRG_SETPOINT0 |br| PIN_PRG_SETPOINT1 |br| PIN_PRG_SETPOINT2 |br| PIN_PRG_SETPOINT3
     - V60 |br| V61 |br| V62 |br| V63
     - float
     - 
     - label, |br| lables(°C/°F)
     - 
     - Push
     - ●
     - ●
     - Time |br| Programs


PIN_ENERGY_CONSUMPTION
------------------------

PIN_ENERGY_EXPENSE
------------------------

PIN_ENERGY_POWER
------------------------

PIN_ENERGY_POWER_UNIT
------------------------

PIN_ENERGY_PRICE
------------------------

PIN_ENERGY_CURRENCY_UNIT
------------------------

.. list-table:: TA65 Virtual Pins about Energy
   :widths: auto
   :header-rows: 1

   * - Virutal Pin Name
     - Pin |br| Value
     - Data |br| Type
     - Data |br| Value
     - Widget |br| Property
     - Timer
     - Push |br| Rquest
     - TA65 |br| -FC
     - TA65 |br| -FH
     - Memo

   * - PIN_ENERGY_CONSUMPTION
     - V84
     - float
     - 
     - 
     - 1 hour
     - Push
     - ●
     - ●
     - Energy |br| Counter

   * - PIN_ENERGY_EXPENSE
     - V85
     - float
     - 
     - 
     - 1 hour
     - Push
     - ●
     - ●
     - Energy |br| Counter

   * - PIN_ENERGY_POWER
     - V86
     - float
     - 
     - 
     -
     - Push
     - ●
     - ●
     - Energy |br| Setting

   * - PIN_ENERGY_POWER_UNIT
     - V87
     - string |br| 7 bytes
     - 
     - 
     -
     - Push
     - ●
     - ●
     - Energy |br| Setting

   * - PIN_ENERGY_PRICE
     - V88
     - float
     - 
     - 
     -
     - Push
     - ●
     - ●
     - Energy |br| Setting

   * - PIN_ENERGY_CURRENCY_UNIT
     - V89
     - string |br| 7 bytes
     - 
     - 
     -
     - Push
     - ●
     - ●
     - Energy |br| Setting


PIN_SET_TEMP_UNIT
-----------------

PIN_SET_TIME_FORMAT
-------------------

PIN_SET_SYSTEM_MODE
-------------------

PIN_SET_SENSOR_MODE
-------------------

PIN_SET_INTERNAL_OFFSET
-----------------------

PIN_SET_FLOOR_LIMITED
---------------------

PIN_SET_SWITCHING_DIFF
----------------------

PIN_SET_SWITCHING_DIFF_COOL
---------------------------

.. list-table:: TA65 Virtual Pins about Settings-A
   :widths: auto
   :header-rows: 1

   * - Virutal Pin Name
     - Pin |br| Value
     - Data |br| Type
     - Data |br| Value
     - Widget |br| Property
     - Timer
     - Push |br| Rquest
     - TA65 |br| -FC
     - TA65 |br| -FH
     - Memo

   * - PIN_SET_TEMP_UNIT
     - V91
     - integer
     - 0:℃ |br| 1:℉
     - 
     - 
     - Push
     - ●
     - ●
     - Temperature |br| Unit

   * - PIN_SET_TIME_FORMAT
     - V92
     - integer
     - 0:12 hours  |br| 1:24 hours
     - 
     - 
     - Push
     - ●
     - ●
     - Time |br| Display |br| Format

   * - PIN_SET_SYSTEM_MODE
     - V93
     - integer
     - 0:heat |br| 1:cool
     - 
     - 
     - Push
     - 
     - ●
     - 

   * - PIN_SET_SENSOR_MODE
     - V94
     - integer
     - 0:Internal Sensor |br|  1:External Sensor  |br|  2:Combined mode,  |br| (1,2 only for heating)
     - labels
     - 
     - Push
     - 
     - ●
     - 

   * - PIN_SET_INTERNAL_OFFSET
     - V95
     - integer
     - -5℃~5℃, |br| Step:0.5
     - labels
     - 
     - Push
     - ●
     - ●
     - Internal |br| Temp |br| Offset

   * - PIN_SET_FLOOR_LIMITED
     - V96
     - integer
     - 20℃~40℃, |br| Step:0.5
     - labels
     - 
     - Push
     - 
     - ●
     - Floor |br| Temp |br| Limited

   * - PIN_SET_SWITCHING_DIFF
     - V97
     - integer
     - 0.5℃~42℃,  |br| Step:0.5
     - labels??
     - 
     - Push
     - ●
     - ●
     - Switching |br| Differential |br| Heating 

   * - PIN_SET_SWITCHING_DIFF_COOL
     - V104
     - integer
     - 0.5℃~4℃, |br| Step:0.5
     - labels??
     - 
     - Push
     - ●
     - ●
     - Switching |br| Differential |br| Cooling


PIN_SET_ADAPTIVE_CTRL
---------------------

PIN_REQUEST_SYNC_TIME
---------------------

PIN_SET_FROCE_VENTILATION
-------------------------

PIN_SET_CHANGEOVER_MODE
-----------------------
 
PIN_SET_CHANGEOVER_TEMP_HEAT
----------------------------

PIN_SET_CHANGEOVER_TEMP_COOL
----------------------------

.. list-table:: TA65 Virtual Pins about Settings-B
   :widths: auto
   :header-rows: 1

   * - Virutal Pin Name
     - Pin |br| Value
     - Data |br| Type
     - Data |br| Value
     - Widget |br| Property
     - Timer
     - Push |br| Rquest
     - TA65 |br| -FC
     - TA65 |br| -FH
     - Memo

   * - PIN_SET_ADAPTIVE_CTRL
     - V98
     - integer
     - 0:Disable |br| 1:Enable
     - 
     - 
     - Push
     - 
     - ●
     - Adaptive |br| Control

   * - PIN_REQUEST_SYNC_TIME
     - V99
     - integer
     - Weekdays |br| & Time
     - 
     - 
     - Push
     - ●
     - ●
     - DateTime |br| Sync

   * - PIN_SET_FROCE_VENTILATION
     - V100
     - integer
     - 0:disabled |br| 1:enabled
     - 
     - 
     - Push
     - ●
     - 
     - Froce |br| Ventilation

   * - PIN_SET_CHANGEOVER_MODE
     - V101
     - integer
     - 0:Heat |br| 1:Cool |br| 2:Auto
     - 
     - 
     - Push
     - ●
     - 
     - Change |br| Over |br| Mode

   * - PIN_SET_CHANGEOVER_TEMP_HEAT
     - V102
     - integer
     - 27~40℃, |br| Step:0.5
     - 
     - 
     - Push
     - ●
     - 
     - Change |br| Over |br| Temp |br| Heating

   * - PIN_SET_CHANGEOVER_TEMP_COOL
     - V103
     - integer
     - 10~25℃, |br| Step:0.5
     - 
     - 
     - Push
     - ●
     - 
     - Change |br| Over |br| Temp |br| Cooling


PIN_MISC_WIFI_STATUS
--------------------

PIN_MISC_FW_VERSION
-------------------

.. list-table:: TA65 Miscellaneous Virtual Pins
   :widths: auto
   :header-rows: 1

   * - Virutal Pin Name
     - Pin |br| Value
     - Data |br| Type
     - Data |br| Value
     - Widget |br| Property
     - Timer
     - Push |br| Rquest
     - TA65 |br| -FC
     - TA65 |br| -FH
     - Memo

   * - PIN_MISC_WIFI_STATUS
     - V124
     - integer
     - rssi, |br| 0~10
     - 
     - 10 seconds
     - Push
     - ●
     - ●
     - Wi-Fi RSSI |br| (Signal strength)

   * - PIN_MISC_FW_VERSION
     - V125
     - string
     - ESP version + |br| LAPIS version
     - 
     - 
     - Push
     - ●
     - ●
     - Firmware |br| version
