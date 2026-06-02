## Writing your program

Conducting science experiments in space means working under some very strict constraints. This applies to astronauts and to you! This section sets out how to ensure your code behaves as expected while running on the ISS, and how to manage things like resources and errors.

We recommend that you start writing your program in small steps, and that you do not try to do everything at once. 

--- task ---

To keep everything organised, create a folder to store all your project files. For the name of the folder, you may wish to use your team name.

--- /task --- 

### What your program must do to achieve flight status

Your code must meet a baseline set of criteria to pass the strict checking process run by Astro Pi Mission Control. If you pass then you will achieve official **flight status** and have your program run aboard the ISS. If your code causes errors or fails to comply with these core operational requirements, it will not run on the ISS. 

#### 1. Write `main.py`

Every submission must include a file named `main.py`. This is the file from which your program will run, and which will be tested by Astro Pi Mission Control. Ideally, all of your functional code should be contained within this file, though additional background files are permitted. The program should write all data to file and finish before your alloted 10 minute window has ended.

--- task ---

Create a new file in Thonny and **Save as** `main.py` in your project folder.

--- /task --- 

#### 2. Capture sensor data

Your program must capture data from at least one of the on board sensors or the camera. You can record data from as many sensors as you like. You can run a more complex program if you wish, as long as there is at least one sensor used in the capture. It is not permitted, for example, to use only the Skyfield library to log the position of the ISS, as this data comes from a predicted list of positions, and does not receive the actual position data from a sensor. This is particularly relevant if you are using it to calculate the speed of the ISS. 

#### 3. Log to file

All data that you want to keep must be written to a file so that it can be downloaded back to Earth. Please see the [Rulebook](https://astro-pi.org/mission-space-lab/rulebook) for a list of acceptable file formats.

#### 4. Finish within your 10 minute time limit

Every program run on the Astro Pis has a 10-minute time slot in daylight. Your program will need to keep track of the time and shut down gracefully before the 10 minutes are over to make sure no data is lost.

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
