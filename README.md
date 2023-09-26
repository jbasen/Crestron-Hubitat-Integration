# Crestron-Hubitat-Integration

Note - A mistake in the help for the Philips Hue module and the RGB-
RGBW module was reported to me.  In the help it states that the dim 
level and dim level fb fields are 0% to 100%.  This is incorrect.
The fields are actually 0d to 100d.  This will be fixed the next time
I update a new version but I wanted to let people know about the error.

Version 3.11 - Adds support for smoke and smoke/co detectors.

Version 3.9 - Adds support for Flair smart vents and pucks.  Note -  The
Flair system must be integrated with the hubitat using the following driver
https://www.ecomatiqhomes.com/hubitatstore

Version 3.8 - Add additional code for memory management to work around
class library memory leak. Added support for Tuya Zigbee Water Valve.
Fixed an issue with the shade module.

Version 3.7 - Added support for last update date/time strings to all modules
so updae times can be monitored. (User Request)

Version 3.6.2 - I haven't been able to find a solution to the problem
setting the Maker API App PostURL field on a 3 series processor.  So,
to make this as painless as possible the code now only tries to set
this field on a 4 series processor.  People using running the code on
a 3 series processor will have to manually set the PostURL field.  The
code checks if it is running on a 3 or 4 series processor and only tries
to set the PostURL field on a 4 series processor.  So, once a person
running the code on a 3 series processor manually sets the PostURL
field, they won't have to repeat that step again unless their
processor IP address changes or the port they wish their processor
to listen for messages changes.  See step 15 below for information
on setting this field.

Version 3.6.1 - It turns out the PostURL bug was only fixed on a 4
Series Crestron processor.  The code now differentiates whether it is
running on a 3 series or 4 series processor to create the proper get
request to set the PostURL field in the Maker API settings on the 
Hubitat

Version 3.6 fixes several bugs introduced by changes made by Crestron
and Hubitat.  Refresh now properly updates module feedback fields and
the PostURL field in the MakerAPI is now properly set again.

New version 3.4 adds outputs to all modules of the device label defined
in Hubitat.  This (hopefully) fulfills a user request

New version 3.1 adds support for double-tap and press-hold events for 
switches and dimmers.  Note - not all switches and dimmers support these
events.  This was tested with Zooz devices by user ZippoD

Also new in version 3.1 is that the multi-sensor module now supports all
the sensors in the Aeotec MultiSensor 6
____________________________________________________________________________

New version 2 adds improved support for devices that don't properly
update feedback status after a command is sent to them.  There are now 
inputs on modules to trigger a refresh and a parameter that will automatically
trigger a refresh any time a command is sent to the hubitat.  To minimize 
traffic to the hub this parameter should be set to no unless needed for a
specific device

____________________________________________________________________________

The Crestron-Hubitat integration is a suite of Crestron modules that provide 
a way to integrate off-the-shelf Zigbee, Z-Wave, and IoT devices into a 
Crestron automation system using a Hubitat Elevation hub.  The Crestron 
modules interface with devices connected to the Hubitat hub through the 
Maker API app on the Hubitat.  The following types of devices are supported:

•	Buttons
•	Contact Sensors
•	Dimmers
•	Fan Controllers
•	Garage Door Controllers
•	Humidity Sensors
•	Light Sensors
•	Locks
•	Moisture Sensors
•	Motion Detectors
•	Phillips Hue Bulbs (White Ambiance and Color)
•	Presence Sensors
•	RGB and RGBW Bulbs
•	Shades
•	Switches / Outlets
•	Temperature Sensors
•	Thermostats

This module suite is released as shareware.  It is free for a developer to 
use on their own personal Crestron system or for a dealer to use in their 
showroom demo system.  However, if it is used on customer systems where a 
dealer will profit from it than I ask for a single payment of $100 from the 
dealer.  

What you get is 

1) My thanks
2) Permission for the dealer to use the module on as many client projects 
as they want
3) A copy of the Simpl# source code for the module (only a binary executable 
is included with the example program)
4) Good Karma

Payment can be made over Paypal to jay.m.basen@gmail.com

Thanks

A Crestron demo program is provided with modules to support all the above 
hardware.  Full S# source code is also provided.

A full article on the module suite can be found in Residential Tech Today 
Magazine can be found here: Coming Soon!

The following are the steps to install the system.  I use a switch, as an 
example, in the steps below, but it could be any of the above device types 
that can be integrated with a Hubitat Elevation Hub.

