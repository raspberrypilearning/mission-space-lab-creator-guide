## Writing your program and resources to help

This section will help you get started with writing your program, and provide links to alternative project pathways that will help you develop the coding skills you need for your experiment. You can choose which reference guides to look at depending on which sensors and camera features you plan to use in your scientific investigation. At this point, you should have already spent some time with your team and your team mentor to plan your program, and have decided what target data streams you are going to collect.

We recommend that you start writing your program in small steps, and that you do not try to do everything at once. 

--- task ---

To keep everything organised, create a folder to store all your project files. For the name of the folder, you may wish to use your team name.

--- /task --- 

### The main.py file

[cite_start]Every submission must include a file named `main.py`. [cite: 60] [cite_start]This is the file from which your program will run, and which will be tested by Astro Pi Mission Control. [cite: 253] When your program is executed, it should autonomously handle your sensor queries, image captures, and file writing routines. Start by making a file for your main program, and add in the code that you get working as you go along.

--- task ---

Create a new file in Thonny and **Save as** `main.py` in your project folder.

--- /task --- 

### Directory structure for your data files

[cite_start]When your code is run on the ISS, it will be started and stopped by an automated system. [cite: 79] [cite_start]Because of this, you must never use absolute or specific file paths in your code (for example, paths like `/home/pi/Desktop` will cause your program to crash because they do not exist on the flight system). [cite: 78, 207]

[cite_start]To ensure that your logged data and photos end up in the correct directory, you must find the active folder dynamically using the special `__file__` variable alongside the `pathlib` library: [cite: 80, 81, 208]

