# THIS IS NOT THE PHD2 MANUAL, JUST A WORKING DOCUMENT THAT IS NOW OBSOLETE #
## If you are looking for the PHD2 Manual, use the Help menu in PHD2, or [view the official manual online](http://openphdguiding.org/man/) ##
# NOT THE PHD2 MANUAL #

 to get started I have ported the existing help file's content below. Over time I/we will massage it into a version that is up-to-date w.r.t. v2. 

## Dave's To-Do List ##

things to document, in no particular order:

  * Calibration Step Size Calculator

  * AO

  * Auto-Restore Calibration switch

  * Fast Recenter After Calibration switch

  * Dark frames: how to keep darks for multiple exposure times, and how Auto-Load Dark Frames works

  * Trendlines display

  * Corrections display

  * interoperability with Sequence Generator Pro

  * Sticky Lock Position (I have no idea what this is!!!)

  * Equipment Profiles and how they work

  * Polar Drift Align tool
  * calibration declination compensation
  * log files directory selection
  * bookmarks
  * ascom devices connect directly without going through the ASCOM chooser
  * simulator
  * connect equipment dialog (replaces scope and mount menus in PHD1)
  * i18n - how to select language (French translation available)
  * how to dock, un-dock and resize windows
  * How to display and graph of guide error in units of arc-seconds or pixels
  * target display

# Introduction #

Welcome to PHD Guiding! v 1.11
PHD Guiding is designed to be "Push Here Dummy" simple, yet provide powerful, intelligent auto-guiding of your telescope. Connect your mount, your camera, select a star, and start guiding. That's it!

Despite actually having a Ph.D., I've always had a tough time figuring out which way North is in the guide frame, whether an axis is mirrored (and if so, which one), how that's affected by camera rotation, how many arcseconds per pixel I'm actually running at (especially since a "2x" barlow isn't always 2x), etc. This is especially tough when standing out in a cold field late at night when all you really want to do is stay warm and collect good images of your DSO.

In PHD Guiding, all calibration is taken care of automatically. You do not need to tell it anything about the orientation of your camera, nor do you need to tell it anything about the image scale. The automatic calibration routine takes care of this for you. Odds are you won't ever need to set a single parameter. Just select your star and hit "PHD Guide" and let the software take care of it.

Copyright © 2006-2009, Craig Stark, Stark Labs


# Main Screen #
PHD Guiding is designed to be easy to use and to take the frustration out of auto-guiding. The main screen looks like this.

The majority of the screen is taken up by the display of your starfield. No matter what sized image your guide camera captures, it will be resized to fit into this window for display purposes only. Likewise, no matter what exposure duration you use, the image will be automatically stretched to show you viable stars for display purposes only. Internally, all spatial and intensity resolution are not altered. The view shown is designed to show you your whole star field and to show you possible guide stars but internally, PHD Guiding uses the raw data off of your guide camera to maximize guiding accuracy.
## Basic controls ##
Near the bottom of the screen are the main controls. PHD Guiding controlled largely by five buttons and a pull-down list. The five buttons are:
1. Camera: Connects to your guide camera
2. Scope: Connects to your telescope mount via whatever method selected in the Mount menu
3. Loop: Begins looping exposures to let you focus and find a guide star
4. Guide: Begin guiding (calibrating if needed)
5. Stop: Stops looping exposures or stops guiding.
Next to this, there is a pull-down list of exposure durations (0.05s - 10s). This lets you quickly and easily set the guide camera's exposure duration. Note, that if your camera does not support a duration, PHD Guiding will do its best to emulate that duration. For example, if you use a short-exposure webcam your maximum true exposure duration may be 1/30th of a second or so. If you select "1 s" from the exposure duration, PHD Guiding will automatically acquire images for one second and stack them on the fly, using the resulting stack as the guide frame.
Next up is a slider here. PHD Guiding auto-stretches and auto-scales the display for you to take the hassle out of this. But, sometimes the display is just a bit too bright or too dim. The slider changes the way the stretch behaves (it changes the gamma curve) to let you dial it in to your liking.
Moving on, we have one other button with an icon - a brain here. This button brings up the Advanced Panel, where you can have a bit more control over PHD Guiding's operations. Next up is a button labeled "Take Dark" in case you wish to use dark subtraction (usually not needed). Finally, if connected to a WDM-style or VFW-style webcam, you'll have a button available to bring up that camera's built-in dialog (that lets you set the camera's exposure duration and other parameters.)
## Menu ##
The most notable entry in the Menu bar is the Mount menu. Here you can select the way you want PHD Guiding to talk to your mount. Your choices here are described in the Telescope Mounts section of the manual. When you press the Scope button, PHD Guiding will attempt to connect to whatever kind of interface you've selected here.
In addition, there is also a Tools menu with various things that may make your life easier. These are all described later in the Tools and Utilities section.
## Status Bar ##
Below the buttons is the Status Bar. The left panels of the Status Bar display messages to the user and will keep you aprised of guiding status. The right panels of the Status Bar indicate whether a camera is connected, a scope is connected, and whether calibration information is known.