1.	First make sure that both the Crestron processor and the Hubitat 
Elevation hub have either static IP addresses or DHCP reservations so 
their IP addresses won’t change over time.  This is necessary for messages 
to reliably pass back and forth between the two systems.

2.	Assuming you have already registered your Hubitat Elevation Hub, 
connect to the IP address of the Hubitat hub using a browser and login 
using your username and password.

3.	Following the manufactures directions, place your Z-Wave, or Zigbee, 
switch into acquire mode.

4.	In your browser select “Devices” from the Hubitat menu and then click 
on the “Discover Devices” button. 

5.	Next click on either the “Z-Wave” or “Zigbee” button depending on the 
type of switch you are working with.

6.	Once the device is discovered you can enter a name for the switch and 
click on the “Save” button.

7.	Next click on “Apps” from the Hubitat menu.

8.	Click on the “Add Built-In App” button and select “Maker API” from the 
list of built in apps.

9.	In the Maker API configuration screen and click on “Select Devices” to 
display a list of devices that are connected to your Hubitat hub.  Then 
check the check box next to the switch you earlier connected to the Hubitat 
and click the “Update” button.  This is very important.  You must click the 
update button or the device will look like it is selected but it will NOT 
work properly with the Maker API app.

10.	Next scroll down to the bottom of the Maker API configuration screen.  
There you will see a series of http commands.  Each command begins with: 
http://<IP Address of Hubitat hub>/apps/api/XX where XX is the ID of the 
Maker App.  Save this number for later.  Also notice that each command 
includes access_token=xxxx-xxxx-xxxx-xxxx-xxxx.  Save the access token 
for later.

11.	Next right mouse button click on the “Get All Devices” link and select 
to open the link in a new browser tab.  In the new browser tab you will see 
the json returned by the Hubitat hub.  This json includes data for all the 
devices attached to your hub.  Find the entry for the switch you just added 
and copy down the “id” number of the switch.  If you have a number of 
devices connected to your hub you may find it easier to read the json if you 
use one of the many json formatters on the internet

12.	Finally, go back to the browser tab with the Hubitat hub user interface 
and press the “Done” button to close the Maker API app configuration page.

13.	Next using whatever procedure you are comfortable with, copy the .usp 
and .ush files from the Hubitat Demo.zip file to your Crestron Usrsplus 
directory.  Each of the .usp files has a # INCLUDEPATH statement that 
defines where the Hubitat.clz file will be located.  You need to change 
the include path to the location where this file will be located on your 
own computer.  I have experienced compile issues when the .clz file is placed 
in the Crestron Usrsplus directory so unfortunately this editing step is 
necessary.  After editing each .usp file you then just need to recompile 
them.  Don’t forget to copy the hubitat.clz file to the directory you 
specified in the INCLUDEPATH.

14.	After you have studied the code in the Hubitat Demo program you just 
need to
a.	Insert a “Hubitat Comm Manager” module into your own program.
b.	Fill the parameter fields on the Comm Manager module in with
i.	The IP address of the Hubitat Hub
ii.	The IP address of the Crestron Processor
iii.	The communications port you want to use for your Crstron processor to
  receive messages
iv.	The access token you copied down in step 10
v.	The Maker API app ID you copied down in step 10
c.	Insert a “Hubitat Switch” module into your program and fill in the 
Device ID parameter field with the switch’s device ID that you copied 
down in step 11.
d.	Now connect the various signals on the two modules in the same way 
you see them connected in the demo program
e.	You are done

15. If you are running this code on a 3 series Crestron processor you
need to perform this step.  It isn't necessary if the code is running
on a 4 series processor.  In the Hubitat Maker API app seetings page 
scroll down until you see the field "URL to POST device events to".
Enter the following in that field:
http://[Crestron Processor IP Address]:[Communications Port]
The communications port is the one specified in the Crestron_Processor_Port
parameter field of the Hubitat Comm Manager module.  So, if the
processor IP address is 192.168.0.10 and the port is set to 9000 then 
the following should be entered in the "URL to POST device events to"
field:
http://192.168.0.10:9000
If this field is not properly set then the Crestron system will not
reflect feedback of the state of devices connected to the Hubitat
Elevation Hub.
There are also Crestron modules to support the ability to have IFTTT 
applets trigger a Crestron program or provide numerical data to a Crestron 
program.  This complements the IFTTT module on this Github and removes the 
necessity of adding a port forward on a home’s router to route IFTTT 
webhook messages to the Crestron processor.  Instead they are routed through 
the Hubitat cloud, to the Hubitat hub, and finally to the Crestron processor.  

