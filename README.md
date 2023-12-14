# Roastuino-Popper-Roaster

[>>> New Fluid Bed Roaster Project <<<](https://github.com/cyberelectronics/Roastuino-Fluidbed-Roaster/tree/main)

There are a lot of Popper Coffee Roaster modifications on the internet.

This is just another one with PID controller (theoretically this controller can be used on other roasters as well), based on Arduino .

I used the same Espressuino hardware with small modifications.

The software is a mix of 4 Arduino sketches:

1. [BBCC (BareBones Coffee Controller) project by Tim Hirzel](http://playground.arduino.cc/Main/BarebonesPIDForEspresso)
2. [Espressuino modifications](https://github.com/cyberelectronics/Espressuino-Gaggia-Classic)
3. [Serial interface for Roastlogger software used from Roastlogger Project](http://homepage.ntlworld.com/green_bean/coffee/roastlogger/roastlogger.htm)   
4. [Arduino Library for MAX31855 thermocouple amplifier by Adafruit](http://learn.adafruit.com/thermocouple/using-a-thermocouple)

**Operation modes:**

1. Manual Mode
2. Auto Mode
3. PC Control Mode
4. Settings Mode

1) **In Manual Mode** we can set the Target Temperature and Fan Speed only, the Time setting is unavailable in this Mode. The Fan Speed controlled using PWM signal (100% on LCD or in Roastlogger means max. RPM, this can be used only with DC motors). The Manual Mode can be activated by pressing the + (INCrement) or – (DECrement) button, while the system is in the STOP state. Using the START / STOP button we can START the AUTO Mode OR STOP the Manual Mode AND AUTO Mode. Heater and Servo Output controlled by the PID algorithm. There is a Total Elapsed Time counter in the upper right side of the LCD, which will be activated instantly when the Manual Mode starts. Temperature values can be displayed on Roastlogger or Artisan.

2) **In Auto Mode** we can set multiple RAMPs (max. 9 ) with different Temperature and Time settings. The FAN Speed also can be set individually for each RAMP. All RAMPs will be executed sequentially one by one, until the next RAMP has 0:00 time setting. In this case (Roasting Finished) the system will STOP the Heater and Servo Output and will START the FAN at full power (100%) for cooling the beans.  The Total Elapsed Time counter will be activated automatically after the first RAMP period (preheat, this can be Enabled or Disabled in the main_test.ino project file). In Auto Mode we can change all of the SET values anytime (without Saving it to the EEPROM) by pressing the + or – and SELECT buttons (the linearized RAMP speed / angle, also will be changed in real time). The Auto Mode can be turned ON/OFF anytime, using the Start / Stop button. Temperature values can be displayed on Roastlogger or Artisan. In the Roasting video you can see a difference between Target Temperature and Real Time Measurement of Bean Temperature at the end of each Ramp. Now this is solved by increasing the Proportional component from PID and adding a customizable OFFSET value (difference in Celsius, between Target Temp and Real T. Bean Temp.)

3) **In PC Control Mode** we can display and set manually the FAN and Heater Power using the Roastlogger interface. Also we can program the Roastlogger for Automatic Control (RAMPs and PID). In PC Control Mode the Arduino PID is deactivated, so Roastlogger can take full control on Heater Power / Servo Output and FAN Power. The Total Elapsed Time counter, will be activated automatically, after you hit the “LOAD Beans” button in the Roastlogger software. PC Control Mode is activated by default, at startup (when the controller is turned ON). This can be turned OFF by pressing any button on the Controller (this will turn OFF only the control side from PC, the controller are still sending Temp. and FAN datas to the PC in every second).

4) **In Settings Mode** you can set each variable for each RAMP in part then Save in EEPROM by pressing the Start / Stop button, when the “Save” message will appear for a second, on the LCD. Just turn OFF the controller for EXIT from this mode.


This controller supports both Thermistors ( EPCOS – B57560G104F – THERMISTOR 100K 1% ) and K Thermocouples (sensor type can be set in main_test.ino project file). For Thermocouples you need the interface board with MAX31855 chip, from Adafruit.

There is a Roast-Counter information displayed at the System startup. This counter will be incremented at every Roast cycle, longer than 3 minutes (default). You can RESET this counter to 0, by pressing the Start/Stop and + (INCrement) buttons while you Turn ON the controller.

There are 4 different Profiles, each with 9 different programmable RAMPs. Each Profile can be loaded individually at the startup by pressing S4 or S3 or S2 or S1 button while you turn ON the controller.

It is possible to connect the controller to an Android device using Bluetooth (cheap HC-06 BTmodule) and TC4 Android App (by Brad, with modifications made by another friend). The PID values (P-I-D) can be changed from both BBCC Plotter and TC4 App (sending ex. PRO=90 or INT=0 or DER=0 ). Chart examples can be found here.

Note: Disconnect the Bluetooth module while uploading the sketch.

**Protections:**

 - One or both of the Thermistors  or Thermocouples unconnected or shorted -> ERR messages on the LCD and Heater Power = 0%.
 - ON Time limit exceeded (30 min. default) -> Heater Power = 0%.
--------------------------------------------------------
**Videos:**
- **First, software test:** https://youtu.be/JrLBjgsHxrg
- **First Hardware Test:** https://youtu.be/ahwki331GuM
- **Full Roast video in Auto Mode:** https://youtu.be/lz_-ME9JabY
- **Settings Mode:** https://youtu.be/SavY8bqy9bM
- **Modified TC4 Android App Demo (final version will be different):** https://youtu.be/n37bFz3xK2w
- **First Crack and Second Crack:** https://youtu.be/Wp2C1tZjCPQ

**Circuit Diagram :**
- Roastuino SCH v1.0
 

**Arduino and Android Source Code :**
- Roastuino Source v1.0
 
 If you want to roast bigger quantities (200g-400g) try this newer project: Fluid Bed Roaster

!!! Warning !!!
 NEVER LEAVE ROASTER UNATTENDED WHILE IN OPERATION !!!
Always ensure there is a circular motion of the green beans in the roaster by adjusting the FAN speed, otherwise the beans can catch fire !!! First time make some tests without turning on the Heater! 
Roastlogger will Start with 100% Heat and 0% FAN! Do not plug in (~230V) the Heater while you didn’t set the Heat Power (slider) to 0% ! 
Build and / or use at your own risk !!!

- Default PID settings:  P = 80;  I = 0;  D = 0;

**My default Roast Profile:**

- Ramp 1 – 3:00 min – 160C
- Ramp 2 – 3:00 min – 200C
- Ramp 3 – 3:00 min – 229C
then cool the beans with 100% FAN while bean temp drops below 60C (aprox. 3 min)

**P4 Connector pinout:**

1. Power Supply Input  (+7V to +15V)
2. GND ( – Common Ground)
3. Vcc OUT (+5V Common)
4. PWM FAN signal OUT (to N channel Power MOSFET)
5. Vcc OUT (+5V NC for DB9 connector)
6. SSR Heater (to SSR)
7. Vcc OUT (+5V NC for DB9)
8. Temp. sensor input 1 (to thermistor 1 – Air – Optional [100K resistor needed if not used])
9. Vcc OUT (+5V NC for DB9)
10. Temp. sensor input 2  (to thermistor 2 – Bean)

**P2 Connector pinout:**

1. GND (NC for DB9)
2. AREF (NC for DB9)
3. +5V   (NC for DB9)
4. AUX4 (NC for DB9)
5. Servo Control OUT ( for mechanically controlled Heaters)
6. AUX6 (I/O)
7. RX (NC for DB9 – TTL)
8. TX (NC for DB9 – TTL)
 

**Push Button functions:**

- S4 – ” START / STOP″ – Start / Stop Manual and Auto Mode
- S3 – ” + ” – Increment Value
- S2 – ”  –  ″ – Decrement Value
- S1 – ” SELECT ” – Select variable which you want to change

**Other functions available at startup (keep pressed then turn ON the controller):**

- S4 – “PROFILE 1 ” – Load PROFILE 1 with 9 RAMP settings
- S3 – “PROFILE 2 ” – Load PROFILE 2 with 9 RAMP settings
- S2 – “PROFILE 3 ” – Load PROFILE 3 with 9 RAMP settings
- S1 – “PROFILE 4 ” – Load PROFILE 4 with 9 RAMP settings
- S3 AND S4 – “RESET RC” – Reset Roast Counter to 0
- S3 AND S2 – “BBCC PLOT” – Enable com. with BBCC Plotter – PID tuning (until Reset)
- S1 AND S2 – “SETTINGS” – Start Settings Mode (until Reset)

**Other Softwares and drivers needed for this project:**

   - [Arduino](http://arduino.cc/en/Main/Software) (tested with 1.0)
   - [Processing](http://www.processing.org/download/) (for BBCC Plotter, optional)
   - BBCC Plotter (for PID tuning, optional)
   - [Roastlogger](http://homepage.ntlworld.com/green_bean/coffee/roastlogger/roastlogger.htm) (for PC Control and temp. curves display, optional)
   - [Artisan](http://code.google.com/p/artisan/) (only for temperature curves display, optional)
   - [Oiginal TC4 Android App](http://code.google.com/p/tc4-shield/downloads/detail?name=TC4%20Android%20App_2_1.zip&can=2&q=) (HC-06 BTmodule needed, optional)
   - [Modified TC4 Android App with CSV Save function](https://github.com/cyberelectronics/Roastuino-Fluidbed-Roaster/tree/main/Docu) (HC-06 BTmodule needed, optional)
   - [Adafruit MAX31855 Thermocouple Arduino library](https://learn.adafruit.com/thermocouple/using-a-thermocouple)
   - CP2102 USB driver (not needed for commercial Arduino (Deumilanove / Uno) boards)