# Using PHD Guiding #
There are six basic steps to start guiding.
1. Press the Camera Button and select your camera
2. Press the Scope Button and select your telescope mount
3. Pick an exposure duration from the drop-down list
4. Hit the Loop Button and adjust your focus. Move the mount or adjust the exposure duration as needed to find a suitable guide star. Then, optionally, hit Stop.
5. Click on a star that's not very near an edge. After you do this, you'll see a green box appear surrounding the star. Note, if you pick a star that is too bright, a dialog will appear telling you the star is "saturated" and that odds are you should either use another star or decrease the exposure duration to keep it from being so over-exposed.
6. Press the PHD Guide button.

## Automatic Calibration ##
When you do that last step, the calibration process will begin. Yellow cross-hairs will appear over the original location of your guide star and PHD Guiding will start to move the mount in various directions, tracking how the star moves as a function of what move commands were sent to the mount.
Once the calibration is done, the cross-hairs will turn green and guiding will begin automatically. To stop guiding (e.g., if you want to move your mount to another target), simply hit the Stop button. When you're ready to start guiding again, select a star (Loop again if necessary) and hit the PHD Guide button. The calibration will not be re-run unless you request (see Advanced Panel).


## The Advanced Panel ##
The advanced panel has a number of controls to help you fine-tune PHD Guiding's operation.

• RA Aggressiveness (100% default): On each frame, PHD Guiding computes how far it thinks the mount should move and in what direction(s) it should move. The aggressivness parameter scales this. If you find your mount is always overshooting the star, decrease this value slightly (say, by 10% steps). If you find PHD Guiding always seems to need to catch up and is lagging behind the star's motion, increase this by a little bit. A little can go a long way here.

• RA Hysteresis (10% default): The point of auto-guiding the mount is not to remove rapid changes associated with poor seeing, but to remove your mount's inherent errors and errors resulting from poor balance, etc. The amount of error your mount has at any time is likely to be pretty similar to the amount of error it had a second or so ago on your last frame. By including a bit of "hysteresis", the past history of the errors is blended into the current estimate of the error, to smooth out the guiding commands. Increasing this will keep PHD Guiding from reacting as quickly to spikes and will smooth out the guide commands.

• Dec guide: There are four settings available here. Off (RA-only guiding), Auto (try to guide out slow dec drift automatically), North (only send North guide commands), and South (only send South guide commands).

• Dec algorithm: There are two settings here that control how the declination guide commands are computed. Declination guiding is not like RA guiding as the errors are not caused by imperfections in your mount's gears but by being imperfectly aligned to the pole. The result is an error that should be smooth and should be in only one direction. Depending on you mount and your alignment, the best way to do this varies and PHD Guiding gives two options. In one, "Lowpass filter", the Declination guiding is smoothed over time. It works well if you are close to the pole and need only a smooth guide adjustment. In the other "Resist switching", normal guide commands are sent and the guiding is more reactive, but PHD still attempts to remain on the same side of the worm gear and to not switch directions unless absolutely necessary.

• Max Dec duration (150 ms default): This value will limit the declination guide command to a maximum value. This is useful to prevent over-shooting and oscillation that can occur, especially when PHD Guiding decides to switch direction in one of the auto-Dec modes.

• Dec slope weight (5.0 default): This parameter is only used if the declination guide mode is set to use the automatic, "lowpass filter". The point of this is to fix an issue in which, with a significant enough declination drift rate, the lowpass filter mode could not keep up with the drift. The math says the value should be 5. Setting it to 0 will restore the old behavior and setting it much higher could lead to over-correcting the declination error.