```python
from pathlib import Path

# Resolve the directory where main.py is currently running
dir_path = Path(__file__).parent.resolve()

# Create a safe file path inside your project directory
data_file = dir_path / "data01.csv"
--- task ---Make sure all file creation routines in your main.py use dynamic pathlib resolution instead of hardcoded folder strings.--- /task ---Test your program with the Astro Pi Replay ToolThe Astro Pi Replay Tool acts as a kind of simulator you can use on Earth that will make your program act as if it is running on an Astro Pi on board the ISS. It allows you to test your code before it goes to space without needing to have a Raspberry Pi, camera, or Sense HAT. The simulation is not perfect, however, and will only produce photos and sensor data from within its own data set, but it should still allow you to test that your program would work when running on board the ISS.   There is an online version and an offline version, available as a Thonny plug-in, for you to test your program. We recommend you use the online version of the tool.  --- collapse ---title: Accessing the Astro Pi Replay Tool onlineThe easiest way to test if your program will work on the ISS is to upload your main.py file to the online Astro Pi Replay Tool.To upload your program simply, open the link and either drag and drop, or select, your main.py file and click run. The Replay tool will run your program in full, and show you the images and data you have captured, along with any files that your program outputs.Make sure your program executes successfully within the simulator environment and produces cleanly populated output files.--- /collapse ------ collapse ---title: Installing Astro Pi Replay Tool on Raspberry Pi BookwormIf you are on Raspberry Pi OS Bookworm, please follow the instructions on how to configure Thonny to use a virtual environment on the raspberrypi website before proceeding with the instructions below.To install the Astro Pi Replay tool, open Thonny, then click on Tools > Manage plug-ins..., and search for thonny-astro-pi-replay. Select the correct plug-in, then press Install.Then, click on Tools > Manage packages..., and search for astro-pi-replay. Select the correct package, then press Install.If you have taken part in Mission Space Lab before and have already downloaded the Astro Pi Replay tool, you should re-install the astro-pi-replay library to make sure you have the latest version. To do this, remove the ~/.astro_pi_replay directory in your home folder (e.g. using the command rm -rf ~/.astro_pi_replay in a Terminal window) and then follow the instructions above, as if you had not installed astro-pi-replay before.You will need to close and restart Thonny for the installation to complete.The Astro Pi Replay tool works by replaying a set of old pictures taken on the ISS. When your code goes to take a picture, instead of accessing some camera hardware, the library selects a picture to replay and acts as if it has just been captured 'live'.  {: width="50%"}  How to use the Astro Pi Replay plug-in  To run your code using the Astro Pi Replay plug-in, do not press the green Run button. Instead, open the Run menu, then click on Astro-Pi-Replay. This will run your code as if it was running on Astro Pi hardware.--- /collapse ---  Note: Although all of the functions of the picamzero library are available, many of the picamzero settings and parameters that would normally result in a different picture being captured are silently ignored when the code is executed using Astro Pi Replay. Additionally, most attributes on the Camera object are ignored. For example, setting the resolution attribute to anything other than (4056,3040) has no effect when simulated on Astro Pi Replay, but would change the resolution when run on an Astro Pi in space.Exploring alternative project guidesTo help you map out your scientific investigation, you can look at full project guides designed around specific telemetry and imaging methods. Use these resources for inspiration on how to capture, format, and evaluate your data sets:NDVI (Normalized Difference Vegetation Index) Guide: Learn how to process visual light data to analyze plant health, vegetation density, and environmental features across the Earth's surface.ISS Speed Guide: Learn how to track motion across successive image captures to calculate the relative speed of the Space Station in orbit.You can also test your scripts using historical data sets collected during previous challenge cycles:Astro Pi Mission Space Lab photosAstro Pi Mission Space Lab data sheetsSimulate running your program in real timeYou may prefer to get started by using the sense_hat and picamzero libraries and simulating running your program in real time. To simulate reading data from the Sense HAT and capturing photos from the camera, you will use the Astro Pi Replay tool online or with Thonny.Taking measurements with the Sense HATIn order to gather experimental metrics, you may wish to gather data from the sensors on the Sense HAT. Check out our Getting started with the Sense HAT project guide to learn how to do this.Taking photos with the cameraYou may also wish to use the camera to take photos of the Earth to use in your program. You can use our Getting started with the Camera Module project guide to learn how to do this. However, if you do not have a Raspberry Pi and High Quality Camera to test your code on, you can still run the same code using the Astro Pi Replay Tool.Here is an example of a simple program to test the Astro Pi Replay plug-in, if you are using the offline version in Thonny:Code snippet# Import the Camera class from the picamzero module
from picamzero import Camera

# Create an instance of the Camera class
cam = Camera()

# Capture an image
cam.take_photo("image1.jpg")
This will simulate taking a picture on the ISS and save it in a file called image1.jpg. If you open this file, you should see the exact photo below.The picamzero library supports a variety of features and camera settings. You can see some examples by going to the 'Recipes' page on the picamzero website, but be mindful that if your code is run on the ISS, it will be taking pictures of a variety of weather conditions with a range of clouds, landscapes, and lighting. However, your program is always guaranteed to be run in daylight.  While all features of the picamzero library will be available on the Astro Pi in space, not all can be simulated by the Astro Pi Replay Tool.  Capturing sequencesUsing picamzero it is very simple to take a sequence of pictures by calling the capture_sequence function. The example below takes three pictures in succession, with a 3 second gap between each one.  Create a new file called camera-sequence.py, and in it, type the following lines:Code snippet# Import the Camera class from the picamzero module
from picamzero import Camera

# Create an instance of the Camera class
cam = Camera()

cam.capture_sequence("sequence", num_images=3, interval=3)
Run this code using Astro Pi Replay online, or with the Thonny plug-in by clicking on Run > Astro-Pi-Replay.Numbering plans for images and filesWhen dealing with lots of files of the same type, it is a good idea to follow a naming convention. In the example above, we use an obvious sequence number — image1.png, image2.png, etc. — to keep our files organised.   If you need more help with using the camera, check out the 'Take still pictures with Python code' step in our 'Getting started with the Camera Module' project guide.--- task ---Update your main.py file to capture images or Sense HAT data in real time.--- /task ---Finding the location of the ISS & Adding EXIF DataYou can find out where exactly an image was taken using the astro_pi_orbit and exif libraries. A highly effective way to keep track of this is by saving the geographic coordinates directly into the EXIF metadata fields within the image files themselves. This attaches the location permanently to the photo without needing a separate matching spreadsheet.   The coordinates in the EXIF data of images are stored using a variant of the degrees:minutes:seconds (DMS) format. The following example shows how to configure picamzero to query the astro_pi_orbit library and automatically embed the real-time position of the ISS as EXIF metadata tags when a picture is captured:   Code snippetfrom astro_pi_orbit import ISS
from picamzero import Camera

iss = ISS()

def get_gps_coordinates(iss):
    """
    Returns a tuple of latitude and longitude coordinates expressed
    in signed degrees minutes seconds.
    """
    point = iss.coordinates()
    return (point.latitude.signed_dms(), point.longitude.signed_dms())

cam = Camera()
cam.take_photo("gps_image1.jpg", gps_coordinates=get_gps_coordinates(iss))
You will need to use the Astro Pi Replay tool to run this snippet.  Note that the latitude and longitude are Angle objects while the elevation is a Distance. The Skyfield documentation describes how to switch between different angle representations and how to express distance in different units.Basic Data Analysis: Calculating AveragesOnce your program begins tracking multiple data points over its 10-minute runtime loop, you can perform live mathematical analysis inside your script. For instance, you can use Python to calculate moving averages of sensor outputs to smooth out anomalous background spikes.The example below demonstrates how to keep a running list of magnetometer readings and output a clean mathematical average to help analyze structural magnetic fields:Code snippet# Example of tracking sensor values to calculate an average
magnetic_readings = []

# Simulate adding new readings inside your loop
magnetic_readings.append(45.2)
magnetic_readings.append(47.6)
magnetic_readings.append(44.1)

# Calculate the average value
average_field = sum(magnetic_readings) / len(magnetic_readings)
print(f"Average magnetic field strength: {average_field:.2f}")
Logging data to a CSV fileFor your submission to pass testing by Astro Pi Mission Control, your program must collect and store your experimental data values systematically. These measurements should be written immediately to a comma-separated values table in your directory called data01.csv. Writing data point rows continuously using append mode ('a') ensures your progress is safely recorded on the storage disk even if an unexpected interruption occurs.   The following example shows how to use the standard csv library to create your data file, initialize it with descriptive table headers, and append new experimental records dynamically:   Code snippetimport csv
from pathlib import Path
from datetime import datetime

# Resolve the safe project directory path
dir_path = Path(__file__).parent.resolve()
data_file = dir_path / "data01.csv"

# 1. Create the CSV file and write the header row once
with open(data_file, 'w', newline='') as f:
    writer = csv.writer(f)
    header = ("Timestamp", "Reading_Index", "Sensor_Value")
    writer.writerow(header)

# 2. Function to append data rows during your experiment loop
def log_experimental_data(file_path, index, value):
    with open(file_path, 'a', newline='') as f:
        writer = csv.writer(f)
        row = (datetime.now(), index, value)
        writer.writerow(row)

# Example usage inside an investigation loop
log_experimental_data(data_file, 1, 101.35)
print("Data written successfully to", data_file)
  --- task ---Update your main.py file so that it initializes and writes data points to a structured file called data01.csv during execution.--- /task ---Make sure to check the Mission Space Lab rulebook for rules on files and file names.
