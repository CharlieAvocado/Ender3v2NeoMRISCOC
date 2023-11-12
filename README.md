It's all FUBAR. This code uploaded for troubleshooting purposes ONLY. Use at your own demise.

**Need Help Troubleshooting Z-Axis Homing Issue on Ender-3 V2 Neo.**

// 2023-11-11 Update: End-stops Diagnotic shows x_min and y_min as open. z_min and filament are TRIGGERED. If I go to 1106: #define USE_ZMIN_PLUG and comment it out, then the z_min disappears completely from the diganostic, and filament still shows as TRIGGERED. As I have no filament sensor, I suspect this needs to come out. Perhaps then everything will work? Not sure how to remove it successfully though. 


**Introduction:**
Hello everyone, I'm experiencing issues with the Z-axis movement on my Ender-3 V2 Neo. I’m unsure if it’s a hardware issue, an electrical/wiring issue, a G-code/firmware issue, or an OP(me) issue. I have made recent modifications, and stupidly I did them at the same time. 

**This is going to be long, I’m sorry. I’m also desperate and wanted to provide as much information as possible in an attempt to find help!**

I’ve **installed Marlin firmware**, using [MRiscoC 20230312](https://github.com/mriscoc/Ender3V2S1/releases/tag/20230312) [aka v2.1.3], modified specifically for the 3 V2 Neo by using the [Lash-L files](https://github.com/Lash-L/Ender-3-V2-Neo-Setup/tree/main/Marlin_Configurations). I went with the 20230312 version because that’s the last one that Lash-L had provided configuration files for, and anything was better than the stock Ender firmware. I have **modified the stock parts** using an [official Creality Ender 3 Direct Drive Extruder Kit](https://www.amazon.com/Creality-Upgraded-Extruder-Flexible-Filament/dp/B08J7N2LNL/ref=sr_1_4?crid=2O5XX6D7ICJTD&keywords=ender+3+hotend+upgrade+kit&qid=1699636828&sprefix=ender+3+hotend+upgrade%2Caps%2C82&sr=8-4). I was tired when shopping and failed to realize the kit was only for Ender 3 Pro or Ender 3 V2; I’ve made it “work” with this [mounting bracket setup](https://www.amazon.com/dp/B09J2BRFMV). All expanded on below.

I'm looking for informed insights or advice to resolve these problems. I have done **extensive troubleshooting, all described below**. The **configuration files** as currently modified [can be viewed here](https://github.com/CharlieAvocado/Ender3v2NeoMRISCOC).

**Issue Description:**

- **Problem Summary:** The **Z-axis does not descend** the majority of the time, and when it does it’s only under one very specific circumstance.
- **Observations:**
    - **Any attempts to autohome or home-Z fails.** X and Y will home every time, but once it switches to Z, the axis goes up a bit, the CR-Touch probe extends properly and then just probes air. At no time will the Z-axis descend. The display shows **STOPPED**.
    - **The CR-Touch appears to be working properly.** No issues when the machine was stock hardware and firmware.
        - Before my current set of upgrades I did upgrade to an all-metal [Creality Ender 3 extruder kit with Capricorn Bowden tubing](https://www.microcenter.com/product/659620/creality-ender-3-extruder-kit-with-capricorn-bowden-tubing-for-ender-3-5-series-cr-10-series). I wouldn’t expect that to cause any issues with my Z-axis, and indeed it didn’t. Worked great.
    - **The Z-axis descends in one, and only one, scenario.** If, and only if, PRIOR to any homing commands, I navigate to Prepare > Move Axis (Live Move optional, has no effect on my error) > Move Z 0.0 > Z-Axis will ascend to maximum gantry limits AND descend within the limits of however far Z-Axis was moved upwards. [E.g., Z-Axis is moved up 40mm; Z-Axis will descend the entire 40mm when asked.] The Z-Axis will not descend below what it recognizes as 0.0mm. Again, if a homing attempt involves the Z-axis before Move Axis, Z will no longer descend under any circumstances excluding manually adjustment. This applies even if a successful Move Axis was performed in which the Z-axis descended. **As soon as homing the Z-axis gets involved, there are no known circumstances, aside from manual adjustment, that will descend the Z-axis.**

**Mechanical Setup:**

- **Printer Model:** Ender-3 V2 Neo. [[MicroCenter](https://www.microcenter.com/product/664536/creality-ender-3-v2-neo-3d-printer)] [[Creality Official](https://store.creality.com/products/ender-3-v2-neo-3d-printer)]
- **Z-Axis Configuration:** Stock. Didn’t change a thing mechanically. Have tested the Z-Axis wires for continuity, tested fine and seems properly wired.
- ****************Extruder:**************** As mentioned, I have **modified the stock parts** using an [official Creality Ender 3 Direct Drive Extruder Kit](https://www.amazon.com/Creality-Upgraded-Extruder-Flexible-Filament/dp/B08J7N2LNL/ref=sr_1_4?crid=2O5XX6D7ICJTD&keywords=ender+3+hotend+upgrade+kit&qid=1699636828&sprefix=ender+3+hotend+upgrade%2Caps%2C82&sr=8-4). **It’s possible this could be the issue.** I don’t know if it’s possible for that extruder to be outright incompatible with my machine in a way that the Z-Axis no longer descends, but I’d doubt it? I assume it would just need G-code dimension modifications to work well. More below.
- **Auto Bed Leveling Device:** I have the stock CR-Touch (Model: ALT04) that came with my machine. It appears to be working properly, but my confidence is limited since the Z-Axis doesn’t descend. Light sequence appears to be working properly until it is stopped by the firmware ending homing functions. I positioned it once to where it would be certain to contact the plate, and it seemed to be working except for the disconnect with the Z-Axis and homing ending.
- **Recent Changes:** As mentioned above, but expanded here:
    - I’ve **installed Marlin firmware**, using [MRiscoC 20230312](https://github.com/mriscoc/Ender3V2S1/releases/tag/20230312) [aka v2.1.3], modified specifically for the 3 V2 Neo by using the [Lash-L files](https://github.com/Lash-L/Ender-3-V2-Neo-Setup/tree/main/Marlin_Configurations). I went with the 20230312 version because that’s the last one that Lash-L had provided configuration files for, and anything was better than the stock Ender firmware. I’ve tried various configurations of Marlin and applied others config files for the Ender-3 V2 Neo, as well as personally modifying the code for my model. **Nothing has fixed the Z-Axis issue, INCLUDING attempting to return to stock firmware, which had previously worked but no longer does.** I feel like that may be key.
    - I **modified the stock parts** using an [official Creality Ender 3 Direct Drive Extruder Kit](https://www.amazon.com/Creality-Upgraded-Extruder-Flexible-Filament/dp/B08J7N2LNL/ref=sr_1_4?crid=2O5XX6D7ICJTD&keywords=ender+3+hotend+upgrade+kit&qid=1699636828&sprefix=ender+3+hotend+upgrade%2Caps%2C82&sr=8-4) and this [mounting bracket setup](https://www.amazon.com/dp/B09J2BRFMV). My plan is to adjust any probe offset measurements necessary via the G-code, but seems pointless to do that before I fix the Z-Axis issue. ********************************************************************************************************************************It is possible that in every way, this extruder is incompatible with my machine?******************************************************************************************************************************** I have not switched back to the stock setup to see if all that’s it, because I didn’t wanna do the work. At this point, I probably should have, and I will if I get no other usable solutions. But I’ll be mad.

**Electrical Setup:**

- ******************Mainboard:****************** I have a Creality 32 bit V4.2.2 board. The microSD slot has a sticker indicating T8. It is NOT a pin27 board.
- **************Display:************** TJC4827X243_011_P04. My understanding is that TJC displays are not 100% compatible with certain Marlin features and/or G-Code preview. I honestly don’t care, so I haven’t changed it beyond making sure it has the right updates. The microSD slot says either 8M or W8, if that’s relevant.
- **Wiring:**
    - **The CR-Touch is wired via a 5pin cable, on each end.** Meaning, it is not the split 3pin/2pin setup older ones use. [Yes, I accounted for this in the code, that’s under Configuration File, below.] The [mounting bracket setup](https://www.amazon.com/dp/B09J2BRFMV) shows what that wire looks like; best seen in 4th picture, inset showing wiring ends (ignore probe and board in picture, they are not the same). Fifth picture shows the 3pin/2pin split that I do NOT have.
    - **Have tested the probe wires for continuity,** tested fine and seems properly wired. I had read that some printers had issues with the ground and voltage wires being swapped in the wiring process. Continuity-wise, they are properly wired per multimeter testing. I took the replacement cable that came with the mounting bracket setup and swapped one end of the ground and voltage wires, in case it was an issue with probe wiring and motherboard wiring, and the machine wasn’t too happy with that, but no damage seems to have resulted and I definitively ruled out that potential issue. Please don’t @ me.
    - As far as I can tell, **I have no loose wires or poor contacts,** though I’m open to suggestions. I tested all Z-Axis and probe wires with a multimeter. As power works, the display works, the thermistor seems to work, all fans work, along with the X-Axis, Y-Axis, Extruder, and microSD, I didn’t test those with a multimeter. I did test the fuse, and it too works. I can find no damage anywhere, nor loose or overly tight connections.
    - Because this model comes with the CR-Touch, **Z-Axis homing is accomplished via USE_PROBE_FOR_Z_HOMING** instead of Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN. As I don’t have a Z-Probe, **nothing is plugged into the Z-Probe on the motherboard**, next to the X-Probe and Y-Probe. Instead, the firmware should use the CR-Touch probe, which is wired into the BL_T (assumedly for BL-Touch) probe on the motherboard, which shows G V IN G OUT. My actual wires are Blue, Red, Yellow, Black, White. More below under Configuration File, but I did try various code changes to no better success.
- **Power Supply:** Stock. The machine is properly switched to 115V, not 230V, and as I’m in the United States, that’s proper. I’ve never run it on 230V, as far as I’m aware I’m the only owner, so it’s unlikely it has been damaged in that manner. I’ve had two power outages while printing, thanks to storms and impatience, but each time the machine seemed fine and I successfully printed many times afterwards, prior to my most recent upgrades. It is plugged into a [Panamax Pm8-Ex 8 AC Outlet Surge Protector](https://www.amazon.com/Panamax-Pm8-Ex-Outlet-Surge-Protectors/dp/B0002YDZWO/ref=asc_df_B0002YDZWO/) which I can only assume did its job, as everything else that was plugged in and on is just fine.

**G-Code and Firmware:**

- **Configuration Files:** [My exact currently installed setup can be found on this GitHub](https://github.com/CharlieAvocado/Ender3v2NeoMRISCOC.git); if you need other files, let me know and I’ll happily drop them in.
- **Firmware Version: Currently** **installed Marlin firmware**, using [MRiscoC 20230312](https://github.com/mriscoc/Ender3V2S1/releases/tag/20230312) [v2.1.3], modified specifically for the 3 V2 Neo by using the [Lash-L files](https://github.com/Lash-L/Ender-3-V2-Neo-Setup/tree/main/Marlin_Configurations). I went with the 20230312 version because that’s the last one that Lash-L had provided matching configuration files for, and anything was better than the stock Ender firmware. [](https://github.com/CharlieAvocado/Ender3v2NeoMRISCOC.git)
    - **Other versions:** I ran stock firmware for a LONG time, until putting on the new extruder. That’s when I decided to change it up. I’ve compiled my own versions of Marlin using MRiscoC’s various Ender3V2 builds, personally modifying differences to accommodate for the 3 V2 Neo. I’ve tried others’ compilations I found on GitHub, including MRiscoC’s Special Configurations . Nothing has worked. Attempting to return to the stock Ender V4.2.2 firmware (Ender-3 V2 Neo-Marlin2.0.8.3-HW-V4.2.2-SW-V1.1.4-CR-Touch) also failed. As did attempting the newest version of that firmware (Ender-3 V2 Neo-Marlin2.0.8.3-HW-V4.2.2-SW-V1.1.5.2-CR-Touch-20230312(En&Jap)). I even attempted Klipper, but it was honestly beyond me. Z-Axis remains unworking.
    - **If you are going to suggest I try new firmware,** you better be making that suggestion only because it is the only thing that will fix the issue, not just because you like it better than Marlin. Please and thank you. **I’m not looking for improvements or code changes, unless they fix my Z-Axis issue.**
- **Relevant Settings:**
    - **Motherboard**: [Configuration.h, lines 91-96.]
        
        ```cpp
        // Choose the name from boards.h that matches your setup
        #ifndef MOTHERBOARD
          #define MOTHERBOARD BOARD_CREALITY_V422
          // #define MOTHERBOARD BOARD_CREALITY_V422 //if you have 4.2.2
          // #define MOTHERBOARD BOARD_CREALITY_V427 //if you have 4.2.7
        #endif
        ```
        
    - **Z-Axis Limits**: [Configuration.h, lines 1772-1777.]
        
        ```cpp
        #define X_MIN_POS -5  // MRiscoC Stock physical limit
        #define Y_MIN_POS 0  // MRiscoC Stock physical limit
        #define Z_MIN_POS 0
        #define X_MAX_POS 238  // MRiscoC Stock physical limit
        #define Y_MAX_POS 229  // MRiscoC Stock physical limit
        #define Z_MAX_POS 250  // Ender Configs
        ```
        
    - **Z Probe Options**: [Configuration.h, lines 1332, 1341-1366.]
        
        ```cpp
        /**
         * Enable this option for a probe connected to the Z-MIN pin.
         * The probe replaces the Z-MIN endstop and is used for Z homing.
         * (Automatically enables USE_PROBE_FOR_Z_HOMING.)
         */
        //#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN  // Probe connected to BLTouch port
        
        // Force the use of the probe for Z-axis homing
        #define USE_PROBE_FOR_Z_HOMING
        
        /**
         * Z_MIN_PROBE_PIN
         *
         * Define this pin if the probe is not connected to Z_MIN_PIN.
         * If not defined the default pin for the selected MOTHERBOARD
         * will be used. Most of the time the default is what you want.
         *
         *  - The simplest option is to use a free endstop connector.
         *  - Use 5V for powered (usually inductive) sensors.
         *
         *  - RAMPS 1.3/1.4 boards may use the 5V, GND, and Aux4->D32 pin:
         *    - For simple switches connect...
         *      - normally-closed switches to GND and D32.
         *      - normally-open switches to 5V and D32.
         */
        //#define Z_MIN_PROBE_PIN 32 // Pin 32 is the RAMPS default
        ```
        
    - **BLTouch Settings**: [Configuration.h, lines 1400-1403.] Note: I tried changing this to CRTOUCH, since that’s what I technically have, but it did NOT like that, so I presume BLTOUCH covers all the options shown there.
        
        ```cpp
        	/**
         * The BLTouch probe uses a Hall effect sensor and emulates a servo.
         */
        #define BLTOUCH  // 3D/CR/BLTouch version
        ```
        

**Troubleshooting Steps Taken:**

- **I have read any and every Reddit thread I could find** on Ender3 V2 Neo issues related to the Z-Axis. Then I looked at any Ender-family Z-Axis issues. I’ve viewed a lot of associated YouTube videos. Some things seemed applicable, some didn’t, I pretty much tried them all regardless. I can’t fathom what’s wrong. All the troubleshooting I’ve done has fixed it for everyone else, but not me, thinking I have a weird problem. **Mentally, I’ve narrowed it down to most likely relating to, in no particular order:**
    - **Hardware**: The [official Creality Ender 3 Direct Drive Extruder Kit](https://www.amazon.com/Creality-Upgraded-Extruder-Flexible-Filament/dp/B08J7N2LNL/ref=sr_1_4?crid=2O5XX6D7ICJTD&keywords=ender+3+hotend+upgrade+kit&qid=1699636828&sprefix=ender+3+hotend+upgrade%2Caps%2C82&sr=8-4) is not only not recommended for my machine, but **perhaps it’s directly incompatible**? Or something on my motherboard is damaged and I don’t know it. Aside from the Z-Axis issue, everything seems fully operational. And the Z-Axis ****does**** technically work, in that it will descend in some circumstances, and can easily be manually adjusted.
    - **G-Code/Firmware**: Incompatibility. Potentially because of my TJC display board? Doubtful. Because the firmware is modified to work for an Ender 3 V2 Neo, but was not technically built for it? No evidence to prove or disprove that answer yet(?). Firmware works smoothly unless the Z-Axis is involved, so **if a firmware issue**, my bet would be on any code related to the Z-Axis, and I simply don’t know enough to fix it on my own.
    - **Wiring**: I really don’t think it’s the wiring. I have previously twisted the CR-Touch connector together into a bundle because some believe that interference is the issue there. I saw no change, plus it seems to operate just fine when not bundled; the probe deploys and returns appropriately, all the right lights are flashing. It’s the Z-Axis that doesn’t respond. Along with checking for continuity in the Z-Axis wires and the CR-Touch wires, I made sure all the connection ends looked like they would appropriately make contact and ensured all pins they connect to appeared undamaged. Everything appears to be connected to the right locations, at the right level of secureness.
    - **Miscellaneous**: All belts are in great shape, all rolling parts are appropriately moving and tensioned properly. All nuts/bolts accounted for and properly secured. I don’t actually have an X-Axis stop to impact the X-Axis Probe sensor, but I rigged a temporary one and it works just fine; I would assume this is irrelevant, but who knows at this point.

**Request for Help:**
I'm looking for advice on what could be causing these issues and how to fix them. Any insights into potential mechanical, electrical, or firmware-related causes that I haven’t yet ruled out, or perhaps did so incorrectly, would be greatly appreciated. Thank you in advance for your help! I’m newer to this, but learning fast and well. I’d like to think I can keep up!