• Calibration step (750 ms default): This specifies how long a guide pulse is sent during each step of the calibration process. 750 ms is a good default, but if you are using a very long (>>2000 mm) focal length guide setup, you may find you need to reduce this number to keep PHD Guiding from losing the guide star during calibration (e.g., 250 ms). In contrast, you can speed up the calibration process if you're using a short focal length (e.g. 400 mm) guide setup by increasing this (e.g. to 1000 ms).

• Min motion (0.25 pixel default): How many pixels must the star move before PHD will send a guide command?

• Search region (15 pixel default): How big a box should be searched in an effort to find the star?

• Noise reduction: Should the image be smoothed to remove noise and hot pixels? Choices include None, 2x2 mean, and 3x3 median. Both 2x2 mean and 3x3 median will reduce the noise considerably. 3x3 median is especially effective at removing hot pixels and neither will significantly affect guiding accuracy.

• Time lapse (0 ms default): At times, you may want to use shorter exposure durations but wait between guide commands. For example, if you are using a Mintron camera that lets you integrate 2s of exposures onboard, you might want to set the exposure duration to 0.05s (to just take one or two video frames), then set the time lapse to 2000 ms. Guide commands would be sent once every 2s this way and only one or two guide frames would be captured (these cameras send the same frame over and over until a new "long exposure" frame is available).

• Camera gain (95% default- currently disabled): Some cameras (but not webcams) let you directly set their "gain boost" here. If you really want to use a bright star and a longer exposure duration or if your camera makes a very noisy image, reduce this.

• LE Port: If you are using a parallel-port based long-exposure webcam, you need to indicate which parallel port to send the commands to. If you don't know, guess (or snoop around the Device Manager for the Resources tab of your Parallel port).

• LE Delay: If you are using a long-exposure modified webcam there is a magic "delay" value that lets you grab the correct video frame (the long-exposure one) out of the stream of blank or short-exposure frames that come on before and after this long-exposure frame. Typical values are about 10-20 ms, but this will vary from system to system.

• Force calibration: When checked, PHD Guiding will perform the calibration the next time the Guide button is pressed. This is useful if you move to another section of the sky and want to re-calibrate PHD.

• Use subframes: Certain cameras (currently the Atik and SBIG cameras) are setup to allow partial downloads from the camera (subframes). This is extremely useful on slow interfaces (e.g. parallel ports and USB1.1) and can mean the difference between the camera being too slow to effectively guide and being quite responseive. When this is enabled, only a small subframe (100x100 pixels) will be downloaded once a star is selected. During initial looping without a selected star, the full frame is downloaded, but once a star is selected, only this small subframe is downloaded. To return to the full-frame (e.g., to select a new star), Shift-click in the main display window.

