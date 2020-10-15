# Thermal Scanner for Arduino

This code is an attempt build a thermal scanner suitable for scanning walls or other subjects that are assumed to be flat. The intended use is trying to find damp or air spots in walls and ceilings.

The question I am trying to answer is - for a static object with a similar temperature profile over time, can we take many measurements with a small (8x8) IR array and use position information to build an image of similar quality as would be achieved with expensive scanners.

# Part List

- Microcontroller (using Arduino Uno, may need Teensy to complete)
- AMG8833 8x8 thermal camera.
- ISM330DHCX IMU (Gyro + accelerometer, should be possible to use other IMUs).
- TFT screen + SD card writer (optional).

# Scan process

- Take constant readings from IMU, using Kalman filter to attempt to determine displacement. This should give us a (noisy) read of displacement from start of scan as a 3D vector.
- Take constant readings from AMG8833. This can read at 10Hz so is likely to be the limiting factor. 
- This will then build up an array of data of the form (x, y, z) + (x_rot, y_rot, z_rot) -> (8x8 IR reads).
- In order for the measurements to be usable, the scan needs to take place in a 2D plane that is paralell to the 2D plane you are trying to scan. Ideally for each measurement, there would be no rotation action and the (x, y, z) readings would be embedded in that plane. You therefore would get a new mapping (z, w) -> (8x8 reads). You can then define a distance that each array represents to build the full picture.
