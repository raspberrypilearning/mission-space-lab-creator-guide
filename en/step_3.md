## Writing your program and resources to help

This section will help you get started with writing your program, and provide links to other projects that will give you some of the coding skills you need. You can pick and choose which project guides you want to look at depending on which sensors or camera you are going to use in your program. At this point, you should have already spent some time with your team to plan your program with your mentor, and have decided what data you are going to collect to make your calculations. 

### Getting Started

We recommend that you start writing your program in small steps, and don’t try and do everything all at once. 

--- task ---

To keep everything organised, create a folder to store all your project files. You may wish to call it your team name.

--- /task --- 

### The main.py file

Every submission must include a file named `main.py`. This is the file from which your program will run, and which will be tested by Astro Pi Mission Control. When you run the finished program, it should do everything you need to estimate the speed of the ISS. Start by making a file for your main program, and add in the code you get working as you go along.

--- task ---

Create a new file in Thonny and ‘Save As’ `main.py` in your project folder

--- /task --- 

### Installing Astro Pi Replay

Next you will install the Astro Pi Replay tool, which allows us to simulate using an Astro Pi Sense HAT or camera to capture data from space.

add image 
add image

To install the Astro-Pi-Replay tool, open up Thonny, and then click **Tools > Manage Plugins**, and search for **thonny-astro-pi-replay**. Select the correct plugin and press install. 

add image

Next, click **Tools > Manage Packages**, and search for **astro-pi-replay**. Select the correct package and press install. 

**You will need to close and restart Thonny for the installation to complete.**

<p style="border-left: solid; border-width:10px; border-color: #d17500; #ff8f00; background-color: #ff8f00; padding: 10px;">
The `Astro Pi Replay` tool works by replaying a set of old pictures taken on the ISS. When your code asks to take a picture, instead of accessing some camera hardware, the library selects a picture to replay and pretends that it has just been captured “live”.

add image

**How to use the Astro Pi Replay Plugin**

To run your code using the Astro Pi Replay plugin, do **not** press the green **“Run”** button. Instead, click **Run>Astro-Pi-Replay**. This will run your code as if it was running on Astro Pi hardware.

Although all of the functions of the `picamera` library are available, many of the `picamera` settings and parameters that would normally result in a different picture being captured are silently ignored when the code is executed using `Astro Pi Replay`. Additionally, most attributes on the `PiCamera` object are ignored. For example, setting the resolution attribute to anything other than `(4056,3040)` has no effect when simulated on `Astro Pi Replay` but would change the resolution when run on the Astro Pi in space. 
</p>

### Calculating with historical data 