• Log info : It is sometimes useful to know just what PHD Guiding did as it tried to guide on your star. Selecting this will log all guide info to a file "PHD\_log.txt" for you to examine, import into a spreadsheet, etc. This is a comma-separated text file ("CSV" format").

• Disable guide output: Selecting this will turn off the actual guide commands. Obviously, you do not want this on by default. It is useful, however, if you wish to examine the unguided periodic error (PE) of your mount.

## Tools and Utilities ##
### Dark frames ###
While the Noise reduction techniques work for many, some people will benefit significantly from using dark frames. The process is quite simple. Begin by selecting the exposure duration you plan on using during guiding. Then press the Take Dark button in the main window. You'll be instructed to cover the telescope and press OK. PHD will take 5 dark frames, average these, and subtract this from all the subsequent light frames.
If you need to redo your dark frame for any reason, simply pull down Erase dark from the Tools menu.
### Manual guiding / mount testing ###
Want to know if these signals from PHD are actually getting to your mount? Or, need to manually guide your mount or nudge it? In the Tools menu, pull down Manual Guide and a dialog will appear that will let you move the mount at guide speed in any direction. Each time you press the button, a pulse of the same duration specified in the Calibration step size will be sent. Listen to (rather than watch) your mount to determine if the mount is getting the commands from PHD. The idea here is just to figure out if the mount is responding to PHD's signals. You won't be able to see the mount move (it's moving at guide speed) but you may be able to hear it. Other options include watching the motors themselves and attaching a laser pointer to your scope and aiming it at something fairly far away (to amplify your motions).
### Crosshairs and Overlays ###
PHD lets you superimpose a bulls-eye, a fine grid, or a coarse grid on the image. This can help in placing the guide star in a consistent position across sessions and in polar alignment. Select what kind of overlay you wish in the Tools menu.
### Logging ###
PHD lets you save data from your guiding session into a log file. When you Enable logging a comma-separated text file (CSV) will be saved in your "Documents" or "My Documents" folder with detailed information about your guiding session. This is suitable for importing into any spreadsheet program or the PHD Log Analyzer for analysis.
The log file's units are all in pixels (distance), seconds (time), and radians (angles). PHD Guiding never determines the true arcesconds per pixel in your image (it doesn't need this). As of version 1.10, the star's "mass" is now logged as well. This is a measure of how bright the star is and can be useful for determining when things like clouds caused the star to be lost.
When trying to determine what went wrong in a guiding session, it can be useful to have a record of all of the guide frames. If you Enable Star Image logging a JPEG image will be saved of a small area around the star for each guide frame. Each image will take up about 4k of disk space.
In addition to logging this data, PHD can save debugging information that can let me diagnose things like intermittent lockups. In general, there is no need for you to Enable debug logging, however.
### Graphing / Plotting performance ###
While the log-file feature lets you evaluate guiding performance offline, it can be useful to look at performance during the session (e.g., to examine drift). When you select Enable graph from the Tools menu, a dialog will appear and float above the main PHD window. Here, you will be able to see the error (y-axis) plotted over time (x-axis). The grid is spaced at 1-pixel increments on the y-axis and 25 time points on the x-axis.
With the buttons on the left you can change the number of time points shown (50, 100, 250, or 500), toggle between plotting RA and Declination or "dx" and "dy" (motions in x and y respectively), or hide the plot. When hidden, the data in the graph will still update, so you will always have access to the most recent (up to 500) samples.
In the lower-left of the graph, an "oscilliation index" is shown. This is the result of calculating (in the current window's worth of data), the odds that the current RA move is in the opposite direction as the last RA move. If you are too aggressive in your guiding and over-shooting the mark each time, this number will head towards 1.0. If you were perfect and not over- or under-shooting and your mount had no periodic error, the score would be 0.5. Perfect with periodic error and the score may be closer to 0.3. If this score gets very low (e.g., 0.1), you may want to increase the RA aggressiveness (and/or decrease the hysteresis). If it gets quite high (e.g., 0.8), you may want to decrease the RA aggressiveness (and/or increase the hysteresis).
In addition to graphing the plotting performance, you can Enable Star Profile to get a real-time plot of the star's profile. You can use this to see how clean an image you're working with. When the small window pops up, you will see the shape of the star as taken through the middle (left-right through the middle). Clicking in the window will let you see the profile in other orientations (middle column, average row, and average column).
### Server ###
PHD Guiding has the ability to communicate with Nebulosity, Stark Labs' image capture and processing program. To do this, PHD Guiding runs a "server" that is not on by default. Once running, the server enables Nebulosity to pause guiding during the download of images from the main camera (some cameras show an increase in noise if the USB bus is loaded during download) and to dither the position of the telescope. By moving the telescope slightly between images on your main camera, any noise that is in a consistent position on the image is no longer in the same place in the sky, allowing for a lower-noise final image after stacking.
### Manual calibration ###
Unless you have a very good reason to look at doing this (and I can hardly thing of many reasons why you should), don't look at using this. But, if you do, this entry in the Tools menu (or Ctrl-M) will let you manually enter calibration data. The entries from one session can be found in the log file and entered here to re-establish previous calibration data.

## Supported Telescope Mounts ##
PHD Guiding supports autoguiding on a range of telescope mounts via several methods:
1.	The ASCOM platform's PulseGuide commands (Windows)
2.	The ShoeString Astronomy GPUSB guide port adapter (USB-based: Windows and OS X)
3.	The ShoeString Astronomy GPINT guide port adapter (parallel-port based: Windows)
4.	Camera's onboard ST-4 output (Windows & OS X)
5.	AstroGene's GC USB ST4 (ASCOM on Windows, direct on OS X)
6.	Mounts connected to MicroProjects Equinox 6 (OS X)
### ASCOM ###
Guide commands can be sent to your mount using the ASCOM platform. Specifically, PHD Guiding uses the PulseGuide set of commands to guide the mount. This set of commands gives excellent control over the mount without the delays or lags associated with older ways of controlling the mount through commands sent over your serial port. Mounts from Meade, Celestron, Takahashi, Losmandy, Astro Physics, Software Bisque, Vixen and more are supported this way. ASCOM must be downloaded and installed prior to using PHD Guiding.
To download ASCOM, visit the ASCOM Standards Download Page (http://www.ascom-standards.org) and download the current Platform.
Please note, that Meade Autostar 497 mounts (LXD-55, LXD-75) require a patched ASCOM driver for best performance (available on the Stark Labs website. Without this, they will not run in a real PulseGuide mode.
### ShoeString GPUSB ###
PHD Guiding also supports the ShoeString GPUSB Guide Adapter (http://www.shoestringastronomy.com). This adapter lets you connect your mount's ST-4 style "guider input" port to your computer's USB port. Currently, support for this adapter is provided in two ways. You can use this via ASCOM (an ASCOM driver for the GPUSB adapter is available). To use it this way, simply select ASCOM as your mount interface in PHD Guiding's Mount menu and select the GPUSB as your ASCOM "telescope". You can also use this directly without ASCOM. To do so, simply select "ShoeString GPUSB" from the Mount menu. Note, this direct connection to the GPUSB is supported on the Mac as well.
### ShoeString GPINT ###
PHD Guiding will also support the parallel-port version of the ShoeString adapter. To use this adapter, you must know the address of your parallel port. You can use the utility provided by ShoeString to help you find this or you can enter the Device Manager in Windows, and examine the Properties page for the parallel port (often called the ECP port) and look in the Resources tab. Valid addresses are 03BC, 0378 and 0278. To use the adapter, select the appropriate GPINT version in the Mount menu.
### Onboard ST-4 ###
Several guide cameras include an ST-4 style port on the camera itself. This plugs directly into your mount's ST-4 "autoguide input" port and means you do not need another adapter. If your camera has one and you wish to use it, select "Onboard camera" from the Mount menu.

## Supported Cameras ##
PHD Guiding can use a number of popular cameras to guide your mount. Unless noted, the cameras are supported only under Windows.
1. ASCOM v5 compliant cameras
2. Atik 16-series and "Generation 3" series cams (e.g., 314, 4000, etc.)
3. CCD Labs Q-Guide
4. Fishcamp Starfish: Windows & OS X
5. Opticstar PL-130 / PL-130C
6. Orion Guider
7. Orion StarShoot DSCI
8. MagZero MZ-5
9. Meade DSI series (color, Pro, II, II-Pro, III, & III-Pro): Windows & OS X
10. SAC 4-2
11. SBIGs (any, but see below): Windows & OS X
12. The Imaging Source / DCAM Firewire: Windows & OS X (see below)
13. Webcam (short-exposure) "WDM / DirectX 8+" (e.g., Meade LPI, Celestron NexImage, ToUCam, Vesta, Orion StarShoot Solar System, etc.)
14. Webcam (short-exposure) "VFW" (e.g., SAC8, older video capture cards or webcams, etc.)
15. Long-exposure webcam with ShoeString LXUSB adapter
16. Long-exposure webcam via Parallel port
### SBIG support ###
Virtually any SBIG camera can be used. However, there are a few caveats to be aware of. If your camera has two sensors and one is used in PHD Guiding, the other will not be accessible from another program. In addition, PHD Guiding downloads full-frames by default. While this is not an issue for USB cameras, the slow speed of the parallel port makes this a problem for parallel port cameras. Users of these cameras (or even many USB 1.1 cameras) will want to enable "Use subframes" in the Advanced menu.
### The Imaging Source ###
On Windows, most of the DCAM-compliant (Firewire) cameras from The Imaging Source are supported in both a direct mode and via their WDM drivers. The exception to this is are the "21F" series cameras that are limited to only 1/30s exposures. These are supported only via the WDM drivers (note the 21AF, 21BF, etc. do not have this limitation). In addition, the USB cameras are supported only by the WDM drivers. When directly connecting to these cameras, the main exposure pull-down will set the camera's exposure. Gain is controlled in the Advanced dialog as with other cameras.
If using the WDM driver, things are slightly more complicated. WDM drivers do not let a program like PHD set the exposure duration parameter directly. Thus, if you have the pull-down set for 1s, but the camera set to 0.1s in the Camera Setup dialog (this a dialog provided by the camera driver), PHD will attempt to stack as many 0.1s frames as it can get in 1s. Thus, to ensure the camera's long-exposures are used, set the exposure in the camera's dialog and set PHD's exposure to something less than or equal to the exposure set in the dialog.

On OS X, the direct-connect mode is provided.