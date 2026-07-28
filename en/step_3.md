# Writing your program

We recommend writing your Python program in small, manageable steps rather than trying to build everything at once.

By following the sections below, you will create the core parts of your Mission Space Lab submission and test each part as you go.

## 1. Create your main.py file

Every submission must include a file called `main.py`. This file acts as the starting point for your program, and it is the file that the automated system on the ISS will look for and run. 

You can include additional Python files in your submission, but your program must begin execution from `main.py`. If you are new to Python or have not worked with multiple files before, we recommend keeping all of your code in `main.py`. This will make your program easier to develop, test, and submit.

--- task ---

Create a new file called `main.py` in your project folder and save it.

--- /task ---

## 2. Capture sensor data

Your program must use at least one of the Astro Pi's sensors or the camera to capture data. Using external libraries to calculate or predict values does not count as collecting sensor or camera data. For example, a program that only uses the `skyfield` library to determine the ISS's position would not meet this requirement, because it is using a model rather than measurements from the Astro Pi's hardware.

### Taking measurements with the Sense HAT

One way to collect data on the ISS is to use the sensors on the Sense HAT. These sensors can measure things such as light levels, colour, temperature, humidity, and movement.

The example below shows a simple program that takes a colour reading and displays it as an RGB code:

```python
from sense_hat import SenseHat
sense_hat = SenseHat()
rgb = sense_hat.colour.colour
print(rgb)
```


Check out our [Getting started with the Sense HAT](https://projects.raspberrypi.org/en/projects/getting-started-with-the-sense-hat) project guide to learn more about how to take sensor readings using the Sense HAT.

### Taking photos with the camera

Another way to collect data on the ISS is by using the Astro Pi's camera. The camera can be used to capture images of the Earth, which can then be analysed as part of your investigation.

The example below shows a simple program that takes a photo:

```python
from picamzero import Camera

camera = Camera()
camera.take_photo("image.jpg")
```


Check out our [Getting started with the Camera Module](https://rpf.io/gswpicamera) project guide to learn more about how to use the camera.


![Photo of clouds above land.](images/image1.jpg)


## 3. Log data to file

Your program must save the data it collects so that it can be returned to Earth for analysis. This data could be sensor readings written to a file or images captured by the camera.

The example below shows how to save data to a CSV file:


```python
import csv

data = 42
with open("data.csv", "w") as csvfile:
   writer = csv.writer(csvfile),
   writer.writerow([str(data)])
```


<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">

⚠️ There are strict rules about the types of files you are allowed to save. Make sure to check the [Rulebook](https://astro-pi.org/mission-space-lab/rulebook) to see which file types are allowed.

</p>


## 4. Finish within your 10-minute time limit

Each Mission Space Lab program is allocated **exactly 10 minutes** to run on the ISS during daylight hours. Your program must keep track of how much time it has been running and stop automatically before the 10 minutes are up. This will help to ensure that your program finishes cleanly and that any data you have collected is saved correctly.

One way to do this is to use Python's `datetime` library:

1. Record the time when your experiment starts
2. Regularly check the current time while your program is running
3. Stop the program when 10 minutes have elapsed

The example below uses this approach to print "Hello from the ISS" every second for 1 minute:

```Python
from datetime import datetime, timedelta
from time import sleep

# Create a variable to store the start time
start_time = datetime.now()
# Create a variable to store the current time
# (these will be almost the same at the start)
now_time = datetime.now()
# Run a loop for 1 minute
while (now_time < start_time + timedelta(minutes=1)):
    print("Hello from the ISS")
    sleep(1)
    # Update the current time
    now_time = datetime.now()
# Out of the loop — stopping
```

--- task ---

Update your `main.py` file to make use of the `datetime` library to stop your program before the 10-minute time slot has finished.

--- /task ---

**Note:** When choosing how long your program should run, remember to account for the time taken to complete one iteration.

For example, if each cycle of your loop takes around 2 minutes to complete, you should not set your stop time to exactly 10 minutes. Instead, you should stop the loop after about 8 minutes. This will give the final iteration enough time to finish and ensure that your program exits before the 10-minute limit is reached.

## 5. Use the correct directory structure for your data files

When your code runs on the ISS, it will be started automatically by the Astro Pi flight operating system. This means you **must not use hard-coded file paths** in your code such as `/home/pi/Desktop`, as these locations may not exist on the flight computer.

Instead, use the special `__file__` variable to find the location of your `main.py` file and save any data files or photos relative to that location. This will ensure that your files are stored in the correct place, regardless of where your program is run.

The example below uses the pathlib library and the `__file__` variable to save files in the same directory as `main.py`: 

```python
from pathlib import Path

# Resolve the directory where main.py is currently running
dir_path = Path(__file__).parent.resolve()

# Create a safe file path inside your project directory
data_file = dir_path / "data01.csv"
```

--- task ---

Check that your code does not use absolute file paths.

--- /task ---


--- collapse ---
---
title: Basic full working example
---

This example code captures one photo and five readings from the magnetometer, then saves the data to a file.

You can adapt the code to capture more images and sensor readings, making the most of the 10 minutes available to your team.

`main.py`

```python
from picamzero import Camera
from sense_hat import SenseHat
import csv

camera = Camera()
sense_hat = SenseHat()

# take a photo and save it in a file called image.jpg
camera.take_photo("image.jpg")

# create a variable to store the sensor readings
data = []

# repeat 5 times
for i in range(5):
    # read the raw x,y,z data from the magnetometer.
    # The contents of magnetometer_reading will be a dictionary
    # that looks like this:
    #
    #   {
    #     "x": 7.907470226287842,
    #     "y": 5.466001033782959,
    #     "z": 12.868783950805664
    #   }
    magnetometer_reading = sense_hat.get_compass_raw()

    # Take the numeric values from the magnetometer_reading
    # and put them in a list called magnetometer_values
    magnetometer_values = list(magnetometer_reading.values())
    # append the values to the data list to save later
    data.append(magnetometer_values)

# open a file called data.csv for writing
with open("data.csv", "w") as csvfile:
    writer = csv.writer(csvfile)

    # write a header to explain what the columns mean
    writer.writerow(["magnetometer_x", "magnetometer_y", "magnetometer_z"])
    # write all 5 readings from the data list to data.csv
    writer.writerows(data)

```
--- /collapse ---


