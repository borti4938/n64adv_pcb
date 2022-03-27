N64 Advanced (PCB)
---

This repository contains all you need files to build your own DIY N64 Advanced board.
Firmware is supplied in another repository.

Please don't ask me for selling a modding.
I either sell some prototypes on some forums marketplaces (which is very unlikely) or I don't have any of the boards.
This is a complete DIY modding project.
So everybody is on his own here.

**WARNING:**
This is an advanced DIY project if you do everything on your own. You need decent soldering skills.
The FPGA has 0.5mm fine pitch with 144pins and one exposed pad below the IC, which has to be soldered to the PCB.
Next to it the video DAC has also 0.5mm pin pitch on the board there are some SMD1206 resistor and ferrit bead arrays.
However, there are some awesome shops out there selling the boards for a great price.

## Table of Contents

- [Checklist](https://github.com/borti4938/n64adv_pcb#checklist-how-to-build-the-project)
- [Assembly](https://github.com/borti4938/n64adv_pcb#assembly)
- [Installation](https://github.com/borti4938/n64adv_pcb#installation)
  - [1. Open the Console](https://github.com/borti4938/n64adv_pcb#1-open-the-console)
  - [2. Optional Steps](https://github.com/borti4938/n64adv_pcb#2-optional-steps)
  - [3. Solder Work](https://github.com/borti4938/n64adv_pcb#3-solder-work)
  - [4. Mount the N64 Advanced Modding PCB](https://github.com/borti4938/n64adv_pcb#4-mount-the-n64-advanced-modding-pcb)
  - [5. Finish the Work](https://github.com/borti4938/n64adv_pcb#5-finish-the-work)
- [Jumper Setup](https://github.com/borti4938/n64adv_pcb#jumper-setup)
- [Cable Setup](https://github.com/borti4938/n64adv_pcb#cable-setup)

## Checklist: How to build the project

- Use PCB files (either [EAGLE-PCB design file](./n64adv.brd) or [Gerber files](./Gerber/)) to order your own PCB or simply use the [shared project(s) on OSHPark](https://oshpark.com/profiles/borti4938)
- If you paln to use solder paste, do not forget to order a stencil for top and bottom, too
- Source the components you need, e.g. from Mouser or Digikey.  
  The BOM is available in [here](./doc/n64adv_BOM.xlsx).
- If you want to use a assembly service (PCBA), you can use the mounting files ([top](./doc/n64adv.mnt) and [bottom](./doc/n64adv.mnb)) , too.
- Wait for everything to arrive
- Assemble your PCB if you haven't use a PCBA service
- Set all jumpers
- Flash the firmware to the serial flash device either in advanced (e.g. using the MiniPro) or after installation (e.g. using an Altera USB Blaster)
  * In advanced:
    * use the n64adv\__fpga-device_\_spi.bin binary, which can be found in the firmware repository
    * If your programmer does not support the serial flash  device on the N64Adv board (currently IS25LP016D), you may setup the flash program for another compatible device (e.g. A25L016 @SOP8). You may have to uncheck _Check ID_ options (or similar)
  * After installing:
    * Use the JIC programming file named n64adv\__fpga-device_.jic
    * Using the IntelFPGA programmer, JTAG chain is initialized by loading JIC file
    * N64 needs to be powered for flashing
    * Power cycle the N64 after flashing
- If you want to go for a clean installation with custom made flexible PCBs, just visit the repository for add ons ([Flex PCB for the digital video interface](https://github.com/borti4938/n64rgb_project_misc/RCP2N64RGB), [Flex PCB for the analog video output](https://github.com/borti4938/n64rgb_project_misc/N64RGB2MoutFilter)

## Assembly

If you have all components available, you can start assembly your board.
The documentation provides assembly sheets ([top](./doc/n64adv_assembly_sheet_top.pdf) and [bottom](./doc/n64adv_assembly_sheet_bot.pdf), which you can print out.
Together with the [BOM](./doc/n64adv_BOM.xlsx) it is just a matter of time and effort to assembly everything.

Note that the FPGA has an exposed pad, which needs a cood connection.
Otherwise the FPGA will not boot at all.
Preheat the FPGA with a hot air gun and solder the exposed pad from bottom if you do not use solder paste.
If you only have your solder iron, apply heat from bottom.
You have to be patient at this point as there are large GND planes around the FPGA.
You may check the FPGA temperature with your finger on the top side (once it gets too hot to touch, you may stop for soldering for a brief moment).

If you populate J5, which is the JTAG connector, please short the pins such that they are flush at the bottom side.
This reduces the risk to short on of these pins with the heat sink (where the PCB will be mounted).

Using non-clean flux (rosin based) is obviously recommended.
Eventhough it is "non-clean" I recommend cleaning everything afterwards (just for the visual finish).

Please double check everything for shorts once you finished your work.
Very important is that the power supply trace do not short to GND.
- 3.3V against GND (e.g. at C1 and at C114)
- 2.5V against GND (e.g. at C702)
- 1.2V against GND (e.g. at C121)
- 5V against GND (e.g. at C2)

## Installation

### 1. Open the console

- Remove Jumper Pak / Expansion Pak
- Remove screws from bottom side of the console  
(needs a 4.5mm gamebit tool)
- Lift top housing
- remove inner screws as marked  
(in very last consoles made the heat sink design changed slightly)
- pull out the mainboard
- remove heat sink and RF shield
- **Hint** Now you have the chance to clean up your N64 shell

![](./doc/img/Screws_bot.jpg)

![](./doc/img/Screws_inside.jpg)
(image by Zerberus (circuit-board.de user))


### 2. Optional Steps

#### 2.1 Preparation of Top RF Shield

This step is optional.
It helps a) to reduce mechanical stress on the flex cable (if used) and b) ensures maximal distance between digital signals (installation wires or flex cable) and analog signals (MultiOut).

- Locate the tap in the RF shield next to the cartridge slot at the side of the MultiOut.  
(this is the place where the installation wires / flex cable will be routed through)
- option 1: bend the tap away

![](./doc/img/Bend_RFShield_Back.jpg)

- option 2: simply cut the tab incl. the small piece up to the step in the RF shield

![](./doc/img/Cut_RFShield_Back.jpg)

##### Some explanation on this optional step

Later, the data lines as well as the clock will be routed close to the MultiOut.
The distance between both is a crucial issue as your wires act as interferer.
Hence, having the tap bended might be ok, but it is better to cut some piece of the RF shield away to further increase the distance between MultiAV and data lines

![](./doc/img/gap_digital_lines_multiout.jpg)

#### GND insulation on the MultiOut

In order to have a better return path for the digital video lines defined, you may insulate GND from the MultiOut.
This step is only recommended if you have a noisy picture.
It comes with the drawback that a) you have a lot of more effort during installation and b) having a longer return path for the analog audio output as well as for S-Video and Composte Video as GND is later routed over the modding board.

- desolder the MultiOut connector
- drill out the GND pads
- insulare the drilles (e.g. with varnish (flex cable or wire install) or small shrink tubes (only wire installation))
- solder MoultiOut back into place

![](./doc/img/MultiOut_GND_insulation.jpg)


### 3. Solder Work

You have the options to either install everything with casual installation wires or using flex cables.
This is just a trade-off between personal installation effort and price.
Personally I recommend using the flex cable.

#### 3.1 Using the Flex Cable 

To use this option, you have to have the flex cables as shown at hand.
Note that the picture shows older prototype version.

![](./doc/img/flex_cables.jpg)

##### 3.1.1 Digital inputs

Start with the digital side:
- Solder the RCP connector side to the RCP-NUS as shown
  - first pin bottom – 8
  - last pin to – 28
- Connect 3.3V to the flex, e.g. taken from C141
- Connect Ctrl. and reset
  - reset from PIF-NUS pin 27
  - Controller from PIF-NUS pin 16  
  (Make sure that PIF-NUS pin 16 is connected to the middle pin of controller port 1, otherwise search for a suiteable connection point)
- bend the flex as marked in a way such that you do not see the shaded area anymore.  
The flex will be routed over the MultiOut.

**Note**: when using the flex of version earlier than _v2022xxyy_ you need to cut the RF shield near the MultiOut as shown in 2.1

![](./doc/img/RCPNUSwFlex.jpg)

##### 3.1.2 Analog outputs

Next up is the analog video flex which will be connected to the MultiOut.
Before you start here, make yourself clear where you want to route sync to.
This depends on whether you want to use sync on luma or sync on composite sync (raw sync) cable.

- Free the sync pin from the MultiOut, which means that you have to trace back the copper track from the MultiOut pin and make sure that there is nothing connected to.
  - pin 3 (raw sync cable): usually unconnected. Only connected in earlier NTSC console (-CPU-01 to -CPU-03)
  - pin 7 (luma sync cable): usually connected. Search for a nearby resistor or trace back to the DENC-NUS or MAV-NUS and lift the luma pin. Remove the resistor or lift the pin at the DENC-NUS or MAV-NUS.
- solder the flex on its place of the MultiOut
- close **SJ1** on the flex from
  - middle to 3 if using raw sync
  - middle to 7 if using luma sync
- Note that it is also possible to connect sync pin 9 if you want to use a sync on composite video cable.
  - disconnect composite vide from pin 9 using the back tracing technique
  - solder a small wire from **SJ1** middle pad to pin 9 of the MultiOut

![](./doc/img/MultiAV_flex.jpg)

#### 3.2 Using Wires

First you need to work out the digital video signals as well as reset and controller 1. 
Most of the signals needed has to be taken from video processor output; the RCP-NUS.
Next to the RCP-NUS is the video encoder, where several types are used during the variety of N64 designs.
The pinouts are given below. Basically we need D0-D6, /DSYNC and /CLK (or VCLK or CLOCK) for the modding board.

![](./doc/img/rcp_pinout.jpg)
(RCP-NUS picture by Marshall; Encoder picture by Viletim)

Next to the video signals, you will need to get power (3.3V and GND), reset and controller 1.
This means you need in total 13 wires.
If you use a ribbon cable, I recommend you to use either a 15 conductor cable (double 3.3V and GND) or a 11 conductor cable and separate 3.3V and GND wires. 

- Locate the video encoder
  - Solder a bunch of wires to the video signal pins D0-D6, /DSYNC and /CLK
- Solder wires for reset and controller 1
  - reset from PIF-NUS pin 27
  - Controller from PIF-NUS pin 16  
  (Make sure that PIF-NUS pin 16 is connected to the middle pin of controller port 1, otherwise search for a suiteable connection point)
- Solder two addional wires for 3.3V (can be picked off C141) and one for GND (large GND plane)
- Route all wires to the MultiOut port

![](./doc/img/rcp_wires.jpg)

![](./doc/img/pif_wires.jpg)

Next you can prepare wires for the analog video.
Connect several wires to the MultiOut.
Before you start here, make yourself clear where you want to route sync to.
This depends on whether you want to use a sync on composte video, a sync on luma or a sync on composite sync (raw sync) cable.

- Free the sync pin from the MultiOut, which means that you have to trace back the copper track from the MultiOut pin and make sure that there is nothing connected to.
  - pin 3 (raw sync cable): usually unconnected. Only connected in earlier NTSC console (-CPU-01 to -CPU-03)
  - pin 7 (luma sync cable): usually connected. Search for a nearby resistor or trace back to the DENC-NUS or MAV-NUS and lift the luma pin. Remove the resistor or lift the pin at the DENC-NUS or MAV-NUS.
  - pin 9 (compostie video sync cable): usually connected. Search for a nearby resistor or trace back to the DENC-NUS or MAV-NUS and lift the luma pin. Remove the resistor or lift the pin at the DENC-NUS or MAV-NUS.
- Prepare some wires to the following pins
  - pin 1: red
  - pin 2: green
  - pin 4: blue
  - pin 3, 7 or 9: sync
  - pin 5 and/or 6: GND (connect pin 5 **and** pin 6 if you insulated GND from the MultiOut)
  - pin 10: 5V

![](./doc/img/MultiAV_wires.jpg)

You have the option to use the FilterAddOn board here.
Note that the filter is optionally included in on the flex cable setup.

If you use the filter board, you clearly have to solder the filter board first and connect the wire to the pads of the filter board as marked on the silk screen.

  
### 4. Mount the N64 Advanced Modding PCB

The PCB is designed to fit on the heat sink of the N64.
There is a large via on the PCB where a M3 screw passes through.

If you have a 3D printer at hand, [Consoles4You](https://twitter.com/Consoles4You/status/1402648032848531457) created a beautiful 3D bracket to make mounting extremely easy.
File can be downloaded [here](https://t.co/0TzSATg8nK).
A mirrow of the files supplied in [this repository](./3d_mount_bracket) courtesy to Consoles4You.
Main advantage is that you do not need to make sure that the JTAG connector does not short out at the heat sink.
Most selled kits comes with uncutted pins here, so there is a high risk.
Another advantage is that you can you a mainboard screw to secure the modding board.

If you do not have a 3D printer you can also mount the PCB using a M3 screw of length 10mm or 12mm, a set of washer and a nut.
Be careful with the JTAG connector **J5** and ensure that the  pins do not short to the heatsink!

![](./doc/img/Mount_PCB2HeatShield.jpg)

If you want to use the heat sink shield as an additional secure GND connection you can also apply some solder to the mounting via.

![](./doc/img/Solder_PCBmounting_hole.jpg)

If everything is mounted and secured, the PCB should sit like that.

![](./doc/img/PCB_at_HeatShield.jpg)


### 5. Finish the Work

Before continue you can assemble the RF shield and heat sink back to the N64 mainboard.
In that way everything is secured in place where it belongs to.
Of course, if you use an installation with wires, make sure that you know which wire is for what.
On my ealrier installs I used some wire marker.

![](./doc/img/wire_markings.jpg)

#### 5.1 With Flex Cable

Bend the flex cables gently at the shaded areas.
In that way you should be able to alling both flex cables at the N64 Advanced modding board.
Solder the flex cables at their places.
(Please note subsection 5.3 - CSYNC).

![](./doc/img/N64A_flex.jpg)

Connect an extra GND wire as shown in 5.3.

#### 5.2 With Wires

Just connect every wire at its place.
The silk screen shows you where everything goes to.
(Please note subsection 5.3 - CSYNC).
Note that the picture shows an older version of the N64 Advanced.
Newer versions have reset and controller at the left hand side.

![](./doc/img/N64A_wires.jpg)

Connect an extra GND wire as shown in 5.4.

#### 5.3 CSYNC

You have the option to connect _/CS_ or _/CS (75ohm)_.
What to prefere / to use depends on the modding board version you have.

##### N64A_20200305 and later (with J33)

Here, I recommend using _/CS (75ohm)_ except if you have a true TTL sink, which is usually not the case!
In virtually 99% of all setups connecting the sync wire to _/CS (75ohm)_ is the way to go.

On installations with the flex cables follows that you have to close SJ3 bottom on the flex.
Please note the Jumper Setup section to correctly setup the sync level.

##### N64A_20191005 and earlier (without J33)

On these modding boards the connection depends on your wire setup.
If you have a resistor in the sync wire inside of your cable, you have to use the TTL _/CS_ output.
This also holds if you have a true TTL setup.
If you have no components in your cable to further attenuate the sync level, you have to connect _/CS (75ohm)_.
Otherwise you may harm your setup.

On installations with the flex cables follows that you have to close either SJ3 top (_/CS_) or bottom (_/CS (75ohm)_).
Never close both!

#### 5.4 Extra GND Wire

Connect an extra GND wire to the heat sink screw.
This improves return path properies of the digital video lines.

![](./doc/img/Extra_GND.jpg)


## Jumper Setup

**IMPORTANT NOTE**  
Most jumpers are used as fallback if the software is unable to load a valid configuration from flash device U5.
They were introduced on the modding board before a menu was implemented in the firmware.  
The only exception is J1.2 - this jumper is still used to show the software whether the _Filter AddOn_ is installed or not.
Note that the _Filter AddOn_ might be part of the flex cable.

There are some jumpers spreaded over the PCB, namely _J1_, _J2_, _J3_, _J4_, _J5_ and _J6_.
_J1 - J4_ have to parts, let's say _.1_ and _.2_, where _.1_ is marked with the _dot_ on the PCB.

#### J1 (Filter AddOn)
##### J1.1 
- opened: output HSYNC and VSYNC for possible VGA output
- closed: use filter addon

##### J1.2
- opened: use filter of the addon board
- closed: bypass filter (actually filter is set to 95MHz cut-off which is way above the video signal content)

#### J2 (RGB, RGsB, YPbPr)
##### J2.1
- opened: RGB output
- closed: RGsB output (sync on green)

##### J2.2
- opened: RGB / RGsB output
- closed: YPbPr output (beats J2.1)


#### J3 (Scanline)
##### J3.1/J3.2
- opened / opened: 0%
- opened / closed: 25%
- closed / opened: 50%
- closed / closed: 100%

#### J33 (CSYNC level @ _/CS (75ohm)_ pad)

- opened: appr. 1.87V @ 75ohm termination i.e. needs a resistor inside the sync wire further attenuating the signal. Designed to work for cables with 470 ohm resistor inside resulting in appr. 450mV @ 75ohm termination
- closed: appr. 300mV @ 75ohm termination suitable for pass through wired cables at sync, works with standard TV / scaler setup

#### J4 (Linemode)
##### J4.1
- opened: linedoubling of 240p/288p to 480p/576p
- closed: no-linedoubling (beats J4.2)

##### J4.2
- opened: no bob de-interlace of 480i/576i
- closed: bob de-interlace of 480i/576i to 480p/576p (J4.1 must be opened)

#### J5
_J5_ is the JTAG connector.

#### J6 (_Outdated_; Power supply of analog outputs)
The analog part can be power with **either** 3.3V **or** 5V. If you want to power this part of the PCB with 3.3V, close _J6_ and leave pad _5V_ unconnected. If you want to power this part with 5V, leave _J6_ opened and connect pad _5V_ to +5V power rail of the N64. **NEVER connect 5V and close J6.**

## Cable Setup

| Signal | Pin MultiOut | Pin SCART | Ref. GND in SCART | Cinch Plug | Note |
|:-------|:-------------|:----------|:------------------|:-----------|:-----|
| Red / Pr | 1 | 15 | 13 | Red plug | Using a 220uF cap in series is possible |
| Green / Y | 2 | 11 | 9 | Green plug | Using a 220uF cap in series is possible |
| Blue / Pb | 4 | 7 | 5 | Blue plug | Using a 220uF cap in series is possible |
| Sync | 3, 7 or 9 | 20 | 17 | _not needed_ | Pin: See installation, Cable: see notes below |
| GND | 5, 6 | 4,5,9,13,17,18,21 | | Outer ring of each plug | Pin 21 @ SCART: outer shield |
| +5V | 10 | 16 | 18 | _not needed_ | SCART: use a 180ohm resistor in series |
| Audio left | 11 | 6 | 4 | Red plug | |
| Audio right | 12 | 2 | 4 | White plug | |

#### Notes on Sync:

- I recommend using the 75ohm compatible csync output.
  - Run a straight wire through your cable for sync if you have an earlier model (boards without J33).
  - If you have a modding board with J33, you can select with J33 whether you have a series resistor in you cable (J33 open) or if you have a straightly wired sync connection (J33 closed).
- If you use TTL sync output, you have to add an additional resistor in series in every case if you want to use the cable on a non-TTL setup (270ohm to 1.1kohm) to attenuate the signal for 75ohm terminationed sinks.

#### RGB cables:

- If you buy an RGB cable, buy a typical RGB cable for a NTSC SNES with sync on luma (MultiAV pin 7) or sync on csync (MultiAV pin 3).
- If you bought a raw csync cable and if you use the 75ohm output, ...
  - N64A_20191005 and earlier (without J33): remove / short the resistor which is possibly installed in the sync trace.
  - N64A_20200305 and later (with J33): leave J33 open if you have a sync connection with a resistor inside your sync line, otherwise close J33.
  
#### PAL RGB cables (SNES PAL):

- PAL RGB cable, i.e. a cable with additional 75ohm resistors to GND, are not supported out of the box.
- You can support them if and only if you have the _Filter AddOn_.  
  To support the cable, replace RN2 with a 39ohm resistor array or solder a second 75ohm resistor array on top of the existing one.

#### RGsB / YPbPr Cables:

With the given table you should be able to build your own component cable or to build an adapter (see pictures below)
Unfortunately, there are no cables / adapter to buy.
Only chance is to buy a Wii component cable and use a MultiOut adapter (SNES/N64/GC style to Wii style).

