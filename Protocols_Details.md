# Protocols details
Here are detailed descriptions of every supported protocols (sorted by RF modules) as well as the available options for each protocol.

 If you want to see examples of model configurations see the [Models](docs/Models.md) page.
 
 The Deviation project (on which this project was based) have a useful list of models and protocols [here](http://www.deviationtx.com/wiki/supported_models).

## Useful notes and definitions
- **Extended limits supported** - A command range of -125%..+125% will be transmitted. Otherwise the default is -100%..+100% only.
- **Channel Order** - The channel order assumed in all the documentation is AETR. You can change this in the compilation settings. The module will take whatever input channel order and will rearrange them to match the output channel order required by the selected protocol. 
- **Italic numbers** are referring to protocol/sub_protocol numbers that you should use if the radio (serial mode only) is not displaying (yet) the protocol you want to access.
- **Autobind protocol**:

1. The transmitter will automatically initiate a bind sequence on power up.  This is for models where the receiver expects to rebind every time it is powered up. In these protocols you do not need to press the bind button at power up to bind, it will be done automatically. In case a protocol is not autobind but you want to enable it, change the "Autobind" (or "Bind at powerup" on OpenTX) setting to Y for the specific model/entry.
2. Enable Bind from channel feature:
   * Bind from channel can be globally enabled/disabled in _config.h using ENABLE_BIND_CH.
   * Bind from channel can be locally enabled/disabled by setting Autobind to Y/N per model for serial or per dial switch number for ppm.
   * Bind channel can be choosen on any channel between 5 and 16 using BIND_CH in _config.h. Default is 16.
   * Bind will only happen if all these elements are happening at the same time:
      - Autobind = Y
      - Throttle = LOW (<-95%)
      - Bind channel is going from -100% to +100%

* Additional notes:
  - It's recommended to combine Throttle cut with another button to drive the bind channel. This will prevent to launch a bind while flying...
  - Bind channel does not have to be assigned to a free channel. Since it only acts when Throttle is Low (and throttle cut active), it could be used on the same channel as Flip for example since you are not going to flip your model when Throttle is low... Same goes for RTH and such other features.
  - Using channel 16 for the bind channel seems the most relevant as only one protocol so far is using 16 channels which is FrSkyX. But even on FrSkyX this feature won't have any impact since there is NO valid reason to have Autobind set to Y for such a protocol.

## Protocol selection in PPM mode
The protocol selection is based on 2 parameters:
  * selection switch: this is the rotary switch on the module numbered from 0 to 15
      - switch position 0 is to select the Serial mode for er9x/ersky9x/OpenTX radio
      - switch position 15 is to select the bank
	  - switch position 1..14 will select the protocol 1..14 in the bank *X*
  * banks are used to increase the amount of accessible protocols by the switch. There are up to 5 banks giving acces to up to 70 protocol entries (5 * 14).  To modify or verify which bank is currenlty active do the following:
      - turn on the module with the switch on position 15
      - the number of LED flash indicates the bank number (1 to 5 flash)
	  - to go to the next bank, short press the bind button, this action is confirmed by the LED staying on for 1.5 sec

Here is the full protocol selection procedure:
1. turn the selection switch to 15
2. power up the module
3. the module displays the current bank by flashing the LED x number of times, x being between 1 and up to 5
4. a short press on the bind button turns the LED on for 1 sec indicating that the system has changed the bank
5. repeat operation 3 and 4 until you have reached the bank you want
6. power off
7. change the rotary switch to the desired position (1..14)
8. power on
9. enjoy

Notes:
  * **The protocol selection must be done before the module is turned on**
  * The protocol mapping based on bank + rotary switch position can be seen/modified at the end of the file [_Config.h](/Multiprotocol/_Config.h)**

## Serial mode
Serial mode is selected by placing the rotary switch to position 0 before power on of the radio.

# A7105 RF Module

## FLYSKY - *1*
Extended limits supported

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
A|E|T|R|CH5|CH6|CH7|CH8

Note that the RX ouput will be AETR.

### Sub_protocol Flysky - *0*

### Sub_protocol V9X9 - *1*
CH5|CH6|CH7|CH8
---|---|---|---
FLIP|LIGHT|PICTURE|VIDEO

### Sub_protocol V6X6 - *2*
CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12
---|---|---|---|---|---|---|---
FLIP|LIGHT|PICTURE|VIDEO|HEADLESS|RTH|XCAL|YCAL

### Sub_protocol V912 - *3*
CH5|CH6
---|---
BTMBTN|TOPBTN

### Sub_protocol CX20 - *4*
Model: Cheerson Cx-20

CH5|CH6|CH7
---|---|---

## FLYSKY AFHDS2A - *28*
Extended limits and failsafe supported

Telemetry enabled protocol:
 - by defaut using FrSky Hub protocol (for example er9x): RX&battery voltages and RX&TX RSSI
 - if using ersky9x and OpenTX: full telemetry information available

Option is used to change the servo refresh rate. A value of 0 gives 50Hz (min), 70 gives 400Hz (max). Specific refresh rate value can be calculated like this option=(refresh_rate-50)/5.

**RX_Num is used to give a number a given RX. You must use a different RX_Num per RX. A maximum of 16 AFHDS2A RXs are supported.**

If telemetry is incomplete (missing RX RSSI for example), it means that you have to upgrade your RX firmware to version 1.6 or later. You can do it from an original Flysky TX or using a STLink like explained in [this tutorial](https://www.rcgroups.com/forums/showthread.php?2677694-How-to-upgrade-Flysky-Turnigy-iA6B-RX-to-firmware-1-6-with-a-ST-Link).

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13|CH14
---|---|---|---|---|---|---|---|---|---|---|---|---|---
A|E|T|R|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13|CH14

Note that the RX ouput will be AETR.

### Sub_protocol PWM_IBUS - *0*
### Sub_protocol PPM_IBUS - *1*
### Sub_protocol PWM_SBUS - *2*
### Sub_protocol PPM_SBUS - *3*

## HUBSAN - *2*

Telemetry enabled for battery voltage and TX RSSI

Option=vTX frequency (H107D) 5645 - 5900 MHz

### Sub_protocol H107 - *0*
Autobind protocol

Models: Hubsan H102D, H107/L/C/D and H107P/C+/D+

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP|LIGHT|PICTURE|VIDEO|HEADLESS

### Sub_protocol H301 - *1*
Models: Hubsan H301

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
A|E|T|R|RTH|LIGHT|STAB|VIDEO

### Sub_protocol H501 - *2*
Models: Hubsan H501S, H122D, H123D

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13
---|---|---|---|---|---|---|---|---|----|----|----|----
A|E|T|R|RTH|LIGHT|PICTURE|VIDEO|HEADLESS|GPS_HOLD|ALT_HOLD|FLIP|FMODES

H122D: FLIP

H123D: FMODES -> -100%=Sport mode 1,0%=Sport mode 2,+100%=Acro

## BUGS - *41*
Models: MJX Bugs 3, 6 and 8

Autobind protocol

Telemetry enabled for RX & TX RSSI, Battery voltage good/bad

**RX_Num is used to give a number to a given model. You must use a different RX_Num per MJX Bugs. A maximum of 16 Bugs are supported.**

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10
---|---|---|---|---|---|---|---|---|---
A|E|T|R|ARM|ANGLE|FLIP|PICTURE|VIDEO|LED

ANGLE: angle is +100%, acro is -100%

***
# CC2500 RF Module

## CORONA - *37*
Models: Corona 2.4GHz FSS and DSSS receivers.

Extended limits supported

Option for this protocol corresponds to fine frequency tuning. This value is different for each Module and **must** be accurate otherwise the link will not be stable.
Check the [Frequency Tuning page](/docs/Frequency_Tuning.md) to determine it.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8

### Sub_protocol COR_V1 - *0*
Corona FSS V1 RXs

### Sub_protocol COR_V2 - *1*
Corona DSSS V2 RXs: CR8D, CR6D and CR4D

To bind V2 RXs you must follow the below procedure (original):
 - press the bind button and power on the RX
 - launch a bind from Multi -> the RX will blink 2 times
 - turn off the RX **and** TX(=Multi)
 - turn on the RX **first**
 - turn on the TX(=Multi) **second**
 - wait for the bind to complete -> the RX will flash, stop and finally fix
 - wait some time (more than 30 sec) before turning off the RX
 - turn off/on the RX and test that it can reconnect instantly, if not repeat the bind procedure

### Sub_protocol FD_V3 - *2*
FlyDream RXs like IS-4R and IS-4R0

## FRSKYV = FrSky 1 way - *25*
Models: FrSky receivers V8R4, V8R7 and V8FR.

Extended limits supported

Option for this protocol corresponds to fine frequency tuning. This value is different for each Module and **must** be accurate otherwise the link will not be stable.
Check the [Frequency Tuning page](/docs/Frequency_Tuning.md) to determine it.
 
CH1|CH2|CH3|CH4
---|---|---|---
CH1|CH2|CH3|CH4

## FRSKYD - *3*
Models: FrSky receivers D4R and D8R. DIY RX-F801 and RX-F802 receivers. Also known as D8.

Extended limits supported

Telemetry enabled for A0, A1, RSSI, TSSI and Hub

Option for this protocol corresponds to fine frequency tuning. This value is different for each Module and **must** be accurate otherwise the link will not be stable.
Check the [Frequency Tuning page](/docs/Frequency_Tuning.md) to determine it.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8

## FRSKYX - *15*
Models: FrSky receivers X4R, X6R and X8R. Also known as D16.

Extended limits and failsafe supported

Telemetry enabled for A1 (RxBatt), A2, RSSI, TSSI and Hub

Option for this protocol corresponds to fine frequency tuning. This value is different for each Module and **must** be accurate otherwise the link will not be stable.
Check the [Frequency Tuning page](/docs/Frequency_Tuning.md) to determine it.

### Sub_protocol CH_16 - *0*
FCC protocol 16 channels @18ms.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13|CH14|CH15|CH16
---|---|---|---|---|---|---|---|---|----|----|----|----|----|----|----
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13|CH14|CH15|CH16

### Sub_protocol CH_8 - *1*
FCC protocol 8 channels @9ms.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8

### Sub_protocol EU_16 - *2*
EU-LBT protocol 16 channels @18ms. Note that the LBT part is not implemented, the TX transmits right away.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13|CH14|CH15|CH16
---|---|---|---|---|---|---|---|---|----|----|----|----|----|----|----
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13|CH14|CH15|CH16

### Sub_protocol EU_8 - *3*
EU-LBT protocol 8 channels @9ms. Note that the LBT part is not implemented, the TX transmits right away.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8

## HITEC - *39*
Models: OPTIMA, MINIMA and MICRO receivers.

Extended limits supported

Option for this protocol corresponds to fine frequency tuning. This value is different for each Module and **must** be accurate otherwise the link will not be stable.
Check the [Frequency Tuning page](/docs/Frequency_Tuning.md) to determine it.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9

### Sub_protocol OPT_FW - *0*
OPTIMA RXs

Full telemetry available on ersky9x and OpenTX. This is still a WIP.

**The TX must be close to the RX for the bind negotiation to complete successfully**

### Sub_protocol OPT_HUB - *1*
OPTIMA RXs

Basic telemetry using FrSky Hub on er9x, ersky9x, OpenTX and any radio with FrSky telemetry support with RX voltage, VOLT2 voltage, TX RSSI and TX LQI. 

**The TX must be close to the RX for the bind negotiation to complete successfully**

### Sub_protocol MINIMA - *2*
MINIMA, MICRO and RED receivers

## SFHSS - *21*
Models: Futaba RXs and XK models.

Extended limits and failsafe supported

Option for this protocol corresponds to fine frequency tuning. This value is different for each Module and **must** be accurate otherwise the link will not be stable.
Check the [Frequency Tuning page](/docs/Frequency_Tuning.md) to determine it.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
A|E|T|R|CH5|CH6|CH7|CH8

***
# CYRF6936 RF Module

## DEVO - *7*
Extended limits and failsafe supported

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
A|E|T|R|CH5|CH6|CH7|CH8

Note that the RX ouput will be EATR.

Bind procedure using serial:
- With the TX off, put the binding plug in and power on the RX (RX LED slow blink), then power it down and remove the binding plug. Receiver should now be in autobind mode.
- Turn on the TX, set protocol = Devo with option=0, turn off the TX (TX is now in autobind mode).
- Turn on RX (RX LED fast blink).
- Turn on TX (RX LED solid, TX LED fast blink).
- Wait for bind on the TX to complete (TX LED solid).
- Make sure to set a uniq RX_Num value for model match.
- Change option to 1 to use the global ID.
- Do not touch option/RX_Num anymore.

Bind procedure using PPM:
- With the TX off, put the binding plug in and power on the RX (RX LED slow blink), then power it down and remove the binding plug. Receiver should now be in autobind mode.
- Turn on RX (RX LED fast blink).
- Turn the dial to the model number running protocol DEVO on the module.
- Press the bind button and turn on the TX. TX is now in autobind mode.
- Release bind button after 1 second: RX LED solid, TX LED fast blink.
- Wait for bind on the TX to complete (TX LED solid).
- Press the bind button for 1 second. TX/RX is now in fixed ID mode.
- To verify that the TX is in fixed mode: power cycle the TX, the module LED should be solid ON (no blink).
- Note: Autobind/fixed ID mode is linked to the RX_Num number. Which means that you can have multiple dial numbers set to the same protocol DEVO with different RX_Num and have different bind modes at the same time. It enables PPM users to get model match under DEVO.

## WK2X01 - *30*
Extended limits supported
Autobind protocol

Note: RX ouput will always be AETR independently of the input AETR, RETA...

### Sub_protocol WK2801 - *0*
Failsafe supported.

This roughly corresponds to the number of channels supported, but many of the newer 6-channel receivers actually support the WK2801 protocol. It is recommended to try the WK2801 protocol 1st when working with older Walkera models before attempting the WK2601 or WK2401 mode, as the WK2801 is a superior protocol. The WK2801 protocol supports up to 8 channels.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
A|E|T|R|CH5|CH6|CH7|CH8

Bind procedure using serial:
- With the TX off, put the binding plug in and power on the RX (RX LED slow blink), then power it down and remove the binding plug. Receiver should now be in autobind mode.
- Turn on the TX, set protocol = WK2X01, sub_protocol = WK2801 with option=0, turn off the TX (TX is now in autobind mode).
- Turn on RX (RX LED fast blink).
- Turn on TX (RX LED solid, TX LED fast blink).
- Wait for bind on the TX to complete (TX LED solid).
- Make sure to set a uniq RX_Num value for model match.
- Change option to 1 to use the global ID.
- Do not touch option/RX_Num anymore.

Bind procedure using PPM:
- With the TX off, put the binding plug in and power on the RX (RX LED slow blink), then power it down and remove the binding plug. Receiver should now be in autobind mode.
- Turn on RX (RX LED fast blink).
- Turn the dial to the model number running protocol protocol WK2X01 and sub_protocol WK2801 on the module.
- Press the bind button and turn on the TX. TX is now in autobind mode.
- Release bind button after 1 second: RX LED solid, TX LED fast blink.
- Wait for bind on the TX to complete (TX LED solid).
- Press the bind button for 1 second. TX/RX is now in fixed ID mode.
- To verify that the TX is in fixed mode: power cycle the TX, the module LED should be solid ON (no blink).
- Note: Autobind/fixed ID mode is linked to the RX_Num number. Which means that you can have multiple dial numbers set to the same protocol DEVO with different RX_Num and have different bind modes at the same time. It enables PPM users to get model match under DEVO.

### Sub_protocol WK2401 - *1*
The WK2401 protocol is used to control older Walkera models.

CH1|CH2|CH3|CH4
---|---|---|---
A|E|T|R

### Sub_protocol W6_5_1 - *2*
WK2601 5+1: AIL, ELE, THR, RUD, GYRO (ch 7) are proportional. Gear (ch 5) is binary. Ch 6 is disabled

CH1|CH2|CH3|CH4|CH5|CH6|CH7
---|---|---|---|---|---|---
A|E|T|R|GEAR|DIS|GYRO

### Sub_protocol W6_6_1 - *3*
WK2601 6+1: AIL, ELE, THR, RUD, COL (ch 6), GYRO (ch 7) are proportional. Gear (ch 5) is binary. **This mode is highly experimental.**

CH1|CH2|CH3|CH4|CH5|CH6|CH7
---|---|---|---|---|---|---
A|E|T|R|GEAR|COL|GYRO

### Sub_protocol W6_HEL - *4* and W6HEL_I - *5*
WK2601 Heli: AIL, ELE, THR, RUD, GYRO are proportional. Gear (ch 5) is binary. COL (ch 6) is linked to Thr. If Ch6 >= 0, the receiver will apply a 3D curve to the Thr. If Ch6 < 0, the receiver will apply normal curves to the Thr. The value of Ch6 defines the ratio of COL to THR.

W6HEL_I: Invert COL servo

option= maximum range of COL servo

CH1|CH2|CH3|CH4|CH5|CH6|CH7
---|---|---|---|---|---|---
A|E|T|R|GEAR|COL|GYRO

## DSM - *6*
Extended limits supported

Telemetry enabled for TSSI and plugins

option=number of channels from 4 to 12. An invalid option value will end up with 6 channels.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12
---|---|---|---|---|---|---|---|---|----|----|----
A|E|T|R|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12

Notes:
 - model/type/number of channels indicated on the RX can be different from what the RX is in fact wanting to see. So don't hesitate to test different combinations until you have something working. Using Auto is the best way to find these settings.
 - RX output will match the Spektrum standard TAER independently of the input configuration AETR, RETA...
 - RX output will match the Spektrum standard throw (1500µs +/- 400µs -> 1100..1900µs) for a 100% input. This is true for both Serial and PPM input. For PPM, make sure the end points PPM_MIN_100 and PPM_MAX_100 in _config.h are matching your TX ouput. The maximum ouput is 1000..2000µs based on an input of 125%.
    - If you want to override the above and get maximum throw (old way) uncomment in _config.h the line #define DSM_FULL_THROW . In this mode to achieve standard throw use a channel weight of 84%.

### Sub_protocol DSM2_22 - *0*
DSM2, Resolution 1024, refresh rate 22ms
### Sub_protocol DSM2_11 - *1*
DSM2, Resolution 2048, refresh rate 11ms
### Sub_protocol DSMX_22 - *2*
DSMX, Resolution 2048, refresh rate 22ms
### Sub_protocol DSMX_11 - *3*
DSMX, Resolution 2048, refresh rate 11ms
### Sub_protocol AUTO - *4*
The "AUTO" feature enables the TX to automatically choose what are the best settings for your DSM RX and update your model protocol settings accordingly.

The current radio firmware which are able to use the "AUTO" feature are ersky9x (9XR Pro, 9Xtreme, Taranis, ...), er9x for M128(9XR)&M2561 and OpenTX (mostly Taranis).
For these firmwares, you must have a telemetry enabled TX and you have to make sure you set the Telemetry "Usr proto" to "DSMx".
Also on er9x you will need to be sure to match the polarity of the telemetry serial (normal or inverted by bitbashing), while on ersky9x you can set "Invert COM1" accordinlgy.

## J6Pro - *22*

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12
---|---|---|---|---|---|---|---|---|----|----|----
A|E|T|R|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12

## WFLY - *40*
Receivers: WFR04S, WFR07S, WFR09S

Extended limits supported

option=number of channels from 4 to 9. An invalid option value will end up sending 9 channels.

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9

***
# NRF24L01 RF Module

## ASSAN - *24*
Extended limits supported

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8

The transmitter must be close to the receiver while binding.

## BAYANG - *14*
Autobind protocol

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11
---|---|---|---|---|---|---|---|---|----|----
A|E|T|R|FLIP|RTH|PICTURE|VIDEO|HEADLESS|INVERTED|RATES

RATES: -100%(default)=>higher rates by enabling dynamic trims (except for Headless), 100%=>disable dynamic trims

### Sub_protocol BAYANG - *0*
Models: EAchine H8(C) mini, BayangToys X6/X7/X9, JJRC JJ850, Floureon H101 ...

Option=0 -> normal Bayang protocol

Option=1 -> enable telemetry with [Silverxxx firmware](https://github.com/silver13/H101-acro/tree/master). Value returned to the TX using FrSkyD Hub are RX RSSI, TX RSSI, A1=uncompensated battery voltage, A2=compensated battery voltage

### Sub_protocol H8S3D - *1*
Model: H8S 3D

Same channels assignement as above.

### Sub_protocol X16_AH - *2*
Model: X16 AH

CH12|
----|
TAKE_OFF|

### Sub_protocol IRDRONE - *3*
Model: IRDRONE

CH12|CH13
----|----
TAKE_OFF|EMG_STOP

## Cabell - *34*
Homegrown protocol with variable number of channels (4-16) and telemetry (RSSI, V1, V2).

It is a FHSS protocol developed by Dennis Cabell (KE8FZX) using the NRF24L01+ 2.4 GHz transceiver. 45 channels are used frequency hop from 2.403 through 2.447 GHz. The reason for using 45 channels is to keep operation within the overlap area between the 2.4 GHz ISM band (governed in the USA by FCC part 15) and the HAM portion of the band (governed in the USA by FCC part 97). This allows part 15 compliant use of the protocol, while allowing licensed amateur radio operators to operate under the less restrictive part 97 rules if desired.

Additional details about configuring and using the protocol are available at the RX project at: https://github.com/soligen2010/RC_RX_CABELL_V3_FHSS

CH1|CH2|CH3|CH4|CH5 |CH6 |CH7 |CH8 |CH9 |CH10|CH11|CH12|CH13|CH14 |CH15 |CH16
---|---|---|---|----|----|----|----|----|----|----|----|----|-----|-----|-----
 A | E | T | R |AUX1|AUX2|AUX3|AUX4|AUX5|AUX6|AUX7|AUX8|AUX9|AUX10|AUX11|AUX12

### Sub_protocol CABELL_V3 - *0*
4 to 16 channels without telemetry

### Sub_protocol CABELL_V3_TELEMETRY - *1*
4 to 16 channels with telemetry (RSSI, V1, V2). V1 & V2 can be used to return any analog voltage between 0 and 5 volts, so can be used for battery voltage or any other sensor that provides an analog voltage.

### Sub_protocol CABELL_SET_FAIL_SAFE - *6*
Stores failsafe values in the RX.  The channel values are set when the sub-protocol is changed to 6, so hold sticks in place as the sub-protocol is changed.

### Sub_protocol CABELL_UNBIND - *7*
The receiver bound to the model is un-bound.  This happens immediately when the sub-protocol is set to 7.

## CG023 - *13*
Autobind protocol

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP|LIGHT|PICTURE|VIDEO|HEADLESS

### Sub_protocol CG023 - *0*
Models: EAchine CG023/CG031/3D X4

### Sub_protocol YD829 - *1*
Models: Attop YD-822/YD-829/YD-829C ...

CH5|CH6|CH7|CH8|CH9
---|---|---|---|---
FLIP||PICTURE|VIDEO|HEADLESS

## CX10 - *12*
Autobind protocol

CH1|CH2|CH3|CH4|CH5|CH6
---|---|---|---|---|---
A|E|T|R|FLIP|RATE

Rate: -100%=rate 1, 0%=rate 2, +100%=rate 3

### Sub_protocol GREEN - *0*
Models: Cheerson CX-10 green pcb

Same channels assignement as above.

### Sub_protocol BLUE - *1*
Models: Cheerson CX-10 blue pcb & some newer red pcb, CX-10A, CX-10C, CX11, CX12, Floureon FX10, JJRC DHD D1

CH5|CH6|CH7|CH8
---|---|---|---
FLIP|RATE|PICTURE|VIDEO

Rate: -100%=rate 1, 0%=rate 2, +100%=rate 3 or headless for CX-10A

### Sub_protocol DM007 - *2*

CH5|CH6|CH7|CH8|CH9
---|---|---|---|---
FLIP|MODE|PICTURE|VIDEO|HEADLESS

### Sub_protocol JC3015_1 - *4*

CH5|CH6|CH7|CH8
---|---|---|---
FLIP|MODE|PICTURE|VIDEO

### Sub_protocol JC3015_2 - *5*

CH5|CH6|CH7|CH8
---|---|---|---
FLIP|MODE|LED|DFLIP

### Sub_protocol MK33041 - *6*

CH5|CH6|CH7|CH8|CH9|CH10
---|---|---|---|---|---
FLIP|MODE|PICTURE|VIDEO|HEADLESS|RTH

## DM002 - *33*
Autobind protocol

**Only 3 TX IDs available, change RX_Num value 0-1-2 to cycle through them**

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11
---|---|---|---|---|---|---|---|---|----|----
A|E|T|R|FLIP|LED|CAMERA1|CAMERA2|HEADLESS|RTH|RATE_LOW

## ESKY - *16*

CH1|CH2|CH3|CH4|CH5|CH6
---|---|---|---|---|---
A|E|T|R|GYRO|PITCH

## ESKY150 - *35*
ESky protocol for small models since 2014 (150, 300, 150X, ...)

Number of channels are set with option. option=0 4 channels and option=1 7 channels. An invalid option value will end up with 4 channels.

CH1|CH2|CH3|CH4|CH5|CH6|CH7
---|---|---|---|---|---|---
A|E|T|R|FMODE|AUX6|AUX7

FMODE and AUX7 have 4 positions: -100%..-50%=>0, -50%..5%=>1, 5%..50%=>2, 50%..100%=>3

## FY326 - *20*

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP|RTH|HEADLESS|EXPERT|CALIBRATE

## FQ777 - *23*
Model: FQ777-124 (with SV7241A)

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
A|E|T|R|FLIP|RTH|HEADLESS|EXPERT

## GW008 - *32*
Model: Global Drone GW008 from Banggood

There are 3 versions of this small quad, this protocol is for the one with a XNS104 IC in the stock Tx and PAN159CY IC in the quad. The xn297 version is compatible with the CX10 protocol (green pcb). The LT8910 version is not supported yet.

CH1|CH2|CH3|CH4|CH5
---|---|---|---|---
A|E|T|R|FLIP

## H8_3D - *36*
Autobind protocol

### Sub_protocol H8_3D - *0*
Models: EAchine H8 mini 3D, JJRC H20/H22/H11D

CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13
---|---|---|---|---|---|---|---|---
FLIP|LIGTH|PICTURE|VIDEO|OPT1|OPT2|CAL1|CAL2|GIMBAL

JJRC H20: OPT1=Headless, OPT2=RTH

JJRC H22: OPT1=RTH, OPT2=180/360° flip mode

H8 3D: OPT1=RTH then press a direction to enter headless mode (like stock TX), OPT2=switch 180/360° flip mode

CAL1: H8 3D acc calib, H20/H20H headless calib
CAL2: H11D/H20/H20H acc calib

### Sub_protocol H20H - *1*
CH6=Motors on/off

### Sub_protocol H20 Mini - *2*
**Only 3 TX IDs available, change RX_Num value 0-1-2 to cycle through them**

### Sub_protocol H30 Mini - *3*
**Only 4 TX IDs available, change RX_Num value 0-1-2_3 to cycle through them**

## HISKY - *4*
### Sub_protocol Hisky - *0*
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
A|E|T|R|GEAR|PITCH|GYRO|CH8

GYRO: -100%=6G, +100%=3G

### Sub_protocol HK310 - *1*
Models: RX HK-3000, HK3100 and XY3000 (TX are HK-300, HK-310 and TL-3C)

Failsafe supported

CH1|CH2|CH3|CH4|CH5
---|---|---|---|---
| | |T|R|AUX

## KN - *9*
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11
---|---|---|---|---|---|---|---|---|----|----
A|E|T|R|DR|THOLD|IDLEUP|GYRO|Ttrim|Atrim|Etrim

Dual Rate: +100%=full range, Throttle Hold: +100%=hold, Idle Up: +100%=3D, GYRO: -100%=6G, +100%=3G

### Sub_protocol WLTOYS - *0*
### Sub_protocol FEILUN - *1*
Same channels assignement as above.

## HONTAI - *26*
Autobind protocol

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11
---|---|---|---|---|---|---|---|---|----|----
A|E|T|R|FLIP|LED|PICTURE|VIDEO|HEADLESS|RTH|CAL

### Sub_protocol HONTAI - *0*
### Sub_protocol JJRCX1 - *1*
CH6|
---|
ARM|

### Sub_protocol X5C1 clone - *2*

### Sub_protocol FQ777_951 - *3*

## MJXQ - *18*
Autobind protocol

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13|CH14
---|---|---|---|---|---|---|---|---|----|----|----|----|----
A|E|T|R|FLIP|LED|PICTURE|VIDEO|HEADLESS|RTH|AUTOFLIP|PAN|TILT|RATE

RATE: -100%(default)=>higher rates by enabling dynamic trims (except for Headless), 100%=>disable dynamic trims

### Sub_protocol WLH08 - *0*
### Sub_protocol X600 - *1*
Only 3 TX IDs available, change RX_Num value 0..2 to cycle through them
### Sub_protocol X800 - *2*
Only 3 TX IDs available, change RX_Num value 0..2 to cycle through them
### Sub_protocol H26D - *3*
Only 3 TX IDs available, change RX_Num value 0..2 to cycle through them
### Sub_protocol E010 - *4*
15 TX IDs available, change RX_Num value 0..14 to cycle through them

If the E010 does not respond well to inputs or hard to bind, set Power to Low.
### Sub_protocol H26WH - *5*
CH6|
---|
ARM|

Only 1 TX ID available

## MT99XX - *17*
Autobind protocol

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP|LED|PICTURE|VIDEO|HEADLESS

### Sub_protocol MT99 - *0*
Models: MT99xx
### Sub_protocol H7 - *1*
Models: Eachine H7, Cheerson CX023
### Sub_protocol YZ - *2*
Model: Yi Zhan i6S
Only one model can be flown at the same time since the ID is hardcoded.
### Sub_protocol LS - *3*
Models: LS114, 124, 215

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP|INVERT|PICTURE|VIDEO|HEADLESS

### Sub_protocol FY805 - *4*
Model: FY805

Only 1 ID available

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP||||HEADLESS

## Q2X2 - *29*
### Sub_protocol Q222 - *0*
Models: Q222 v1 and V686 v2

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12
---|---|---|---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP|LED|MODULE2|MODULE1|HEADLESS|RTH|XCAL|YCAL

### Sub_protocol Q242 - *1* and Q282 - *2*

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12
---|---|---|---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP|LED|PICTURE|VIDEO|HEADLESS|RTH|XCAL|YCAL

Model: JXD 509 is using Q282 with CH12=Start/Stop motors

## Q303 - *31*
Autobind protocol

CH1|CH2|CH3|CH4
---|---|---|---
A|E|T|R

### Sub_protocol Q303 - *0*
Q303 warning: this sub_protocol is known to not work at all/properly with 4in1 RF modules.

CH5|CH6|CH7|CH8|CH9|CH10|CH11
---|---|---|---|---|---|---
AHOLD|FLIP|PICTURE|VIDEO|HEADLESS|RTH|GIMBAL

GIMBAL needs 3 position -100%/0%/100%

### Sub_protocol CX35 - *1*
CH5|CH6|CH7|CH8|CH9|CH10|CH11
---|---|---|---|---|---|---
ARM|VTX|PICTURE|VIDEO||RTH|GIMBAL

ARM is 2 positions: land / take off

Each toggle of VTX will increment the channel.

Gimbal is full range.

### Sub_protocol CX10D  - *2* and Sub_protocol CX10WD - *3*
CH5|CH6
---|---
ARM|FLIP

ARM is 3 positions: -100%=land / 0%=manual / +100%=take off

## Shenqi - *19*
Autobind protocol

Model: Shenqiwei 1/20 Mini Motorcycle

CH1|CH2|CH3|CH4
---|---|---|---
 | |T|R

Throttle +100%=full forward,0%=stop,-100%=full backward.

## SLT - *11*
Autobind protocol

### Sub_protocol V1 - *0*

CH1|CH2|CH3|CH4|CH5|CH6
---|---|---|---|---|---
A|E|T|R|GEAR|PITCH

### Sub_protocol V2 - *1*

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8
---|---|---|---|---|---|---|---
A|E|T|R|CH5|CH6|CH7|CH8

### Sub_protocol Q100 - *2*
Models: Dromida Ominus UAV

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13
---|---|---|---|---|---|---|---|---|---|---|---|---
A|E|T|R|RATES|-|CH7|CH8|MODE|FLIP|-|-|CALIB

RATES takes any value between -50..+50%: -50%=min rates, 0%=mid rates (stock setting), +50%=max rates

CH7 and CH8 have no visible effect

MODE: -100% level, +100% acro

FLIP: sets model into flip mode for approx 5 seconds at each throw of switch (rear red LED goes out while active) -100%..+100% or +100%..-100%

CALIB: -100% normal mode, +100% gyro calibration

### Sub_protocol Q200 - *3*
Model: Dromida Ominus Quadcopter FPV, the Nine Eagles - FENG FPV and may be others

Dromida Ominus FPV channels mapping:

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13
---|---|---|---|---|---|---|---|---|---|---|---|---
A|E|T|R|RATES|-|CH7|CH8|MODE|FLIP|VID_ON|VID_OFF|CALIB

FENG FPV: channels mapping:

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12|CH13
---|---|---|---|---|---|---|---|---|---|---|---|---
A|E|T|R|RATES|-|CH7|CH8|FLIP|MODE|VID_ON|VID_OFF|CALIB

RATES takes any value between -50..+50%: -50%=min rates, 0%=mid rates (stock setting), +50%=max rates

CH7 and CH8 have no visible effect

MODE: -100% level, +100% acro

FLIP: sets model into flip mode for approx 5 seconds at each throw of switch (rear red LED goes out while active) -100%..+100% or +100%..-100%

CALIB: -100% normal mode, +100% gyro calibration

### Sub_protocol MR100 - *4*
Models: Vista UAV, FPV, FPV v2

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11|CH12
---|---|---|---|---|---|---|---|---|---|---|---
A|E|T|R|RATES|-|CH7|CH8|MODE|FLIP|VIDEO|PICTURE

RATES takes any value between -50..+50%: -50%=min rates, 0%=mid rates (stock setting), +50%=max rates

CH7 and CH8 have no visible effect

FLIP: sets model into flip mode for approx 5 seconds at each throw of switch (rear red LED goes out while active) -100%..+100% or +100%..-100%

MODE: -100% level, +100% acro

## Symax - *10*
Autobind protocol

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP|RATES|PICTURE|VIDEO|HEADLESS

RATES: -100%(default)=>disable dynamic trims, +100%=> higher rates by enabling dynamic trims (except for Headless)

### Sub_protocol SYMAX - *0*
Models: Syma X5C-1/X11/X11C/X12

### Sub_protocol SYMAX5C - *1*
Model: Syma X5C (original) and X2

## V2X2 - *5*
CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9|CH10|CH11
---|---|---|---|---|---|---|---|---|----|----
A|E|T|R|FLIP|LIGHT|PICTURE|VIDEO|HEADLESS|MAG_CAL_X|MAG_CAL_Y

### Sub_protocol V2x2 - *0*
Models: WLToys V202/252/272, JXD 385/388, JJRC H6C, Yizhan Tarantula X6 ...

PICTURE: also automatic Missile Launcher and Hoist in one direction

VIDEO: also Sprayer, Bubbler, Missile Launcher(1), and Hoist in the other dir

### Sub_protocol JXD506 - *1*
Model: JXD 506

CH10|CH11|CH12
---|---|---
Start/Stop|EMERGENCY|CAMERA_UP/DN

## YD717 - *8*
Autobind protocol

CH1|CH2|CH3|CH4|CH5|CH6|CH7|CH8|CH9
---|---|---|---|---|---|---|---|---
A|E|T|R|FLIP|LIGHT|PICTURE|VIDEO|HEADLESS

### Sub_protocol YD717 - *0*
### Sub_protocol SKYWLKR - *1*
### Sub_protocol SYMAX4 - *2*
### Sub_protocol XINXUN - *3*
### Sub_protocol NIHUI - *4*
Same channels assignement as above.

# OpenLRS module

## OpenLRS - *27*
This is a reservation for OpenLRSng which is using Multi's serial protocol for their modules: https://openlrsng.org/. On the Multi side there is no protocol affected on 27 so it's just ignored.
