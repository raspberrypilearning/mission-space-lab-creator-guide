## Optimising your program for the ISS

For astronauts, working in space means working under some very strict constraints, and the same applies to you! This section sets out how to ensure your code behaves as expected while running on the ISS, and how to manage things like resources and errors.

### Running an experiment for 10 minutes 

Every program run on the Astro Pis has a 10-minute time slot in daylight to estimate the speed of the ISS. Your program will need to keep track of the time and shut down gracefully before the 10 minutes are over to make sure no data is lost.

One way to stop a Python program after a specific length of time is using the `datetime` Python library. This library makes it easy to work with times and compare them. 

By recording and storing the time at the start of the experiment, we can then check repeatedly to see if the current time is greater than that start time plus a certain number of minutes, seconds, or hours. In the program below, this is used to print "Hello from the ISS" every second for 1 minute: 

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

### Using relative paths

Your program is going to be stored in a different location when it is deployed on the ISS, so it is really important to avoid using absolute file paths when writing your `result.txt` file (or any other file you might want to write). Use the code below to work out which folder the `main.py` file is currently stored in, which is called the `base_folder`:

```Python
from pathlib import Path
base_folder = Path(__file__).parent.resolve()
```

Then you can save your data into a file underneath this `base_folder`: 

```Python
data_file = base_folder / "data.csv"
for i in range(10):
    with open(data_file, "w", buffering=1) as f:
        f.write(f"Some data: {i}")
```

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
Make sure to check the [Mission Space Lab rulebook](https://astro-pi.org/mission-space-lab/rulebook) for the rules on files and file names.

</p>

### Closing resources 

At the end of the experiment, it is a good idea to close all resources that you have open. For example, close all files that you have open: 

```Python
file = open(file)
file.close()
```
--- task --- 

Review your `main.py` file and update it so that it closes all resources appropriately.

--- /task --- 



### Preparing for the unexpected

A program can fail for many reasons, but with some foresight and planning, it is possible for your program to deal with these issues instead of crashing and losing the chance to capture data and images aboard the ISS. In this section, you are going to try to find ways to improve your program so that it stands the best chance of working as intended if something unexpected happens.


--- collapse ---
---
title: "Exception handling"
---

An exception is when something happens while a program is running that it does not know how to handle. This can cause the program to crash, unless it has a procedure to follow in the event of something going wrong.

Visit Ada Computer Science to learn more about [exception handling](https://adacomputerscience.org/concepts/design_exception?examBoard=all&stage=gcse). 

--- /collapse ---

--- collapse ---
---
title: "File buffering"
---

When you write to a file using the `open` function, Python normally does not save the file to disk immediately. Instead, it keeps the file contents to save in a temporary storage area in the computer's memory called a buffer. Python does this so that it can choose the best time to write to the disk — something that normally does not matter us. But while the data is in the buffer and not yet saved to the disk, there is a chance that it could be lost if an error occurs. To prevent this from happening, we can tell Python to save the buffer to disk at the end of every line of text by setting the `buffering` argument to `1`:

```Python
with open("some_file.txt", "w", buffering=1) as f:
    f.write("example data")
```

--- /collapse ---

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
**Note:** If you are writing bytes to a file (with argument `"wb"`), then you should tell Python to not use a buffer at all and to write the data to disk immediately. You can do this by setting the `buffering` argument to `0`.
</p>

--- task --- 

Review your program and consider if you need to set the buffering mode when writing to a file.

--- /task --- 

--- collapse ---
---
title: "Logging"
---

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

--- /collapse ---

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
We recommend that you always use the `logzero` library (for logging important events that take place during your experiment), even if you also write sensor data to a file.
</p>

Once you have finished writing your program and you believe it provides the ISS speed estimate in the correct format and follows best practices like logging and handling errors, it is crucial to thoroughly test your program using the Astro Pi Replay Tool.
