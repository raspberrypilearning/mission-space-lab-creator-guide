# Enhancing your program

This section provide tips for improving your program and links to other project guides that will help you use the data and images you will log to investigate certain scientific phenomena. You are not limited to these ideas though - you are free to conduct any scientific experiment you want, providing your program adheres to the Rulebook.

## Predict the location of the ISS

You will be able to download up to 42 pictures that you take on the ISS. It can be nice to know where in orbit an image was taken, and this is something you can do easily with the `astro_pi_orbit` and `exif` libraries available on the Astro Pis.

The following is an example of a program that will, when run using the Astro Pi Replay Tool, create a new image called `gps_image1.jpg`. The `picamzero` library will have set the Exif metadata for the image to include the current latitude and longitude of the ISS.

```Python
from astro_pi_orbit import ISS
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
```

You will need to use the [Astro Pi Replay tool](https://rpf.io/replay)
to run this snippet.

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
Note that the latitude and longitude are `Angle` objects while the elevation is a `Distance`. The Skyfield documentation describes [how to switch between different angle representations](https://rhodesmill.org/skyfield/api-units.html#skyfield.units.Angle) and [how to express distance in different units](https://rhodesmill.org/skyfield/api-units.html#skyfield.units.Distance).

</p>

## Capturing sequences

Using `picamzero` it is very simple to take a sequence of pictures by calling the `capture_sequence` function. The example below takes three pictures in succession, with a 3 second gap between each one:


```Python
# Import the Camera class from the picamzero (picamzero) module
from picamzero import Camera

# Create an instance of the Camera class
cam = Camera()

cam.capture_sequence("sequence", num_images=3, interval=3)
```
Run this code using the [Astro Pi Replay Tool](https://rpf.io/replay)), and you should see that it takes 3 images.

## Numbering plans for images and files

When dealing with lots of files of the same type, it is a good idea to follow a naming convention. In the example above, we use an obvious sequence number — `image1.png`, `image2.png`, etc. — to keep our files organised.

If you need more help with using the camera, check out the ['Take still pictures with Python code' step](https://projects.raspberrypi.org/en/projects/getting-started-with-picamera/5) in our 'Getting started with the Camera Module' project guide.


## Closing resources 

When your program exits it is a good idea to close all resources that you have open. For example, close all files that you have open: 

```Python
file = open(file)
file.close()
```
--- task --- 

Review your `main.py` file and update it so that it closes all resources appropriately.

--- /task --- 

## Dealing with Errors and Exceptions

When Python encounters an error, it will throw either an `Error` or an `Exception`. This can be very frustrating, as it will cause your program to crash. With some foresight and planning, though, it is possible for your program to deal with these issues instead of crashing and potentially losing the chance to capture data and images on the ISS. 

Visit Ada Computer Science to learn more about [exception handling](https://adacomputerscience.org/concepts/design_exception?examBoard=all&stage=gcse).

## File buffering

When you write to a file using the `open` function, Python normally does not save the file to disk immediately. Instead, it keeps the file contents to save in a temporary storage area in the computer's memory called a buffer. Python does this so that it can choose the best time to write to the disk — something that normally does not matter us. But while the data is in the buffer and not yet saved to the disk, there is a chance that it could be lost if an error occurs. To prevent this from happening, we can tell Python to save the buffer to disk at the end of every line of text by setting the `buffering` argument to `1`:

```Python
with open("some_file.txt", "w", buffering=1) as f:
    f.write("example data")
```

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
**Note:** If you are writing bytes to a file (with argument `"wb"`), then you should tell Python to not use a buffer at all and to write the data to disk immediately. You can do this by setting the `buffering` argument to `0`.
</p>

--- task --- 

Review your program and consider if you need to set the buffering mode when writing to a file.

--- /task --- 

## Logging

If your program fails, then it is always helpful to have a record of what happened, so that you can fix it for next time. The `logzero` Python library ([documentation here](https://logzero.readthedocs.io/en/latest/)) makes it easy to make notes about what's going on in your program. You can log lots of information about what happens in your program — every loop iteration, every time an important function is called — and if you have conditionals in your program, `logzero` will log which route the program went (`if` or `else`). But remember that you cannot download more than 250MB of data from the ISS.

Here is a basic example of how `logzero` can be used to keep track of loop iterations:

```Python
from logzero import logger, logfile
from time import sleep

logfile("events.log")

for i in range(10):
    logger.info(f"Loop number {i+1} started")
    ...
    sleep(60)
```

The two main types of log entry you can use are `logger.info()` to log information, and `logger.error()` when your program experiences an unexpected error or handles an exception. There is also `logger.warning()` and `logger.debug()`.


<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
We recommend that you always use the `logzero` library (for logging important events that take place during your experiment), even if you also write sensor data to a file.

</p>


## Using your data

If you want to, you can use the data and images that you have captured to conduct some scientific research! You can conduct any scientific experiment that you want, providing that your program adheres to the [Rulebook](https://astro-pi.org/mission-space-lab/rulebook). Though, if you'd rather follow a ready-made guide, you can pick either of the projects below.

### Using NDVI (Normalized Difference Vegetation Index

Learn how to process visual light data to analyze plant health, vegetation density, and environmental features across the Earth's surface using this project guide:  [NDVI (Normalized Difference Vegetation Index)](https://projects.raspberrypi.org/en/projects/astropi-ndvi/0) 


### Calculating the speed of the ISS 

You may wish to start by learning how to write a program using historical photos taken by teams in previous years with our [Calculate the speed of the ISS using photos](https://projects.raspberrypi.org/en/projects/astropi-iss-speed/0) project guide. Once you have written a program, you can try it out using different images or data sets to improve the accuracy of your estimate. Here are some examples of images and data you can use:

- [Astro Pi Mission Space Lab 2022/23 photos](https://www.flickr.com/photos/raspberrypi/collections/72157722152451877/)
- [Astro Pi Mission Space Lab 2022/23 data](https://docs.google.com/spreadsheets/d/1RjPEp2IHVB6For65wuUQdWntsg1H5sHWpYUtLzK9LCM/edit?usp=sharing)
