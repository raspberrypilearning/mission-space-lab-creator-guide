## Minimum requirements for your program

We recommend that you start writing your program in small steps, and that you do not try to do everything at once. 

### What your program must do to achieve flight status

Your code must meet a baseline set of criteria to pass the strict checking process run by Astro Pi Mission Control. If you pass then you will achieve official **flight status** and have your program run aboard the ISS. If your code causes errors or fails to comply with these core operational requirements, it will not run on the ISS. 

#### 1. Write your main.py file

Every submission must include a file named `main.py`. This is the file from which your program will run, and which will be tested by Astro Pi Mission Control. Ideally, all of your functional code should be contained within this file, though additional background files are permitted. The program should write all data to file and finish before your alloted 10 minute window has ended.

--- task ---

Create a new file in Thonny and **Save as** `main.py` in your project folder.

--- /task --- 

#### 2. Capture sensor data

Your program must use at least one on-board sensor or the camera to capture data (you can use more if you wish). Programs that rely solely on external libraries to predict data — such as using the Skyfield library to look up the ISS position — do not qualify as using sensor data.

#### 3. Log to file

All data that you want to keep must be written to a file so that it can be downloaded back to Earth. Please see the [Rulebook](https://astro-pi.org/mission-space-lab/rulebook) for a list of acceptable file formats.

The `logzero` Python library makes it easy to monitor what's going on in your program. You can log as much information about what happens in your program — every loop iteration, every time an important function is called — and if you have conditionals in your program, `logzero` will log which route the program went (`if` or `else`).

Here's a basic example of how logzero can be used to keep track of loop iterations:

```python
from logzero import logger, logfile
from pathlib import Path
from time import sleep

base_folder = Path(__file__).parent.resolve()
logfile(base_folder/"events.log")

for i in range(10):
    logger.info(f"Loop number {i+1} started")
    ...
    sleep(60)
```

The two main types of log entry you can use are `logger.info()` to log information, and `logger.error()` when you experience an unexpected error or handle an exception. There's also `logger.warning()` and `logger.debug()`.

For example, if you had a function to detect night or dark from photos, you could log this information too:

```python
for i in range(10):
    if night_or_dark() == 'night':
        logger.info('night - wait 60 seconds')
        sleep(60)
    else:
        ...
```

If you want to handle an exception, but log that you did so, you can use `logger.error`:

```python
try:
    do_something()
 except Exception as e:
    logger.error(f'{e.__class__.__name__}: {e})')
```

For example, dividing by zero in `do_something` would create the following log entry:

```txt
[E 190423 00:04:16 test:9] ZeroDivisionError: division by zero
```

Your program would continue without crashing, but rather than seeing no log entry, you see that an error occurred at this time.

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
It's a good idea to use **both** the `csv` library (for recording experiment data) and the `logzero` library (for logging important events that take place during your experiment).
</p>


#### 4. Finish within your 10 minute time limit

Each program runs for exactly 10 minutes during ISS daylight hours. Your program must track this time and close automatically before the 10 minutes end so you do not lose any data.

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

#### 5. Use the correct directory structure for your data files

When your code is run on the ISS, it will be started and stopped by an automated system. Because of this, you must never use absolute or specific file paths in your code (for example, paths like `/home/pi/Desktop` will cause your program to crash because they do not exist on the flight system). 

To ensure that your logged data and photos end up in the correct directory, you must find the active folder dynamically using the special `__file__` variable alongside the `pathlib` library:

```python
from pathlib import Path

# Resolve the directory where main.py is currently running
dir_path = Path(__file__).parent.resolve()

# Create a safe file path inside your project directory
data_file = dir_path / "data01.csv"
```
--- task ---

Make sure all file creation routines in your main.py use dynamic pathlib resolution instead of hardcoded folder strings.

--- /task ---
