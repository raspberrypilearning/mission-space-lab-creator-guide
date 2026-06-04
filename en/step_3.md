# Writing your program

We recommend that you write your Python program in small, incremental steps rather than trying to write everything at once. By following the steps below, you will develop the core elements of your Mission Space Lab submission.

## 1. Write your main.py file

Every submission must include a file named `main.py`. This file acts as the "launchpad" for your program, and it is the exact file that the automated system on the ISS will look for and run. You may include extra Python files in your submission, but your main code logic _must_ start in your `main.py` file. Unless you already know how to work with multiple Python files, we recommend that you put all of your functional code into the `main.py` file.

--- task ---

Create a new file called `main.py` in your project folder and save it.

--- /task --- 

## 2. Capture sensor data

Your program must use at least one of the Astro Pi's sensors or the camera to capture data. Programs that rely solely on external libraries to predict data — such as using the `skyfield` library to look up the ISS position — do not qualify as using sensor data.

### Taking measurements with the Sense HAT 

To gather environmental data from the ISS, you can use the sensors on the Sense HAT. Here is an example of a simple program that takes a colour and light-level reading:

```python
from sense_hat import SenseHat
sense_hat = SenseHat()
rgb = sense_hat.colour.colour
print(rgb)
```

Check out our [Getting started with the Sense HAT](https://projects.raspberrypi.org/en/projects/getting-started-with-the-sense-hat) project guide to learn more about how to take sensor readings using the Sense HAT.

### Taking photos with the camera

To take a photo of Earth from the ISS you can use the Astro Pi's camera. Here is an example of a simple program that takes a photo:

```python
from picamzero import Camera

camera = Camera()
camera.take_photo("image.jpg")
```

Check out our [Getting started with the Camera Module](https://rpf.io/gswpicamera) project guide to learn more about how to use the camera. 

![Photo of clouds above land.](images/image1.jpg)

## 3. Log data to file

Your program must write some sensor data to a file or save an image. This will allow the data to be downloaded back to Earth for you to analyse and enjoy. You can save your data into a file using this code snippet:

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

## 4. Finish within your 10 minute time limit

Each Mission Space Lab program is allocated exactly 10 minutes during ISS daylight hours. Your program must track how much time has elapsed and close automatically before the 10 minutes end so you do not lose any data.

You can use the Python `datetime` library to stop your program automatically. Here is how it works:

1. Save the exact time your experiment starts.
2. Check the current time repeatedly during the experiment.
3. Stop the program safely when the current time reaches your start time plus 10 minutes.

In the program below, this is used to print "Hello from the ISS" every second for 1 minute: 

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

**Note:** When deciding on the runtime for your program, make sure you take into account how long it takes for your loop to complete a cycle. For example, if you want to make use of the full 10-minute slot available, but each loop through your code takes 2 minutes to complete, then your `timedelta` should be **10-2 =** `8` minutes, to ensure that your program finishes before 10 minutes have elapsed.

## 5. Use the correct directory structure for your data files

When your code is run on the ISS, it will be started and stopped by an automated system. Because of this, you must never use absolute or specific file paths in your code (for example, paths like `/home/pi/Desktop` will cause your program to crash because they do not exist on the flight system). 

To ensure that your logged data and photos end up in the correct directory, you must find the active folder dynamically using the special `__file__` variable, which points to the location of the current file. The code snippet below uses the `__file__` variable and `pathlib` library to write files in the same directory as where the `main.py` file is stored:

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
title: Basic working example
---

This example code will capture 1 photo and 5 readings from the magnetometor sensor before logging the data to file. Add more code to capture more images and sensor data to maximise the 10 minutes avaialble for your team.

main.py

```python
from sense_hat import SenseHat
sense_hat = SenseHat()
rgb = sense_hat.colour.colour
print(rgb)

```


--- /collapse ---


