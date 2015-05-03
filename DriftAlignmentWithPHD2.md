# Polar Drift Alignment with PHD2 #

> <blockquote><font color='red'><b>Note: this document is mostly obsolete since the Drift Align Tool was added to PHD2. The Drift Align tool is described in the PHD2 help file and in <a href='http://openphdguiding.org/man/Tools.htm#Drift_Align'>the online help</a>. Although you can still follow these instructions, many of the steps have been automated with the Drift Align tool.</b></font></blockquote>

Here's my workflow for drift alignment with PHD2.

Prerequisites:
  * Works best when connected to a scope with ASCOM. With an ASCOM scope PHD can get the actual declination of the scope. Otherwise, it has to assume declination zero, and the calculations will be off a bit if you are not actually pointing at declination zero. You can still drift align perfectly well, but the calculations giving alignment error and number of pixels to move the guide star will be off.
  * Enter your guide scope focal length in Brain => Global tab
  * Enter your guide camera pixel size in Brain => Camera tab

Start with your scope roughly polar aligned. For example, use your mount's polar alignment scope if it has one. Otherwise do your best to get the polar axis of the mount pointing towards to pole.

# Adjust Azimuth #

Point scope near the intersection of the meridian and the equator (declination 0).

Select a star and calibrate PHD2. Make sure Dec Guide Mode is Auto so it calibrates both RA and Dec.

Open the Graph Window. Set Dec Guide Mode to "None". Enable Trendlines. Display RA/Dec on the graph (not dx/dy).

We are going to iterate between drifting and adjusting the mount's azimuth. The Dec trendline on the PHD2 graph will show which way the guide star is drifting. When perfectly aligned, the trend line will be flat.

The first iteration or two are for determining how the azimuth adjustment affects the Declination drift slope. Make note of how the slope changes when you adjust azimuth one way or the other. You are looking for whether adjusting azimuth one way pushes the slope up or down.

**Drift** Start Guiding. Clear the graph data. Let the guide star drift until the Dec slope stabilizes. For the first couple of iterations, don't worry about waiting for an accurate slope, just drift long enough so you can tell in which direction you need to correct. After the first couple iterations, drift long enough for the magenta Polar Alignment circle radius to stabilize.

**Adjust Azimuth** Press Stop, then press Loop. Adjust the mount's azimuth in the direction that will flatten the Dec slope trend line. If the magenta Polar Alignment circle is visible in the main window, move the guide star until it is on the magenta circle. If the Dec slope is too steep, as it will likely be for the first iteration or two, the Polar Alignment circle may be too big to be visible on the screen. In that case, use the alignment error amount (pixels) shown on the graph as a guide for how far you need to move the guide star.

Repeat **Drift** and **Adjust Azimuth** until you are satisfied with your azimuth alignment.

> Tip: you can keep track of the star's position in each iteration by marking the position with a Bookmark. Ctrl-click on the image to add or remove a bookmark. Press 'B' to toggle display of bookmarks on/off.


# Adjust Altitude #

Stop guiding and slew the scope to the East or West, close to the horizon. Keep the scope at zero (or close to zero) declination. No need to re-calibrate.

Repeat the **Drift**/**Adjust** cycle, but this time adjust the scope's altitude.

# Summary #
  * point scope to equator+meridian, adjust Azimuth
  * point scope to equator+horizon, adjust Altitude
  * **Drift**: Start Guiding, clear graph data, wait
  * **Adjust**: Stop Guiding, Loop Exposures, adjust alt/az to flatten Dec slope, move guide star to Polar Alignment error circle for one-shot correction.