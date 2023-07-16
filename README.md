# Solar Tracker

Author: John Culley

Date: 2023-02-07

### Solution Design

There are four major components to the code functionality: i2c display, servos, voltmeter, and timer. Each component was constructed with their functionality contained within functions with an efficiently laid out sequence. The first function of the code is to read the voltage, which is contained within the 'read_voltage' function. This function uses the adc port of the ESP32 to read the raw data which is then transferred to mV. The voltage is what is returned from the function. The voltage function is then used extensively within the 'find_azm_alt' function. The 'find_azm_alt' function needs predefined variables to store the previous and current state of voltage, the current azmuth and altitude values, the max voltage, and azmuth and altitude step and range values which will be passed to the servos function. Within the 'find_azm_alt' function it begins by checking if the servo needs to do a wide scan of the entire range. If this proves to be true, the function performs a binary check of the voltages by first -90 to 90 for azmuth and 0 to 90 for altitude and storing the maximum voltage within the range along with the degrees at which the maximum voltage was acquired. After the maximum voltage was acquired, at the end of the function it is stored in the 'prev_volt' variable. Upon reentry to the function, a check is performed to determine whether the servos need to check the entire range or perform a small shift. If 'prev_volt' is greater than the measured voltage plus 300, then another complete range check needs to be performed. Otherwise, a small shift can be performed. This check allows for the solar tracker to only do a complete range check if the solar input has changed substantially, and if it hasn't moved substantially then a small shift follows the solar input. The 'rotate_servo' function is called within the 'find_azm_alt' function with a servo number parameter and a range parameter. This allows the function to be called for a specific servo and range allowing it to be used flexibly within the code. The rotate_servo function uses the preinitalized comparators and the output of the example_angle-to_compare function to check the physical servo location vs the desired angle. The example_angle_to-compare function checks if the given angle is within the allowed minimum and maximum along with inside the needed pulsewidth. The hardware timer was set to a period of 20s which allowed for all of the functions to complete in their entirety before starting another cycle. We use connect two servo motors to GPIO pins 12 and 33 and connect the display to the I2C pins. Then we use ADC channel 6 (GPIO pin 34) to read the voltage of the solar panel.

### Sketches/Diagrams
<p align="center">
<img width="254" alt="image" src="https://user-images.githubusercontent.com/113144839/217354289-5bb91f5f-3c35-4cb3-b399-be2358f6df6b.png">
</p>
<p align="center">
The flowchart shows the algorithm design and project flow through its multiple states and the timer period of 20s.
</p>

### Supporting Artifacts
- [Project Demonstration](https://drive.google.com/file/d/1Bk7z9dyCVurmIdYeYjn3OUVca8PROR9R/view?usp=sharing).


### Modules, Tools, Source Used Including Attribution
ESP-IDF Example Code, 
ESP32, 
Visual Studio Code, 
i2c display, 
SG90 servos, 
Solar panel

### References
ESP-IDF Source Code

