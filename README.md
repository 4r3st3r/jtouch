Software-only capacitive touch for the Raspberry Pi Pico
=

Any GPIO pin can be used as a capacitive touch sensor.

This code uses a PIO program to detect the capacitance of a finger. First the pin is set to output and pulled low. Then it's set to input with the internal pull up enabled, and the PIO counts how long it takes to go high. There's a tiny difference in the timing of this count depending on whether there's a finger nearby, and by repeating the test thousands of times in a loop, that difference is amplified.

The PIO loops for a fixed length of time (about 16ms, for a 60Hz update rate), and returns a count of the number of charge / discharge cycles.

The Python program tracks the minimum and maximum counts, and turns the current count into a 0-1 level.

This is a fork of AncientJames' capacitive touch code, which has been reformatted for ease of use into a module to be more easily imported into any Pi Pico projects. 

capsense.py is the module, which can then be imported and used as: 

```python
# Import the module
from capsense import Device

# Define which GPIO pins are being used
capSensors = [0,] # include as many as needed / active

# Create a device with GPIO 0 as the touch sensor
with (Device(capSensors)) as touch:
    while True:
        # Update the capacitive touch sensor readings
        touch.update()
        
        # Read the level of the touch sensor (0.0 = no touch, 1.0 = full touch)
        level = touch.level(0)  # We are using channel 0 since we only have one pin, other pints available in the list if intialised
```

I have left AncientJames' original code in this git as an easy (and very impressive) standalone demo of this code. 

https://user-images.githubusercontent.com/14081099/221393030-e98d9114-3211-4b21-a260-b7a585bc5c02.mp4

# KNOWN ERRORS
- Currently getting the ```OSError: [Errno 12] ENOMEM``` error on the Pi Pico W every other run (or so)
  - This can be worked around by resetting the machine every time it happens with ```machine.reset()```
