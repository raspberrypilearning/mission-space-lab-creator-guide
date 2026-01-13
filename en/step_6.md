## Testing your program

You should now test your program using **[Astro Pi Replay](https://rpf.io/replay)**. Doing this gives your entry the best chance of success and of ensuring that it will work aboard the ISS. When Astro Pi Mission Control receives your program, it will be tested and evaluated using Astro Pi Replay, and if it succeeds, on an Astro Pi on Earth. Hundreds of teams submit programs to Mission Space Lab each year and, unfortunately, there is not enough time to check for mistakes or debug code errors. If your program has errors when we test it, your program will not be eligible to run on the ISS.

If you have been following this creator guide from the start, you should have already seen the online [Astro Pi Replay Tool](https://rpf.io/replay), or installed the offline version as a Thonny plug-in. The installation instructions can be found earlier in this guide and in the Astro Pi Replay documentation.

To test your program and simulate it running aboard the ISS, go to the [Astro Pi Replay Tool](https://rpf.io/replay) and submit your program file. If you are using the Astro Pi Replay plug-in with Thonny, run your `main.py` code through the Astro Pi Replay plug-in by opening the **Run** menu and clicking on **Astro-Pi-Replay**.

Your code should complete within 10 minutes.

When it has finished, double-check that it created a `result.txt` file in your project folder with a valid structure. Additionally, observe any other output files created by your project. Did your saved files exceed the 250MB limit, or include file types that are not allowed in the rules? Finally, check your logs for any errors.
If you are using the online version of Astro Pi Replay, you may download a zip file of the output of your program.

If you see any errors, or the program does not do what you expected it to, you will need to address this before you submit your code, to make sure you have the best chance of achieving 'flight status'. You can rerun your experiment with the **Astro Pi Replay** tool as many times as needed until you are confident that your program works.

--- task ---

Test your program with **[Astro Pi Replay](https://rpf.io/replay)** and check the output for any problems or unexpected behaviour.

--- /task ---

### Program checklist

Your program will also be analysed to make sure that it adheres to the rulebook. Take the time now to read it and re-review your program to ensure it fits the bill.

--- task ---

Check your program against the [Mission Space Lab rulebook](https://astro-pi.org/mission-space-lab/rulebook).

--- /task ---

Many Mission Space Lab teams have not had their programs run on the ISS due to some common mistakes or errors in their programs. Below you will find a list of common mistakes with descriptions of why they affect the programs running on the ISS. 

### Common mistakes 

--- collapse ---
---
title: "ZeroDivisionError when calculating the speed"
---

You'll need to make sure that your program doesn't try to divide by zero when it tries to calculate the speed. This can happen when the image time fields field are rounded to the nearest second (such as when using the `datetime_digitized` field as in the [Calculate the speed of the ISS using photos](https://projects.raspberrypi.org/en/projects/astropi-iss-speed) project). If your code takes two photos in less than one second, they might appear to have the same timestamp, which will cause a `ZeroDivisionError` and make your program crash.

To stop this, you can add a `sleep` command to your code. This makes sure there is at least a one second gap between each photo:

```python
from time import sleep
from picamzero import Camera

camera = Camera()
camera.take_photo("image1.jpg")
sleep(1)
camera.take_photo("image2.jpg")
```

--- /collapse ---

--- collapse ---
---
title: "cv2.error when calculating image features"
---

If you are following the [Calculate the speed of the ISS using photos](https://projects.raspberrypi.org/en/projects/astropi-iss-speed) project and the photos you are comparing lack enough contrast, the `calculate_features` function might return `None` instead of the image descriptor data. This is a common cause of bugs, as later functions like `calculate_matches` expect a `numpy` array and will crash if they receive `None` instead.

To stop your program from crashing when this happens, you can wrap your code in a `try-except` statement like this:

```python
cv_error_count = 0
max_cv_errors = 5

for i in range(10):
    try:
        time_difference = get_time_difference(image_1, image_2) # Get time difference between images
        image_1_cv, image_2_cv = convert_to_cv(image_1, image_2) # Create OpenCV image objects
        keypoints_1, keypoints_2, descriptors_1, descriptors_2 = calculate_features(image_1_cv, image_2_cv, 1000) # Get keypoints and descriptors
        matches = calculate_matches(descriptors_1, descriptors_2) # Match descriptors
        display_matches(image_1_cv, keypoints_1, image_2_cv, keypoints_2, matches) # Display matches
        coordinates_1, coordinates_2 = find_matching_coordinates(keypoints_1, keypoints_2, matches)
        average_feature_distance = calculate_mean_distance(coordinates_1, coordinates_2)
        speed = calculate_speed_in_kmps(average_feature_distance, 12648, time_difference)

        # successfully calculated the speed - reset counts
        cv_error_count = 0
    except cv2.error as e:
        cv_error_count += 1
        if cv_error_count >= max_cv_errors:
            raise e
```

This snippet tries to calculate the features in your images, but instead of crashing when it hits a `cv2.error` it will skip and try again. Because the landscape and lighting are always changing below the ISS, the problem will often fix itself in the next iteration - but this is unfortunately not absolutely guaranteed. For this reason, the snippet counts the number of errors encountered and re-raises the `cv2.error` if more than 4 errors are encountered in a row. Should this happen, Astro Pi Mission Control will do their best to re-run your code in better conditions.

--- /collapse ---

--- collapse ---
---
title: "Opening and closing the camera repeatedly"
---

If you create mutliple `Camera` objects, for example in a loop, you are likely to make the Raspberry Pi run out of memory and not have your program accepted by Astro Pi Mission Control.

--- /collapse ---

--- collapse ---
---
title: "Not using full-resolution images"
---

Beware that the ground sampling distance, or GSD, can change with the final resolution of the image.

The value used in the [Calculate the speed of the ISS using photos](https://projects.raspberrypi.org/en/projects/astropi-iss-speed/0) project guide is valid for images in full resolution, `(4056x3040)`, but not necessarily for smaller resolutions. For this reason, we recommend that you capture full-resolution images.

--- /collapse ---

--- collapse ---
---
title: "Storing more than 42 images"
---

Your program is not allowed to retain more than 42 images at the end of the 10 minutes — though it can store more than that while it is running.

--- /collapse ---

--- collapse ---
---
title: "User input"
---

Your program cannot rely on interaction with an astronaut to work.

--- /collapse ---

--- collapse ---
---
title: "Using the LED matrix"
---

Your program is not allowed to use the LED matrix.

--- /collapse ---

--- collapse ---
---
title: "Poor documentation"
---

When you have created a useful piece of software and you want to share it with other people, a crucial step is creating documentation that helps people understand what the program does, how it works, and how they can use it. Make sure your program contains comments and the method used to determine the speed of the ISS is well explained.

[This project guide](https://projects.raspberrypi.org/en/projects/documenting-your-code/) shows you the recommended way to add useful comments to your program.

**Note:** Any attempt to hide, or make it difficult to understand, what a piece of code is doing may result in disqualification. And of course, there should be no bad language or rudeness in your code.

--- /collapse ---

--- collapse ---
---
title: "Overfitting to the replayed data"
---

Your code must be responsive to the images and sensor data on the ISS and not rely on specific landmarks or geographic areas shown in the sequence(s) used in Astro Pi Replay.

--- /collapse ---

--- collapse ---
---
title: "Use of absolute file paths"
---

Make sure that you do not use any specific paths for your data files. Use the `__file__` variable.

--- /collapse ---

--- collapse ---
---
title: "Not saving data immediately"
---

Make sure that any experimental data is written to a file as soon as it is recorded. Avoid saving data to an internal list or dictionary as you go along and then writing it all to a file at the end of the experiment, because if your experiment ends abruptly due to an error or exceeding the 10-minute time limit, you will not get any data.

--- /collapse ---

--- collapse ---
---
title: "Running out of space"
---

You are allowed to produce up to 250MB of data. Remember that the size of an image file will depend not only on the resolution, but also on how much detail is in the picture — a photo of a blank white wall will be smaller than a photo of a landscape. Using Astro Pi Replay will give you a good idea of how many pictures you will be able to take.

--- /collapse ---

--- collapse ---
---
title: "Forgetting to call your function"
--- 

We have seen cases where teams have written a function but forgotten to call it in their `main.py` program — watch out!

--- /collapse ---

--- collapse ---
---
title: "Saving into directories that do not exist"
--- 

A number of teams want to organise their data into directories such as data, images, etc. This in and of itself is a really good thing, but it is easy to forget to make these directories before writing to them.

--- /collapse ---

--- collapse ---
---
title: "Networking"
--- 

For security reasons, your program is not allowed to access the network on the ISS. It should not attempt to open a socket, access the internet, or make a network connection of any kind. This includes local network connections back to the Astro Pi itself.

--- /collapse ---

--- collapse ---
---
title: "Trying to run another program"
--- 

In addition to not being able to use any networking, your program is not allowed to run another program or any command that you would normally type into the terminal window of the Raspberry Pi, such as `vcgencmd`.

--- /collapse ---

--- collapse ---
---
title: "Multiple threads"
--- 

If you need to do more than one thing at a time, you can use a multithreaded process. There are a number of Python libraries that allow this type of multitasking to be included in your code. However, to do this on the Astro Pis, you are only permitted to use the `threading` library.

Only use the `threading` library if absolutely necessary. Managing threads can be tricky, and as your program will be run as part of a sequence of many other programs, we need to make sure that the previous one has ended smoothly before starting the next. Rogue threads can behave in an unexpected manner and take up too much of the system resources. If you do use threads in your code, you should make sure that they are all managed carefully and closed cleanly at the end of your program. You should also make sure that comments in your code clearly explain how this is achieved.

--- /collapse ---

--- collapse ---
---
title: "Setting the program execution time too short"
--- 

Some teams set their program execution time to a small value (e.g. 1 minute) for testing and then forget to change it back to an appropriate value. Make sure to use as much of your allocated time slot as possible.

--- /collapse ---

--- task --- 

Review your program again. Can you spot any of the common mistakes in your program?

--- /task ---