You may wish to start by learning how to write a program that estimates the speed of the ISS using the camera module with this project “[calculate the ISS speed with photos](https://projects.raspberrypi.org/en/projects/astropi-iss-speed/0)". Then, once you have written a program you can try it out using different images or data sets to improve the accuracy of your estimate. Here are some examples of images and data you can use:

- [Astro Pi 2022/23 Mission Space Lab Photos](https://www.flickr.com/photos/raspberrypi/collections/72157722152451877/)
- [Astro Pi 2022/23 Mission Space Lab Data](https://docs.google.com/spreadsheets/d/1RjPEp2IHVB6For65wuUQdWntsg1H5sHWpYUtLzK9LCM/edit?usp=sharing)

### Simulate running your program in real time

You may prefer to get started by using the `sense_hat` and `picamera` libraries and simulating running your program in real time. To simulate reading data from the SenseHAT and capturing photos from the camera, you will use the Astro Pi Replay tool. Using the tool is simple - instead of running your program by pressing the green “run” button, click **Run > Astro-Pi-Replay**.

### Taking measurements with the SenseHAT 
In order to calculate the speed of the ISS, you’ll need to gather data from the sensors on the Sense HAT. Check out our project “[Getting Started With the Sense HAT](https://projects.raspberrypi.org/en/projects/getting-started-with-the-sense-hat)” to learn how to do this. 

Remember that you will need to run your code using the **Astro-Pi-Replay** plugin on Thonny. 

### Taking photos with the Camera Module 

You may also wish to use the camera module to take photos of the Earth to use in your program. You can complete the activity “[Getting started with the Camera Module](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera)" to learn how to do this. However, if you do not have a Raspberry Pi and camera module to test your code on, you can still run the same code through the Astro-Pi-Replay plugin.

This will simulate taking a picture on the ISS and save it in a file called image1.jpg. If you open this file, you should see the exact photo below. 

![Image of clouds above the sea outputted by the Astro Pi Replay tool](images/image1.jpg)

The `picamera` library offers a huge number of features and camera settings. You can see some more advanced examples by going to the [Basic Recipes](https://picamera.readthedocs.io/en/release-1.13/recipes1.html) page on the picamera website, but be mindful that if your code is run on the ISS then it will be taking pictures of a variety of weather conditions with a range of clouds, landscapes, and lighting. 

While all features of the picamera library will be available on the Astro Pi in space, not all can be simulated by the Astro-Pi-Replay plugin. More information is available here.

### Capturing sequences

Using a **for** loop, it’s very simple to take a sequence of pictures by repeatedly calling the capture function. The example below takes 3 pictures in succession. It also saves the images as a png instead of a jpeg.

Create a new file called camera-sequence.py and in it type the following lines:

```
s = "Python syntax highlighting"
print s

# Import the PiCamera class from the picamera module
from picamera import PiCamera

# Create an instance of the PiCamera class
cam = PiCamera()

# Set the resolution of the camera to 4056x3040 pixels
cam.resolution = (4056, 3040)

# Capture three images using a loop
for i in range(3):
    # Capture an image and save it 
    # with a filename like 
    # "image0.png", "image1.png", etc.
    cam.capture(f"image{i}.png")
```
Run this code using the Astro-Pi-Replay plugin by clicking **Run > Astro-Pi-Replay**.

### Numbering plans for images and files

When dealing with lots of files of the same type, it’s a good idea to follow a naming convention. In the example above, we use an obvious sequence number: `image1.jpg`, `image2.jpg`, etc., to keep our files organised.

If you need more help with using the picamera, then check out the [Take still pictures with Python code](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera/5) project. 

--- task --- 

Can you update your main.py file to capture images or SenseHAT data in real-time?

--- /task --- 

### Finding the location of the ISS

You will be able to download up to 42 pictures that you take on the ISS. It can be nice to know where exactly an image was taken - something you can do easily with the `orbit` and `exif` libraries available on the Astro Pis.

The following is an example of a program that will, when run using the Astro-Pi-Replay plugin, create a new image called `gps_image1.jpg`. The `custom_capture` function will have set the EXIF metadata for the image to include the current latitude and longitude of the ISS. There are several ways of formatting [latitude and longitude](https://www.britannica.com/science/latitude) angles, and using the custom_capture function. You will have to adapt this code to suit your particular program.

add code block

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
Note that the latitude and longitude are `Angle` objects while the elevation is a `Distance`. The skyfield documentation describes [how to switch between different angle representations](https://rhodesmill.org/skyfield/api-units.html#skyfield.units.Angle) and [how to express distance in different units](https://rhodesmill.org/skyfield/api-units.html#skyfield.units.Distance).

</p>

### Machine Learning with the Coral Accelerator 

If you have access to a Coral Machine Learning accelerator, check out the [Image classification](https://projects.raspberrypi.org/en/projects/image-id-coral/2) project. You will walk through the process of training a machine learning model to classify images and experience using the `tensorflow_lite` library. You can then use a similar approach to classify images played back when you run your program using Astro Pi Replay, or on the ISS. 

Once you’ve completed this project you may want to look at the [Coral examples page](https://coral.ai/examples/) and [this GitHub page](https://github.com/robmarkcole/satellite-image-deep-learning#datasets) for some inspiration on how to apply machine learning techniques to your own experiment. 

### Writing a your result file 

For your submission to pass testing by Mission Control your program needs to write a file called `result.txt` that contains your estimate for the speed of the ISS. This file must be in text file format (.txt), and will contain your estimate to up to five decimal points. Please do not include any other data in this file.

**7.12345**
*Example result.txt for an average speed estimate.*

The following is an example of a program that will write a txt file called "`result.txt`" with an estimated speed value in km/s (kilometers per second) to 5 decimal places. You will have to adapt this code to suit your particular program.

add code block

--- task --- 

Update your `main.py` file so that it writes a file called `result.txt` when it is executed.

--- /task --- 

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">

Make sure to check the [Mission Space Lab Official Guidelines](https://astro-pi.org/mission-space-lab/guidelines/program-checklist) for rules on files and filenames.

</p>
