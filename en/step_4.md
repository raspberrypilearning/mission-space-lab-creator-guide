# Enhancing your program

This section provides ideas for improving your program, along with links to other project guides that show how data and images collected on the ISS can be used to investigate different scientific phenomena. 

These guides are intended to inspire your ideas, but you are not limited to the projects described here. You are free to design and carry out your own scientific investigation, provided that your program follows the [Rulebook](https://astro-pi.org/mission-space-lab/rulebook).

## Predict the location of the ISS

You will be able to download up to 42 pictures that you take on the ISS. It can be nice to know where in orbit an image was taken, and this is something you can do easily with the `astro_pi_orbit` and `exif` libraries available on the Astro Pis.

The following is an example of a program that will create a new image called `image_with_coordinates.jpg`. The `picamzero` library will have set the Exif metadata for the image to include the current latitude and longitude of the ISS.

```Python
from astro_pi_orbit import ISS
from picamzero import Camera

iss = ISS()

def get_predicted_latlon_coordinates(iss):
    """
    Returns a tuple of latitude and longitude coordinates expressed
    in signed degrees minutes seconds.
    """
    point = iss.coordinates()
    return (point.latitude.signed_dms(), point.longitude.signed_dms())

cam = Camera()
cam.take_photo("image_with_coordinates.jpg", latlon_coordinates=get_predicted_latlon_coordinates(iss))
```

You will need to use the [Astro Pi Replay Tool](https://rpf.io/replay) to run this snippet.

## Capturing sequences of images

With `picamzero` it is very simple to take a sequence of pictures by calling the `capture_sequence` function. The example below takes three pictures in succession, with a 3-second gap between each one:


```Python
# Import the Camera class from the picamzero (picamzero) module
from picamzero import Camera

# Create an instance of the Camera class
cam = Camera()

cam.capture_sequence("sequence", num_images=3, interval=3)
```
Run this code using the [Astro Pi Replay Tool](https://rpf.io/replay), and you should see that it takes 3 images.

## Numbering plans for images and files

When dealing with lots of files of the same type, it is a good idea to follow a naming convention. We recommend that you use an obvious sequence number that is padded with leading zeros — `image01.png`, `image02.png`, etc. — to keep your files organised. You can use this code snippet to pad numbers with leading zeros:

```python
file_number = 1
filename = f"image_{file_number:03d}.jpg"
print(filename)
```

## File buffering

When you write to a file using the `open` function, Python normally does not save the file to disk immediately. Instead, it keeps the file contents to be saved in a temporary storage area in the computer's memory called a buffer. Python does this so that it can choose the best time to write to the disk — something that normally does not matter to us. But while the data is in the buffer and not yet saved to the disk, there is a chance that it could be lost if an error occurs. To prevent this from happening, you can tell Python to save the buffer to disk at the end of every line of text by setting the `buffering` argument to `1`:

```Python
with open("some_file.txt", "w", buffering=1) as f:
    f.write("example data")
```

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">

**Note:** If you are writing bytes to a file (with argument `"wb"`), then you should tell Python to not use a buffer at all and to write the data to disk immediately. You can do this by setting the `buffering` argument to `0`.
</p>


## Using your data

If you want to, you can use the data and images that you have captured to conduct some scientific research! You can conduct any scientific experiment that you want, providing that your program adheres to the [Rulebook](https://astro-pi.org/mission-space-lab/rulebook). If you'd rather follow a ready-made guide instead, you can pick either of the projects below.

### Using NDVI (normalised difference vegetation index)

Learn how to process visual light data to analyse plant health, vegetation density, and environmental features across the Earth's surface using our ['Capture plant health with NDVI and Raspberry Pi' project guide](https://projects.raspberrypi.org/en/projects/astropi-ndvi/0).


### Calculating the speed of the ISS

You may wish to start by learning how to write a program using historical photos taken by teams in previous years with our ['Calculate the speed of the ISS using photos' project guide](https://projects.raspberrypi.org/en/projects/astropi-iss-speed/0). Once you have written a program, you can try it out using different images or data sets to improve the accuracy of your estimate. Here are some examples of images and data you can use:

- [Astro Pi Mission Space Lab 2022/23 photos](https://www.flickr.com/photos/raspberrypi/collections/72157722152451877/)
- [Astro Pi Mission Space Lab 2022/23 data](https://docs.google.com/spreadsheets/d/1RjPEp2IHVB6For65wuUQdWntsg1H5sHWpYUtLzK9LCM/edit?usp=sharing)
