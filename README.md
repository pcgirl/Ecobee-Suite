Free Ecobee Suite, version: 1.4.* 
======================================================
Latest: Version 1.4.0 Released February 11, 2018

REALLY IMPORTANT!!!
=====================================================
Early morning February 11, 2018, I realized a significant issue with release 1.3.0 that caused conflicts with the installation of this release in SmartThings locations that had prior releases already installed and nut fully released. The fix has necessitated a complete new re-release as version 1.4.*. 

***Users who have already installed v1.3.0 should immeditaely remove it and install this new version. If you have any issues with this, please PM me directly.***

<b>The significant changes to 1.4.0 include:</b>
- Ecobee Suite (Connect) has been renamed to <b>Ecobee Suite Manager</b>
- Created devices now named "EcobeeSensor: (ecobee sensor name)" and "Ecobee Thermostat: (ecobee thermostat name)" to distinguish from prior versions
- Created Sensors' SmartThings Network ID will be different from prior releases (this is what caused most of the issues with the 1.3.0 release)

Important Notice!
-----------------
>*The Ecobee Suite version you find here (in SANdood/Ecobee-Suite) has been renamed from prior versions and moved into my private namespace (sandood), to allow it to be installed alongside the stock SmartThings device (instead of replacing it). Users of prior versions of my suite (or of @StrykerSKS version) **MUST** install this completely from scratch. Please see the [Upgrading from Prior Releases](#upgrading) section below for more information.*

## <a name="top">Table Of Contents</a>
- [Introduction](#intro)
  - [Motivation](#motivation)
  - [Donations](#donations)
  - [What's New](#whazzup)
- [Installation](#installation)
  - [Upgrading from Prior Releases](#upgrading)
  - [Installation Preparation](#install-prep)
  - [Installing Using GitHub Integration](#github-install)
	  - [Installing Device Handlers](#github-devices)
	  - [Installing SmartApps](#github-smartapps)
	  - [Enabling oAuth](#github-oauth)
  - [Installing Manually from Code](#manual-install)
	  - [Installing Device Handlers](#manual-devices)
	  - [Installing Ecobee Suite Manager](#manual-manager)
	  - [Endabling oAuth](#manual-oauth)
	  - [Installing Helper SmartApps](#install-helpers)
  - [Finalizing Installation on SmartThings Mobile](#install-mobile)
  - [Updating the Code](#updating)
	  - [Updating using GitHub](#github-updates)
	  - [Updating Manually from Code](#manual-updates)
- [Features](#features)
  - [General Overview](#general)
  - [Thermostat and Sensor Device User Interfaces](#features-therm-ui)
	  - [Thermostat User Interface](#thermostat)
	  - [Sensor User Interface](#sensor)
	  - [SmartThings Integration](#smartthings)
	  - [Operational Enhancements](#operational)
  - [Screenshots](#screenshots)
  - [Ecobee Suite Manager SmartApp](#features-manager-sa)
  - [ecobee Suite Routines SmartApp](#features-routines-sa)
  - [ecobee Suite Open Contacts SmartApp](#features-opencontact-sa)
  - [ecobee Suite Smart Circulation SmartApp](#features-smart-circ-sa)
  - [ecobeeSuite Smart Mode](#smart-mode-sa) ***(NEW!)***
  - [ecobee Suite Smart Room SmartApp](#features-smart-room-sa)
  - [ecobee Suite Smart Switches SmartApp](#features-smart-switches-sa)
  - [ecobee Suite Smart Vents SmartApp](#features-smart-vents-sa)
  - [ecobee Suite Smart Zones SmartApp](#features-smart-zone-sa)
- [Troubleshooting](#troubleshooting)
  - [Reporting Issues](#reporting-issues)
- - [Quick Links](#quicklinks)
- [License](#license)
- [Integrating With Ecobee Suite Devices](#integration)
  - [Supported Device Attributes](#attributes)
  - [Supported Commands](#commands)
	  - [Changing Programs](#changeprogram)
	  - [Changing Thermostat Modes](#changingmode)
	  - [Changing Thermostat Fan Modes](#changingfan)
	  - [Complex Commands Requiring Arguments](#complex)

## <a name="intro">Introduction</a>
This document describes the various features related to the Ecobee Suite of Device Handlers and supporting Helper SmartApps for Ecobee thermostats and sensors. 

The following components are part of the Suite:
- **Ecobee Suite Manager SmartApp**: This SmartApp provides a single interface for Ecobee Installation, Authorization, Device Setup (both Thermostats **and** Sensors), Behavioral Settings and even a Debug Dashboard. Additional features can be added over time as well thanks to built in support for Child SmartApps, keeping everything nicely integrated into one app. 

   ***IMPORTANT**: This is the "parent" SmartApp to the entire Ecobee Suite - **users should not install any instances of the other SmartApps or devices**, neither on their mobile devices nor in the IDE.*
 
- **ecobee Suite Routines Helper SmartApp**: Child Helper SmartApp that lets you trigger settings changes on your Ecobee thermostats based on the SmartThings Hello Modes and/or Routines. This version also supports changing SmartThings Mode or executing a Routine when the thermostat enters a new Program or Vacation. Settings include the Ecobee Program (Comfort Settings), Fan Modes and Hold Types. In additional to SmartThings Hello Modes, sunrise/sunset triggers are also support. Multiple instances of this SmartApp are also supported for maximum flexibility.
- **Ecobee Suite Smart Circulation Helper SmartApp**: Child Helper SmartApp that adjusts hourly circulation time (fan on minimum minutes) trying to get (and keep) 2 or more rooms (temperature sensors, either Ecobee or SmartThings) to within a configurable temperature difference.
- **Ecobee Suite Smart Mode Helper SmartApp** ***New***: Automatically change the thermostat(s) Mode (Auto, Cool, Heat, Off, etc.) based on an external (outdoor) weather temperature.
- **Ecobee Suite Smart Room Helper SmartApp**: Child Helper SmartApp that automates a "Smart Room." Intended for rooms that are normally not used (doors, windows and automated vents closed), this app will Activate a "Smart Room" when the door is left open for a configurable period (defalt 5 minutes). Once active, the room's Ecobee Sensor is added to any of the user-selected default 3 Programs (Home, Away, Sleep), and (optionally) the automated vent (EcoNet or Keene) is opened. If the door is closed, after a configurable number of hours (default 12), it will De-activate the Smart Room, removing the Ecobee sensor from specified Programs and closing the vent. While active, opening a window will temporarily de-activate the room until the window is closed. If motion is detected while the door is closed, the automated de-activation will be cancelled until the next time the door is closed (and no motion is detected. Requires: Ecobee thermostat(s) with Ecobee Temp/Occupancy Sensor and Door contact sensor. Optional: Window contact sensor(s), EcoNet or Keene smart vents.
- **Ecobee Suite Smart Switches Helper SmartApp**: Child Helper SmartApp that will turn on/off one or more switches, and/or set the level on one or more dimmers (also works for vents), all based on changes to the Operating State of one or more thermostats. Intended to turn on vent booster fans when HVAC is pushing air, and (optionally) turn them off when they return to idle state. Can also be used to control lights and dimmers; dimmer control can be used to operate vents (see also Smart Vents Helper, below).
- **Ecobee Suite Smart Vents Helper SmartApp**: Child Helper SmartApp that will open and close one or more SmartThings-controlled HVAC vents based on room temperature in relation to specified heating/cooling target setpoint, or the setpoint of the specified thermostat.
- **Ecobee Suite Smart Zones Helper SmartApp**: CChild Helper SmartApp that attempts to synchronize fan on time between two zones. Intended for use on multi-zones HVAC systems; running the fan on all the zones simultaneously could help reduce electricity consumption vs. having each zone run the fan independently.
- **Ecobee Thermostat Device Handler (DTH)**: The Device Handler for the Ecobee Thermostat functions and attributes. Presents a much more detailed view of the thermostat/HVAC system status, including heat/cool stages, humidification & dehumidification status, fan circulation control, and active/inactive icons for many modes and states. Designed using icons representative of the Ecobee web and mobile apps. Also supports a broad suite of extended attributes and commands to allow WebCOrE and SmartThings apps to monitor and control the thermostat, allowing customization and integration within a users' Smart Home. 
- **Ecobee Sensor Device Handler (DTH**: This implements the Device Handler for the Ecobee Sensor attributes. Includes indicators for current thermostat Program, buttons that allow adding/removing the zone from specific Programs (Home, Away, Sleep only), and indicators that appear when Smart Room is configured for the room. This is also used to expose the internal sensors on the Thermostat to allow the actual temperature values (instead of only the average) to also be available. This is critically important for some applications such as smart vents.

Here are links to the working version of the repository being developed and maintained by Barry A. Burke [(on GitHub)](https://github.com/SANdood/Ecobee-Suite) and [(on SmartThings Community)](https://community.smartthings.com/t/release-updated-ecobee-suite-v1-4-0-free/118597).

--------------------------------
### <a name="motivation">Motivation</a>

I maintain the original intent as defined by Sean:

The intent is to provide an Open Source DTH and SmartApps for Ecobee thermostats that can be used by the SmartThings community of users ***free of charge*** and ***without fear of the device disappearing in the future***. This will help ensure accessibility for all users and provide for an easy mechanism for the community to help maintain/drive the functionality.

Let me be clear: this is not a competition. I am not trying to replace or outdo any other version, and I personally have nothing against people making money off of their efforts. I have invested my time in this as Open Source so as to meet my own wants and desires, and I offer the result back to the community with no strings attached.

Please feel free to try any version you want, and pick the one that best fits your own use case and preference.

If you like this version and are so inclined, post a brief message of support on the SmartThings Community link above. And if you **don't** like it, or have any problems with the installation or operation of the suite, please also post on the link above. I strive for total transparency, and your use of this Suite does not require you to post positive reviews, nor are you prohibited in any way from posting negative reviews. 

-------------------------
### <a name="donations">Donations</a>

This work is fully Open Source, and available for use at no charge.

While not required, I do humbly accept donations. If you would like to make an *optional* donation, I will be most grateful. You can make donations to me on PayPal at <https://paypal.me/BarryABurke>

-------------------------------
## <a name="whazzup">What's New</a>

As mentioned, the most significant enhancement with the release of 1.4.* is the separation from the SmartThings Ecobee support, as well as from all prior versions of both @StrykerSKS' and my own Ecobee integration. Users of these prior versions should review the [Upgrading from Prior Releases](#upgrading) section below for more IMPORTANT information before installing this version.

### Key enhancements in release 1.4.0
- ***Significantly improved handling of API connection errors.*** While I cannot yet guarantee non-stop operation during all SmartThings and/or Ecobee Cloud issues, the code now silently recovers from most error conditions;
- Addition of the new [Smart Mode](#smart-mode-sa) Helper SmartApp, to automatically change your thermostat(s) mode based on (external) temperature changes
- Enhanced setpoint adjustments, including using the Up/Down arrows on the main tile;
- Enhanced UI, with the addition of
  - Current & target humidity levels (the latter only when humidifier or dehumidifier is enabled;
  - New slider to manually adjust fan circulation time (fanMinOnTime);
 <sp>

- Better (complete?) support for Celsius; 
- A cornucopia of performance optimizations to improve both initial installation and running performance
- Improved API status display (last row in the Thermostat UI) - now shows "WARN" status when a call to the Ecobee API times out

-------------------------
# <a name="installation">Installation</a>

## General

It is highly recommended that you use the GitHub Integration that SmartThings offers with their IDE. This will make it **much** easier for you to keep up to date with changes over time. For the general steps needed for setting up GitHub IDE integration, please visit <http://docs.smartthings.com/en/latest/tools-and-ide/github-integration.html> and follow the steps for performing the setup.

## <a name="upgrading">Upgrading from Prior Releases</a>

Since both the namespace and the name of the SmartApps & Devices has changed in this version, users will find that this release will install *alongside* instead of replacing their current installation. This means that you will be creating a completely new set of thermostat and sensor devices in SmartThings. And while you probably do not want to run two completely separate-but-similar devices, you may want to keep the old one around until you are satisfied that you have recreated your entire setup (including Helper SmartApps) within this new Suite.

There is **one key recommendation** if you want to run both an old and this new version: you should first change the polling frequency of your existing installation to 5 minutes or longer. Both versions will be polling the same API from your username, and excess polling may cause undesired timeouts in one or both instances.

Proceed with the [installation instructions](#install-prep) below, then return here.

Once you have successfully recreated your devices and supporting Helper SmartApps in the new Ecobee Suite, you can then proceed with removing your old support (should you choose to do so). It is **extremely IMPORTANT** that you follow these steps to remove your old implementation - *failure to do so may require you to solicit assistance from SmartThings Support to completely remove the old support.*

1. Using the old Ecobee (Connect) on your mobile device, remove all of the Helper SmartApps, one by one, and then exit out of Ecobee (Connect)
2. Select each of your old Sensor devices one by one, then tap the "SmartApps" tab to verify that each Sensor is not used by any existing SmartApps. If they are, remove them from these SmartApps (you'll probably want to replace them with the "new" Sensor devices)
3. Back in the old Ecobee (Connect), go to **Sensors** page, and de-select ALL of the sensors listed there. Save/Done back out of Ecobee (Connect), and all of the Sensor devices *should* be deleted automatically. Don't worry if they're not, though...it doesn't always work, but you can still proceed...
4. Select each of your old Thermostat devices, one by one, then tap the "SmartApps" tab, and verify that these also are not used by any SmartApps.
5. Back into Ecobee (Connect), and this time select the **Thermostats** page, de-select ALL of the thermostats listed there, and exit back out of Ecobee (Connect).
6. Finally, back into Ecobee (Connect) one last time, scroll down and go to the **Remove this instance** page, tap the big red Remove button and confirm. 

At this point, you should have totally removed all the old support, and (hopefully) have a fully functional Ecobee Suite v1.3.* installation.

If any devices or SmartApps fail to remove ***after following the above steps***, you can try to manually go into each device/SmartApp on your movile and remove them from within the DTH and/or SmartApp itself. You can also try to manually delete them from within your IDE. If neither of these work, you'll probably have to [email SmartThings Support](mailto://support@smartthings.com).

## <a name="install-prep">Install Preparation</a>

If you are not familiar with adding your own custom devices, then be sure to familiarize yourself with the [SmartThings IDE](https://graph.api.smartthings.com/) before you begin the installation process.

You will also need to make sure that you have your Ecobee username and password handy. You should login to <http://www.ecobee.com/> now to ensure you have your credentials.

## <a name="github-install">Installing Using GitHub Integration (Recommended Method)</a>
If this is your first time installing this new Ecobee Suite (versions 1.3.* and later) first follow these steps to link your IDE with the new Ecobee Suite repository (note: this is a new repository, different from the one used for prior versions - everyone will be required to perform these steps once to get to the new version).
1. Login to the IDE at [ide.smartthings.com](http://ide.smartthings.com)
2. Click on **`My Locations`** at the top of the page
3. Click on the name of the location that you want to install to
4. Click on the **`My Device Handlers`** tab
5.  Click **`Settings`**
6.  Click **`Add new repository`** and use the following parameters:
    - Owner: **`SANdood`**
    - Name: **`Ecobee-Suite`** *(note that the hyphen is required)*
    - Branch: **`master`**
7. Click **`Save`**

### <a name="github-devices">Installing Ecobee Suite Device Handlers</a>
Once the Ecobee Suite repository is connected to your IDE, use the GitHub integration to install the current version into your workspace. In the IDE:

8. Click **`Update from Repo`** and select the **`Ecobee-Suite`** repository we just added
9. Find and Select **`ecobee-sensor.groovy`** and **`ecobee-thermostat.groovy`**
10. Select **`Publish`**(bottom right of screen near the **`Cancel`** button)
11. Click **`Execute Update`**
	- Note the response at the top of the **`My Devices Handlers`** page. It should be something like "**`Updated 0 devices and created 2 new devices, 2 published`**"
	- Verify that the two devices show up in the list and are marked with Status **`Published`** (NOTE: You may have to reload the **`My Device Handlers`** screen for the devices to show up properly.)

Once completed, you can delete the OLD device handlers if you prefer. To do so, you can select the Edit Properties icon next to each device and select Delete.

### <a name="github-smartapps">Installing Ecobee Suite SmartApps</a>
Once you have both of the Ecobee Suite Device Handlers added and published in your IDE, it is time to add the Ecobee Suite SmartApps.

12. Click on the **`My SmartApps`** tab
13. Click **`Update from Repo`** and select the **`**Ecobee-Suite**`** repository we added earlier
14. Select the checkboxes next to **`ecobee-suite-manager.groovy`**, **`ecobee-suite-routines.groovy`**, **`ecobee-suite-open-contacts.groovy`**, **`ecobee-suite-smart-circulation.groovy`**, **`ecobee-suite-smart-mode.groovy`**, **`ecobee-suite-smart-room.groovy`**, **`ecobee-suite-smart-switches.groovy`**, **`ecobee-suite-smart-vents.groovy`**, and **`ecobee-suite-smart-zones.groovy`** (all 9 SmartApps listed)
15. Select **`Publish`**(bottom right of screen near the `Cancel` button)
16. Click **`Execute Update`**
	- Again, note the response at the top of the My SmartApps page. It should be something like "**`Updated 0 and created 8 SmartApps, 8 published`**"
	- Verify that all 9 of the SmartApps show up in the list and are marked with Status **`Published`**

### <a name="github-oauth">Enabling oAuth for `Ecobee Suite Manager`</a>
Finally, we must enable oAuth for the connector SmartApp as follows:

**NOTE: This is the most commonly missed set of steps, but failing to enable OAuth will generate cryptic errors later when you try to use the SmartApp. *So please don't skip these steps.***

17. Locate the **`Ecobee Suite Manager`** SmartApp from the list and Click on the **`Edit Properties`** button to the left of the SmartApp that we just added (looks like pencil on a paper)
18. Click on the **`oAuth`** tab (
19. Click **`Enable oAuth in Smart App`**
20. Click **`Update`** (bottom left of screen)
21. Verify that **`Updated SmartApp`** appears at the top of the screen

That's it - you are now all set to skip down to [install the Ecobee Suite from your mobile device](#mobile).

**REMINDER:** The entire Ecobee Suite is installed using the Ecobee Suite Manager SmartApp. You should not attempt to install the individual devices or SmartApps directly - this will undoubtedly result in a failed installation, and you may require assistance from SmartThings support to recover if you don't follow these instructions.

Once completed, you can delete the OLD SmartApps if you prefer. To do so, you can select the Edit Properties icon next to each of the old SmartApps (the ones NOT in the "sandood" namespace) and select Delete.

## <a name="manual-install">Installing Manually from Code</a>
For this method it is recommended that you have one browser window open on GitHub and another on the IDE. If you have used the GitHub installation method above, you can skip this section and [proceed to completing the installation on your mobile device](#install-smartapp-phone).

### <a name="manual-devices">Installing Ecobee Suite Device Handlers Manually</a>
Follow these steps to install the **`Ecobee Sensor`** Device Handler:
1. Login to the IDE at [ide.smartthings.com](http://ide.smartthings.com)
2. [IDE] Click on **`My Locations`** at the top of the page
3. [IDE]Click on the name of the location that you want to install to
4. [IDE]Click on the **`My Device Handlers`** tab
5. [IDE] Click **`New Device Type`** (top right corner)
6. [IDE] Click **`From Code`**
7. [GitHub] Go to the [raw source code for the Ecobee Sensor](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/devicetypes/sandood/ecobee-suite-sensor.src/ecobee-suite-sensor.groovy) *(click the hyperlink)*
8. [GitHub] Select all of the text in the window (use Ctrl-A if using Windows)
9. [GitHub] Copy all of the selected text to the Clipboard (use Ctrl-C if using Windows)
10. [IDE] Click inside the text box
11. [IDE] Paste all of the previously copied text (use Ctrl-V if using Windows)
12. [IDE] Click **`Create`**
13. [IDE] Click **`Save`**
14. [IDE] Click **`Publish`** --> **`For Me`**

Follow these steps to install the **`Ecobee Thermostat`** Device Handler:
15. [IDE] Click on the **`My Device Handlers`** tab
16. [IDE] Click **`New Device Type`** (top right corner)
17. [IDE] Click **`From Code`**
18. [GitHub] Go to the [raw source code for the Ecobee Thermostat](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/devicetypes/sandood/ecobee-suite-thermostat.src/ecobee-suite-thermostat.groovy) *(click the hyperlink)
19. [GitHub] Select all of the text in the window (use Ctrl-A if using Windows)
20. [GitHub] Copy all of the selected text to the Clipboard (use Ctrl-C if using Windows)
21. [IDE] Click inside the text box
22. [IDE] Paste all of the previously copied text (use Ctrl-V if using Windows)
23. [IDE] Click **`Create`**
24. [IDE] Click **`Save`**
25. [IDE] Click **`Publish`** --> **`For Me`**

Once completed, you can delete the OLD device handlers if you prefer. To do so, you can select the Edit Properties icon next to each device and select Delete.

### <a name="manual-manager">Installing `Ecobee Suite Manager` Manually from Code</a>
Again, it is recommended to have one browser window open on GitHub and another on the IDE.

Follow these steps to add the **`Ecobee Suite Manager`** SmartApp to your IDE:
1. [IDE] Click on the **`My SmartApps`** tab
2. [IDE] Click **`New SmartApp`** (top right corner)
3. [IDE] Click **`From Code`**
4. [GitHub] Go to the [raw source code for the Ecobee Suite Manager SmartApp](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/smartapps/sandood/ecobee-suite-manager.src/ecobee-suite-manager.groovy)
6. [GitHub] Select all of the text in the window (use Ctrl-A if using Windows)
7. [GitHub] Copy all of the selected text to the Clipboard (use Ctrl-C if using Windows)
8. [IDE] Click inside the text box
9. [IDE] Paste all of the previously copied text (use Ctrl-V if using Windows)
10. [IDE] Click **`Create`**
11. [IDE] Click **`Save`**
12. [IDE] Click **`Publish`** --> **`For Me`**

### <a name="manual-oauth">Enabling oAuth for `Ecobee Suite Manager`</a>
Finally, we must enable oAuth for the connector SmartApp as follows:

**NOTE: This is the most commonly missed set of steps, but failing to enable OAuth will generate cryptic errors later when you try to use the SmartApp. *So please don't skip these steps.***

13. [IDE] Click on the **`My SmartApps`** tab
14. [IDE] Verify that the SmartApp shows up in the list and is marked with Status `Published`
15. [IDE] Click on the **`Edit Properties`** button to the left of the SmartApp that we just added (looks like pencil on a paper)
16. [IDE] Click on the **`oAuth`** tab
17. [IDE] Click **`Enable oAuth in Smart App`**
18. [IDE] Click **`Update`** (bottom left of screen)
19. [IDE] Verify that **`Updated SmartApp`** appears at the top of the screen

### <a name="manual-helpers">Installing the Ecobee Suite Helper SmartApps Manually from Code</a>
Follow these steps to add the Helper SmartApps to your IDE, one at a time (links to Raw Source Code are provided below):

**NOTE:** It is mandatory that you add ALL of the available Helper SmartApps, or **`Ecobee Suite Manager`** will fail with an error.
20. [IDE] Click on the **`My SmartApps`** tab
21. [IDE] Click **`New SmartApp`** (top right corner)
22. [IDE] Click **`From Code`**
23. [GitHub] Go to the raw source code for each of the Helper SmartApps, one at a time (links provided below)
24. [GitHub] Select all of the text in the window (use Ctrl-A if using Windows)
25. [GitHub] Copy all of the selected text to the Clipboard (use Ctrl-C if using Windows)
26. [IDE] Click inside the text box
27. [IDE] Paste all of the previously copied text (use Ctrl-V if using Windows)
28. [IDE] Click **`Create`**
29. [IDE] Click **`Save`**
30. [IDE] Click **`Publish`** --> **`For Me`** (Optional)
31. [IDE] Click on the **`My SmartApps`** tab
32. [IDE] Verify that the SmartApp shows up in the list

Repeat the above steps (20-32) for the each of the available Helper SmartApps (click each link for the raw source code):
A. [`ecobee Suite Open Contacts`](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/smartapps/sandood/ecobee-suite-open-contacts.src/ecobee-suite-open-contacts.groovy)
B. [`ecobee Suite Routines`](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/smartapps/sandood/ecobee-suite-routines.src/ecobee-suite-routines.groovy) 
C. [`ecobee Suite Smart Circulation`](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/smartapps/sandood/ecobee-suite-smart-circulation.src/ecobee-suite-smart-circulation.groovy) 
D. [`ecobee Suite Smart Mode`](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/smartapps/sandood/ecobee-suite-smart-mode.src/ecobee-suite-smart-mode.groovy) 
E. [`ecobee Suite Smart Room`](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/smartapps/sandood/ecobee-suite-smart-room.src/ecobee-suite-smart-room.groovy)
F. [`ecobee Suite Smart Switches`](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/smartapps/sandood/ecobee-suite-smart-switches.src/ecobee-suite-smart-switches.groovy)
G. [`ecobee Suite Smart Vents`](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/smartapps/sandood/ecobee-suite-smart-vents.src/ecobee-suite-smart-vents.groovy)
H. [`ecobee Suite Smart Zones`](https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/smartapps/sandood/ecobee-suite-smart-zones.src/ecobee-suite-smart-zones.groovy)

Once completed, you can delete the OLD SmartApps (the ones that appear under the `SmartThings` namespace) if you prefer. To do so, you can select the Edit Properties icon next to each of the old SmartApps (the ones NOT in the "sandood" namespace) and select Delete.

## <a name="install-mobile">Finalizing Installation on SmartThings Mobile</a>

The final steps for installing the Ecobee Suite Device Handlers and SmartApps is completed entirely using the Ecobee Suite Manager SmartApp. The SmartApp will guide you through the basic installation and setup process. It includes the following aspects:
- Authentication with Ecobee to allow API Calls for your thermostat(s) (and connected sensors)
- Discovery, selection and creation of Thermostat devices
- Discovery, selection and creation of Remote Sensor devices (if there are any)
- Setup of optional features/parameters such as Smart Auto Temp Control, Polling Intervals, etc
- Installation and configuration of Helper SmartApps (as desired)

Follow these steps for the SmartApp on your mobile device:
1. Open the SmartThings app and login to your Location
2. Open the **`Marketplace`** tab
3. Open the **`SmartApps`** tab
4. Scroll down and select **`My Apps`** (at the bottom of the list)
5. Scroll to open the **`Ecobee Suite Manager`** SmartApp.
(**NOTE:** If you see "Ecobee (Connect)" in this list, you did not remove it from your IDE. This is not a problem - you can use both. Just remember that this new Ecobee Suite uses **`Ecobee Suite Manager`**, and not the old version.
6. Select the Ecobee API Authorization page (as indicated on the screen) and then the ecobee Account Authorization page to enter your Ecobee Credentials
7. Enter your Ecobee Email and Password, and then press the green LOG IN button
8. On the next page, Click **`Accept`** to allow SmartThings to connect to your Ecobee account
9. You should receive a message indicating **`Your ecobee Account is now connected to SmartThings!`**
10. Click **`Done`** (or **`Next`** depending on your device OS)
11. Click **`Done`** (or **`Next`** depending on your device OS) again to save the credentials and exit out of Ecobee Suite Manager 
You should receive a small green popup at the top stating "**`Ecobee Suite Manager is now installed and automating`**"
12. In the SmartThings mobile app, go to the **`My Home`** screen and select the **`SmartApps`** tab
13. Select the the **`Ecobee Suite Manager`** SmartApp that you just installed
14. Select the Ecobee Thermostat devices you want to connect from your account
15. Select the Ecobee Sensors you want to connect (if any)
(NOTE: The options are dynamic and will change/appear based on other selections. E.g., you won't see any Thermostats to select if your Ecobee account login failed; you won't see any sensors to choose until you select the thermostats they are connected to).
16. You can also go into the **`Preferences`** section to set various preferences such as **`Hold Type`**, **`Smart Auto Temperature`**, **`Polling Interval`**, **`Debug Level`**, **`Decimal Precision`**, and whether to create separate sensor objects for thermostats.]  - 
17. After making all selections, Click **`Done`** to save your preferences and exit the SmartApp

At this point, the SmartApp will automatically create all of the new devices, one for each thermostat and sensor. These will show up in your regular **`Things`** list within the app. 

Using the default settings is fine, but some of the more advanced features will require you to change the default settings for Ecobee Suite. My recommended settings are:
- **Hold Type:** If not specified, this will default to the setting on the thermostat itself. Some of the helper SmartApps allow you to customize this for specific operations.
- **Polling Interval:** 1 minute if you are using Helper SmartApps to react to thermostat conditions (like changing to heating/cooling, or running a specific Program/Climate); otherwise 2-5 minutes might be sufficient.
- **Decimal Precision:** One of the reasons I created this in the first place is to get more precision out of the thermostat. If you set this to 1 decimal position, you'll understand better how the thermostat is reacting to your environment, and when "72°" is really "72.4°".
- **Debug Level:** Although the default is 3, I tend to run with this set to 2 because I've optimized this level to provide the most basic of operational status in the Live Logging monitor.
- 

> **NOTE 1**: It may take a few minutes for the new devices to show up in the list and for them to populate their displays. You should try refreshing the list if they are not there (pull down on the list). In extreme cases, you may have to restart the SmartThings app on your mobile device to update the list. You should only have to do this once.<br/>

> **NOTE 2**: If you uninstall the SmartApp it will automatically remove all of the thermostats and sensors that it previously installed. This is necessary (and expected) as those devices are "children" of the SmartApp.<br/>


-------------------------
## <a name="updating">Updating the Code</a>
If you have enabled GitHub integration with the SmartThings IDE, then updates are a breeze. Otherwise the steps are a bit more manual but not too complicated.

### <a name="github-updates">Updating with GitHub Integration</a>
The IDE provides visual cues to alert you that any device types or SmartApps have been updated in their upstream repositories. See the [GitHub/IDE integration guide](http://docs.smartthings.com/en/latest/tools-and-ide/github-integration.html) for more details on the different colors.

Once you have determined that an update is available, follow these steps:
1. Login to the SmartThings IDE
2. Go to either the **`My Device Handlers`** or **`My SmartApps`** tabs to see if there are updates (the color of the item will be purple)
3. Click the **`Update from Repo`** button (top right)
4. Select the **`Ecobee-Suite`** repository
5. Select ALL of items in the **`Obsolete (updated in GitHub)`** column
6. Select **`Publish`** (bottom right)
7. Click **`Execute Update`** (bottom right)
	- You should receive a confirmation message such as this example: **`Updated 1 and created 0 SmartApps, 1 published`**
	- (Optional, but <b>HIGHLY Recommended</b>) Rerun the **`Ecobee Suite Manager`** SmartApp. This seems to alleviate any residual issues that may occur due to the update

You should now be running on the updated code. Be sure that you check for both updates of the SmartApp **and** the Device Type. Updating one but not the other could cause compatibility problems.

### <a name="manual-updates">Updating manually</a>

To update manually, you will need to "cut & paste" the raw code from GitHub into the SmartThings IDE, Save and Publish the code. I will leave it to the reader to work through the full individual steps, but the links to the code are the same as those that were used during the initial install process.

Note that there are EIGHT SmartApps and TWO Device Handlers in this suite. For proper operation, you will need to copy/paste ALL 10 of these files into your IDE, even if you aren't going to use them all. At a minimum, you should also publish the Ecobee Suite Manager SmartApp and the two Device Handlers.

--------------------------
## <a name="features">Features</a>
### <a name="general">General</a>
This collection of SmartApps and Device Handlers has been designed for simple installation, flexibile configuration options and easy operation. It is also extensible through the use of Child SmartApps that can easily be added to the configuration. **And it fully implements the related [SmartThings Capabilities](http://docs.smartthings.com/en/latest/capabilities-reference.html).**

Key Highlights include:
- **Open Source Implementation!** Free as in beer AND speech. No donations or purchase needed to use (but if you insist, see [donations](#donations).
- Single installation SmartApp, **`Ecobee Suite Manager`** used for installing both Thermostats **and** Sensors. No need for multiple apps just for installation! In fact, the **`Ecobee Suite Manager`** SmartApp is the only SmartApp interface you'll need to access all available functions, including those provided by Child SmartApps (if installed).
- Sophisticated User Interface: Uses custom Ecobee icons throughout the design to provide a more polished look and feel.
- Display of current weather with support for separate day and night icons (just like on the front of your Ecobee thermostat)!
- Robust watchdog handling to minimize API Connectivity issues, but also includes an API Status Tile to quickly identify if there is an ongoing problem. No more guessing if you are still connected or not.
- Included Helper SmartApp (**`ecobee Suite Routines`**) for automating thermostat settings based on SmartThings Hello Modes being activated (such as through a Routine), and/or automating SmartThings Modes/Routines based on Ecobee Thermostat Program changes (including Vacations).
- Additional Smart Helper Apps to automate ciculation time, coordinate fan-on time between multiple zones, and automate a "Smart Room."
- Full support for both Fahrenheit and Celsius

### <a name="features-therm-ui">Thermostat and Sensor Device User Interfaces</a>
The primary user interface on a day-to-day basis will be two different types of Device Handlers that are shown on the **`Things`** list under **`My Home`** in the mobile app. 

#### <a name="thermostat">Thermostat UI</a>
  - The main MultiAttributeTile tile now reflects additional information
    - Background colors match Ecobee3 thermostat colors for Idle (magenta), Heat (orange) & Cool (blue), and Fan Only (green) and Offline (red) are added
    - Displays (Smart Recovery) when heating/cooling in advance of a Program change
    - Displays the setpoint temperature that will initiate a heat/cool demand while idle, and the actual target temp while heating/cooling. 
      - On iOS, the call-for-heat temperature will be indicated by "Heating **at** XX.X°" in the large tile (while idle), and the target temperature (while heating) "Heating **to** YY.Y°". Same for Cool and Auto modes
      - Android mistakenly says "Heating **at** XX.X" for both modes; bug reports have been filed to get this corrected (again).
    - All temperatures can be configured to display in 0, 1 or 2 decimal positions (with appropriate rounding)
  - Most icons are dynamic and in full color
    - New Operating State icons reflect current stage for multi-stage Heat or Cool, plus humidification/dehumidification (if configured)
    - Hold: Program icons are now "filled" when in a Hold event so you can recognize the Hold at a glance
    - Buttons that are inactive due to the current state/mode/program are greyed out
    - Display-only buttons do not invoke any actions
  - Dynamic "Resume" button
    - Normally displays as Resume (Program) button, to override a current Hold: event
    - While thermostat is in a Vacation event, the button becomes 'Cancel" to allow the cancelation of the Vacation
  - Hold/Vacation Status display
    - Displays when current Hold or Vacation ends
    - If the thermostat loses its connection to the Ecobee Cloud, displays the time that the last valid update was recieved
  - Default Hold handling
    - In addition to Permanent and Temporary holds, now supports hourly holds: 2 hours, 4 hours or Custom Hours.
    - The default hold type can be configured in Ecobee Suite Manager or for each individual thermostat in Thermostat Preferences.
    - Also supports using the thermostat's preference setting (askMe is not currently supported)
  - New Refresh actions
    - If pressed once, will only request the latest updates from the Ecobee Cloud
    - If pressed a second time within a few seconds of the first press completing, will force a full update from the Ecobee Cloud
   
#### <a neme="sensor">Sensor UI</a>
  -	A new multiAttributeTile replaces the old presentation, with motion displayed at the bottom left corner
  - Now displays the parent thermostat current Program within each Sensor device
  - New mini-icons indicate which of the 3 default programs (Home, Away, Sleep) the sensor is included in
    - The sensor can be added or removed from a Program by tapping these mini icons
  - Includes 4 new blank mini-tiles that are utilized by the new Smart Room Helper App
	
#### <a name="smartthings">SmartThings Integration</a>
  - Messages sent to the Device notification log (found under the Recently tab of a Device) are now optimized, most with associated colored icons 
  - All current Attributes and Capabilities of Thermostat devices are supported
  - Most Ecobee Settings Object variables are are now available as Attributes of a Thermostat (so things like WebCoRE can see and use them) 
  - Supports the latest updated Thermostat Attributes released by SmartThings on July 7, 2017
  - Now offers several new API Command interfaces - see **`ecobee-thermostat.groovy`** for specifics. These include new API commands to:
    - Add a sensor to a Program
    - Delete/Remove a sensor from a Program
    - Delete the current Vacation
    - Change the minimum fan on time for both the current running Program and current Vacation event

#### <a name="operational">Operational Enhancements</a>

  - **Operational Efficiency**
    - Redesigned to do only lightweight polls of the Ecobee Cloud before requesting updates
    - If updates are available, then only requests those updated objects, and only for the thermostats with updates
    - From the updated Ecobee objects, only the data that has changed is sent to the individual devices
    - As a result of the above, it is possible to run will polling Frequency less than the recommended 3 minutes (as low as 1 minute)
<br>

  - **Timeout Handling.**
Releases prior to 1.3.0 did not handle connection timeouts to the Ecobee servers very well. That is a gross understatement, because the "handling" was limited to sending notifications every hour nagging that the user needed to reauthorize with Ecobee.<br>
Beginning with 1.3.0, this Ecobee Suite will silently retry connection timeouts without sending any notifications (it will report warnings in Live Logging). Usually such timeouts are transient, and the side effect is only a delayed update. For lengthy outages, the code *should automatically reconnect* once the SmartThings to Ecobee linkage is restored.<br>
Thus, if you receive a notification that you need to re-authorize, you probably really do need to reauthorize.

  - **Stacked Holds**
Ecobee devices support a concent of "stacked" holds - each hold operation can be stacked on top of a prior hold, such that Resuming the current hold merely "pops the stack" to the prior hold. This is a very difficult concept to manage across multiple interfaces, because it is difficult to depict what the result of a "Resume" operation will be at any given point in time.<br>
This implementation avoids the complexity by supporting only a single depth of Hold: events - whenever you execute a Resume you reset the thermostat to run the currently scheduled Program.
        
<b>Ask Alexa Message Queue Support</b>

 - For users of Michael Struck's most awesome Ask Alexa integration for SmartThings <http://thingsthataresmart.wiki/index.php?title=Ask_Alexa>
 	- you can now configure your Ecobee Alerts and Reminders to be sent to Ask Alexa Message Queues. 
 	- This is enabled in a new Ask Alexa preferences page in Ecobee Suite Manager where you can specify the target Message Queue(s). 
	- You can also specify an expiration time for Alerts so that they are removed from Ask Alexa after a specified period of time.

NOTE: You will not be able to configure Ask Alexa support until you have fully installed Ask Alexa <b>AND</b> created at least 1 custom Message Queue, as the Primary Queue is being deprecated.

- The current implementation does not support acknowledging of messages directly via Ask Alexa, but when the Alert/Reminder is acknowledged (on the thermostat or in the Ecobee application/web site) they are also removed from the Ask Alexa Message Queue.

- This feature will evolve over the coming months to include more control over what gets sent to Ask Alexa and to support Notifications Only. So stay tuned!

## <a name="screenshots">Screenshots</a> 


Ecobee Thermostat Device |  Ecobee Thermostat Device with Annotation
:-------------------------:|:-------------------------:
<img src="https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/images/v1.3.0%20Tstat.png" border="1" height="1200" /> | <img src="https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/images/v1.3.0-Tstat-annotated.png" border="1" height="1200" />

Ecobee Sensor Device |  <nbsp;> 
:-------------------------:|:-------------------------:
<img src="https://raw.githubusercontent.com/SANdood/Ecobee-Suite/master/images/v1.3.0-sensor.jpg" border="1" width="295" /> | 

### Annotation Notes
- **Button** Executes labeled action/state when pressed (unless grey)|
- **Cycler** Displays current state, toggles to next state when pressed|
- **Slider** Opens SmartThings Slider control (limited to whole numbers only)|
- **Current Temperature** Decimal value of current Temperature display on Thermostat (note: may be an average of the associated sensors)
- **Up / Down Setpoint buttons** will be slow to respond, but will not actually change the setpoint until 5 seconds after last button pressed (configurable delay in Ecobee Suite (Control) Preferences). Operates in 1° increments in Fahrenheit, and 0.5° increments in Celsius
- **Thermostat Operating State** describes the current activity, or the temperature at which a demand for heat/cool will be made (heating/cooling/fan only/idle/pending heat/pending cool)
- **Refresh Button** Tap once to request a changes-only poll to the Ecobee API, tap again immeditately after the "Bee" icon reappears to force a poll of ***all*** the Ecobee API
- **Equipment Operating State** more detailed state of operations, including heat/cool stages, humidifier/dehumidifier, Smart Recovery, emergency/aux heat, etc.
- **Hold/Vacation Status** displays when current hold or vacation is scheduled to end. Occaisionally reports other status of interest
- **Thermostat Date & Time** date and time as of the last completed poll of the Ecobee API
- **Command Center** Most every tile below this is active, unless the tile is greyed out. Tiles are greyed if the action is not currently allowed because of some state (e.g., cannot change the Program when the thermostat is in Vacation mode).
- 

## <a name="features-manager-sa">`Ecobee Suite Manager` SmartApp</a>
The **`Ecobee Suite Manager`** SmartApp provides a single SmartApp interface for accessing installation, updates, Child SmartApps and even debugging tools. The interface uses dynamic pages to guide the user through the installation process in order simplify the steps as much as possible.

The SmartApp provides the following capabilities:
- Perform Ecobee API Authorization (OAuth)
- Select Thermostats from account to use (dynamic list, so any future Thermostats can easily be added at a later date)
- Select Sensors to use (dynamic list, will only show sensors associated with the previously selected Thermostats)
- Access Child SmartApps (such as **`ecobee Suite Routines`**)
- Set various Preferences:
  - Set default Hold Type ("Until Next Program", "Until I Change", "2 Hours", "4 Hours", "Specified Hours" and "Thermostat Default")
  - Allow changes to temperature setpoint via arrows when in auto mode ("Smart Auto Temperature Adjust")
  - Polling Interval
  - Debugging Level
  - Adjustable Decimal precision for displaying temperatures
  - Include Thermostats as a separate Ecobee Sensor (useful in order to expose the true temperature reading and not just the average temperature shown on the thermostat, e.g. for Smart Vent input)
  - Monitor external devices to drive additional polling and watchdog events
  - Delay timer value after pressing setpoint arrows (to allow multiple arror presses before calling the Ecobee APIs)
- Set Ask Alexa Message Queue integration preferences
- Select Polling and Watchdog Devices (if enabled in Preferences)
- Debug Dashboard (if Debug Level is set to 5)


## <a name="features-routines-sa">`Mode/Routine/Program` Handler</a>
The `ecobee Suite Routines` SmartApp provides the ability to change the running Program (Comfort Setting) when a SmartThings Mode is changed (for example, by running a Routine) or a Routine is run. 

<b>Features:</b>
- Change one or multiple thermostats
- Trigger based on SmartThings Location Mode Change or SmartThings Routine Execution
  - Choose any (including custom) Ecobee Programs to switch to. Or can even choose to Resume Program instead
  - Change the Fan Mode (Optional)
  - Set the Fan Minimum Runtime (Optional)
  - Disable a Vacation hold (Optional)
- Trigger based on Program Change on one or more thermostats
  - Change the Location Mode or execute a Routine when the thermostat Program Changes - including Vacations!
- Temporarily Disable app without having to delete and recreate!

## <a name="features-opencontact-sa">`Contacts & Switches` Handler</a>
The `ecobee Suite Open Contacts` SmartApp can detect when one (or more) contact sensors (such as doors and windows) are left open for a configurable amount of time and can automatically turn off the HVAC and/or send a notification when it is detected. 

This Handler also supports switches/dimmers/vents, with configurable selection of whether contact open or close, switch on or off will turn off the HVAC. This enhancement allows (for example) automatically turning off the HVAC when the attic fan is running.

<b>Features:</b>
- Change one or multiple thermostats
- Trigger based on one or multiple contact sensors and/or switches
- Configurable delay timers (for trigger and reset)
- Configurable actions: Notify Only, HVAC Only or Both
- Support for Contact Book or simply SMS for notifications
- Temporarily Disable app without having to delete and recreate!

## <a name="features-smart-circ-sa">`Smart Circulation` Handler</a>
The `Ecobee Suite Smart Ciculation` SmartApp will adjust fan mininum on time of an Ecobee thermostat as it tries to get two or more temperature sensors to converge.

<b>Features:</b>
- Monitor/synchronize two or more temperature sensors (Ecobee or any SmartThings temperature sensor)
- Configurable maximum/target temperature delta
- Configurable minimum fan minutes on per hour
- Configurable maximum fan minutes on per hour
- Configurable minutes +/- per adjustment
- Configurable adjustment frequency (in minutes)
- Option to override fan minutes per hour during Vacation hold
- Enable during specific Location Modes or when thermostat is in specific Program
- Temporarily Disable app without having to delete and recreate!

## <a name="smart-mode-sa">`Smart Mode` Handler</a> (NEW!)
This (new) Helper will automatically change Ecobee Thermostats' Mode based on external temperature changes. Generally, the intention is to run Cool or Heat only modes when the outside temperatures might otherwise cause cycling between Heat/Cool if "Auto" were selected. Can also be used to turn the HVAC off when outdoor temperatures warrant leaving the windows open.

**Features**
* Choose from only the Modes that are configured on the thermostat (Auto, Aux_Heat, Cool, Heat, Off) 
* Choose from 4 possible temperature sources:
  * Zip Code (SmartThings Location or manually specified Zip Code) - uses SmartThings embedded support for WeatherUnderground data
  * Any SmartThings Temperature Sensor
  * The Ecobee-supplied outdoor Weather Temperature (note, this is notoriously bad data for most people, unless you live within a couple of miles of the weather sources Ecobee uses)
  * A nearby WeatherUnderground Personal Weather Station (pws) (the app will help locate nearby pws's)
* Optionally deliver Notifications whenever the thermostat(s) mode is changed
* Use a single instance (below, above and between temperatures), or create multiple instances

Many thanks to @JustinL for the original idea, the starting code base, and beta testing for this new Smart Mode Helper.

## <a name="features-smart-room-sa">`Smart Room` Handler</a>
The `ecobee Suite Smart Room` Helper SmartApp will automate a normally unused room, Activating it one or more of the Doors associated with the Smart Room are left open for a configurable number of minutes, and Deactivating it when all the Doors are closed for a configurable number of hours.

<b>Requirements:</b>
  - Ecobee thermostat with Sensor support
  - Ecobee Sensor in the room
  - One or more Doors, each with a contact sensor
  
<b>Optional:</b>
  - SmartThings-controlled vent running stock device drivers
    - EcoNet Vent
    - Keene Smart Vent
  - One or more Windows with contact sensor(s)
  
<b>Features:</b>
  - Specify one or more Ecobee Sensors that are in the Smart Room
    - Configure which of the 3 default ecobee Thermostat Programs (Home, Away, Sleep) the Sensor should be added to when Activated
    - Configure which of these Programs it should be added to when Deactivated
  - Specify one or more Door contact sensors to monitor
    - Configurable number of minutes a Door being open will Activate the Smart Room
    - Configurable number of hours all Doors being closed will Deactivate the Smart Room
  - Optionally specify one or more Window contact sensors to monitor
    - If open while room is Activated, temporarily Deactivate the room
  - Optionally specify additional motion sensors within the Smart Room
    - If motion is detected while a Smart Room is Activated and all the Doors are closed *and* the Deactivate monitor is disabled (monitor will be re-enabled the next time the door closes)
  - Optionally controls specified SmartThings-controlled vents
    - Open vent(s) when Smart Room is activated, and close them when Deactivated
    - Supports EcoNet and Keene vents using SmartThings stock DTH only
    - Optionally configure a minimum vent closed level (to protect over-pressure HVAC situation)
  - Optionally notify on Activations via Push, using Contact List or default Push
  - Temporarily Disable app without having to delete and recreate!
    
## <a name="features-smart-switches-sa">`Smart Switches/Dimmer/Vent` Handler</a>
`ecobee Suite Smart Switches` Helper SmartApp enables activating switches or dimmers based upon the Operating State of a thermostat.

## <a name="features-smart-vents-sa">`Smart Vents` Handler</a>
`ecobee Suite Smart Vents` automates one or more vents to reach/maintain a target temperature that you specify or from a specified thermostat. It uses one or more temperature devices, and opens/closes vents based upon current temperature, target temperature and thermostat operating state.

## <a name="features-smart-zone-sa">`Smart Zones` Handler</a>
`ecobee Suite Smart Zones` Documentation coming soon.


-------------------------
## Troubleshooting

| Symptom 	| Possible Solution 	|
|---------	|-------------------	|
| The devices are not showing up in the Things tab after installation    	|  It can take several minutes for things to show up properly. If you don't want to wait then simply kill the SmartThings app and reload it.              	|
| Receive error similar to "error java.lang.NullPointerException: Cannot get property 'authorities' on null object"        	| This indicates that you have not turned on OAuth for the SmartApp. Please review the installation instructions and complete the OAuth steps.                  	|
| "You are not authorized to perform the requested operation."        	|  This indicates that you have not turned on OAuth for the SmartApp. Please review the installation instructions and complete the OAuth steps.                 	|
| Irregular behavior after an update to the SmartApp or Device Handler code| It is possible that after updating the codebase that you may experience strange behavior, including possible app crashes. This seems to be a general issue with updates on SmartThings. Try the following steps: <br/> 1) Re-run the `Ecobee Suite Manager` SmartApp to re-initialize the devices <br/> 2) If that does not solve the problem, remove and re-install the SmartApp |
|         	|                   	|
|         	|                   	|

"You are not authorized to perform the requested operation."

### Debug Level in SmartApp
The `Ecobee Suite Manager` SmartApp allows the end user to config the Debug Level they wish to use (ranging from 1-5). The higher the level the more debug information is fed into the `Live Logging` on the SmartThings IDE.

Also, if a user chooses Debug Level 5, then a new `Debug Dashboard` will appear within the SmartApp. This dashboard gives direct access to various state information of the app as well as a few helper functions that can be used to manaually trigger actions that are normally timer based.

### Live Logging on IDE
The `Live Logging` feature on the SmartThings IDE is an essential tool in the debugging process of any issues that may be encountered.

To access the `Live Logging` feature, follow these steps:
- Go the SmartThings IDE (<https://graph.api.smartthings.com/>) and log in
- click `Live Logging`

### Installed SmartApps Info on IDE
The SmartThings IDE also provides helpful insights related to the current state of any SmartApp running on the system. To access this information, follow the follwing steps:
- Go the SmartThings IDE (<https://graph.api.smartthings.com/>) and log in
- Click `My Locations` (select your location if you have more than one)
- Scroll down and click `List SmartApps`
- Find the `Ecobee Suite Manager` SmartApp and click the link

### <a name="quicklinks">Quick Links</a>
- README.md (this file): <https://github.com/SANdood/Ecobee-Suite/blob/master/README.md>
- SmartThings IDE: <https://graph.api.smartthings.com>


----------------------------------------------

## <a name="reporting-issues">Reporting Issues</a>
All issues or feature requests should be submitted to the latest release thread on the SmartThings Community. For the major release version 1.3.0, please use this thread: https://community.smartthings.com/t/release-free-ecobee-suite-version-1-3/118319

If you have complaints, please email them to me directly at either [storageanarchy@gmail.com](mailto://storageanarchy@gmail.com) or to @storageanarchy on the SmartThings Community.

------------------------------------------------------------------

## <a name="license">License<a/>

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at:
      http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

----------------------------------------------------

## <a name="integration">Integrating With Ecobee Suite Devices</a>
The Ecobee Suite's thermostat devices can be leveraged by other SmartApps and by WebCoRE automations in a variety of ways. The provided Ecobee Thermostat DTH implements the following SmartThings capability sets completely (click each capability for the SmartThings Developer Documentation describing each capability):

- [Actuator](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Actuator)
- Health Check
- [Motion Sensor](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Motion-Sensor)
- [Refresh](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Refresh)
- [Relative Humidity Measurement](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Relative-Humidity-Measurement)
- [Sensor](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Sensor)
- [Temperature Measurement](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Temperature-Measurement)
- [Thermostat](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Thermostat)
- [Thermostat Cooling Setpoint](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Thermostat-Cooling-Setpoint)
- [Thermostat Fan Mode](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Thermostat-Fan-Mode)
- [Thermostat Heating Setpoint](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Thermostat-Heating-Setpoint)
- [Thermostat Mode](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Thermostat-Mode)
- [Thermostat Operating State](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Thermostat-Operating-State)
- [Thermostat Setpoint](https://smartthings.developer.samsung.com/develop/api-ref/capabilities.html#Thermostat-Setpoint)

It is thus possible to subscribe to state changes associated with these capabilities, and to send commands to the devices using the standard command sets these capabilities define.

Because the current SmartThings defined capabilities do not fully expose ALL of the native capabilities of Ecobee thermostats, several additional attributes (states) and commands have been added. 

-----------------------------

### <a name="attributes">Supported Device Attributes (States)</a>
Here is the complete list of attributes (states) that the DTH maintains and exposes (items marked with asterisks are added, and/or have overloaded meanings from the SmartThings definitions):
    
    *apiConnected: full*
    *autoMode: true*
    *auxHeatMode: false*
    checkInterval: 960
    *coolAtSetpoint: 74.5*
    *coolDifferential: 0.5*
    coolingSetpoint: 74.5
    *coolingSetpointDisplay: 74.0*
    coolingSetpointMax: 92.0
    coolingSetpointMin: 65.0
    coolingSetpointRange: [65.0, 92.0]
    *coolMode: true*
    coolRange: (65..92)
    coolRangeHigh: 92.0
    coolRangeLow: 65.0
    *coolStages: 1*
    *currentProgram: Home*
    *currentProgramId: home*
    *currentProgramName: Hold: Home*
    *debugEventFromParent: setProgram(Home) for EcoTherm: Downstairs)**decimalPrecision: 1*
    *ecobeeConnected: true*
    *equipmentOperatingState: idle*
    *equipmentStatus: idle*
    *fanMinOnTime: 50*
    *hasBoiler: false*
    *hasDehumidifier: true*
    *hasElectric: false*
    *hasForcedAir: true*
    *hasHeatPump: false*
    *hasHumidifier: true*
    *heatAtSetpoint: 68.5*
    *heatCoolMinDelta: 4.0*
    *heatDifferential: 0.5*
    *heatingSetpoint: 68.5*
    *heatingSetpointDisplay: 69.0*
    heatingSetpointMax: 78.0
    heatingSetpointMin: 45.0
    heatingSetpointRange: [45.0, 78.0]
    *heatMode: true*
    *heatRange: (45..78)*
    heatRangeHigh: 78.0
    heatRangeLow: 45.0
    *heatStages: 2*
    *holdStatus: Hold ends today at 6:30pm*
    humidity: 41
    *humiditySetpoint: 42*
    *lastPoll: Succeeded*
    motion: inactive
    *newCoolSetpoint: 0.0*
    *newHeatSetpoint: 0.0*
    programsList: ["Away", "Home", "Sleep", "Awake"]
    *scheduledProgram: Away*
    *scheduledProgramId: away*
    *scheduledProgramName: Away*
    *setpointDisplay: 68.0°*
    *statHoldAction: nextPeriod*
    supportedThermostatFanModes: [on, auto, circulate, off]
    supportedThermostatModes: [off, heat, auto, cool]
    temperature: 68.7
    *temperatureDisplay: 68.7°*
    *temperatureScale: F*
    thermostatFanMode: auto
    *thermostatFanModeDisplay: circulate*
    *thermostatHold: hold*
    thermostatMode: heat
    thermostatOperatingState: idle
    *thermostatOperatingStateDisplay: idle*
    thermostatSetpoint: 69.0
    thermostatSetpointMax: 78.0
    thermostatSetpointRange: [null, 78.0]
    *thermostatStatus: Setpoint updating...*
    *thermostatTime: 2018-02-09 17:00:23*
    *timeOfDay: day*
    *weatherSymbol: 0*
    *weatherTemperature: 27.9*
    
---------------------------------------------------

### <a name="commands">Supported Commands</a>

The complete set of thermostat DTH commands that can be called programmatically is:

#### <a name="changeprogram">Changing Programs</a>
- **home** - change to the "Home" program
- **present** - change to the "Home" program (for compatibility with Nest smart thermostats)
- **asleep** - change to the "Sleep" program
- **night** - change to the "Sleep" program
- **away** - change to the "Away" program
- **wakeup** - change to the "Wakeup" program
- **awake** - change to the "Awake" program
- **resumeProgram** - return to the regularly scheduled program

Calling any of the above will initiate a Hold for the requested program for the currently specified duration (Permanent, Until I Change, 2 Hours, 4 Hours, etc.). If the duration is Until I Change (temporary) and the requested program is the same as the current program, resumeProgram is effected instead to return to the scheduled program.

#### <a name="changemode">Changing Thermostat Modes</a>
- **off** - turns off the thermostat/HVAC. Note, however, if the Fan Minimum On Time is not zero when this is called, the HVAC will turn off, but the fan will remain in circulate mode. Thus, to turn the HVAC *completely* off, you should first call **fanOff**, then **off**
- **auto** - puts HVAC into Auto mode, where either Heating or Cooling can be supplied, based upon the internal temperature and the heat/cool setpoint values
- **cool** - puts HVAC into Cooling (only) mode
- **heat** - puts HVAC into Heating (only) mode
- **emergencyHeat** - for HVAC systems supporting auxiliary heat (usually heat-pump systems), turns on  auxiliary heating
- **emergency** - ditto emergencyHeat
- **auxHeatOnly** - ditto emergencyHeat

#### <a name="changefan">Changing Fan Operating Modes</a>
- **fanOff** - turns the fan completely off. If Fan Minimum On Time is non-zero, it will be reset to zero
- **fanAuto** - sets the fan to operate "on-demand" whenever heating or cooling
- **fanCirculate** - same as fanAuto, except the Fan Minimum On Time is non-zero
- **fanOn** - runs the fan continuously

#### <a name="complex">Complex Commands Requiring Arguments</a>
The following command entry points require multiple arguments, and so are most optimally utilized by SmartApps that can construct the required arguments. Specifically, I have not heard of anyone successfully utilizing these from WebCoRE. If you want to try using them, I suggest you review the DTH code directly to understand the required argument structure.

- **setCoolingSetpoint** - change the cooling setpoint (will create a Temperature Hold); specify temperature in F or C, based on how you have configured the Ecobee Suite and Thermostat (both should agree)
- **setHeatingSetpoint** - ditto, but for heating setpoint
- **setThermostatProgram** - use to change to custom Programs/Climates other than the ones defined above
- **setFanMinOnTime** - used to set a custom number of minutes for the Fan Minimum On Time circulation mode
- **setVacationFanMinOnTime** - ditto, but for a vacation
- **scheduleVacation** - define a vacation programmatically 
- **deleteVacation** - delete a previously defined vacation
- **cancelVacation** - cancel the currently active vacation (if any)
