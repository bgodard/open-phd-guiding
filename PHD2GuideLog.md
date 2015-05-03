# PHD2 Guide Log Format #

The PHD2 Guide Log records information about each camera frame while calibrating or guiding.

Each time calibration or guiding starts a new section is added to the log. Each section consists of a summary of calibration or guiding settings, a column label line, and lines for the camera frames.

The lines for the camera frames are in comma-separated value (CSV) format.

There may be informational lines interspersed with the CSV lines indicating events that affect guiding, such as guiding parameter changes. These lines all start with `INFO:`.

All distances and positions are in units of guide camera pixels.

# Calibration section #

The calibration section columns are:

| **Column** | **Description** |
|:-----------|:----------------|
| Direction | calibration step direction |
| Step | step number |
| dx | x-axis distance from starting point |
| dy | y-axis distance from starting point |
| x | x position |
| y | y position |
| Dist | distance from starting position |

# Guiding section #

The guiding section columns are:

| **Column** | **Description** |
|:-----------|:----------------|
| Frame | Frame number |
| Time | elapsed time from start of guiding |
| mount | guide correction device "Mount" or "AO", or "DROP" if camera frame was rejected |
| dx | X distance from lock position |
| dy | Y distance from lock position |
| RARawDistance | distance from lock position on Dec axis |
| DECRawDistance | distance from lock position on RA axis |
| RAGuideDistance | effective RA distance computed by the RA guide algorithm |
| DECGuideDistance | effective Dec distance computed by the Dec guide algorithm |
| RADuration | RA guide pulse duration (ms) |
| RADirection | RA pulse direction E or W |
| DECDuration | Dec guide pulse duration (ms) |
| DECDirection | Dec pulse direction N or S |
| XStep | AO X-axis step |
| YStep | AO Y-axis step |
| StarMass | Star mass value |
| SNR | Star Signal to Noise Ratio value |
| ErrorCode | Error code value for this frame (0 if no error) |
| ErrorDescription | description of the error, if any |