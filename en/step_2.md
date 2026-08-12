## Astro Pi hardware

The Astro Pis aboard the ISS are two modified Raspberry Pi 4 computers with 8GB of memory. Each one is fitted with a Sense HAT (that stands for ‘Hardware Attached on Top’) and a camera, and are housed in a custom aluminium flight case. 

The Sense HAT (V2) includes sensors that can measure temperature, humidity, light, and colour, as well as motion and orientation using a gyroscope, magnetometer, and accelerometer. This allows you to investigate things such as movement, acceleration, and the local magnetic field. 

The Astro Pis are also equipped with high-quality Raspberry Pi cameras with 5mm lenses, which can be used to take amazing pictures of the Earth from space. You can [find out more about the computers and sensors here](https://astro-pi.org/about/the-computers).

![Animation of the Astro Pi computers being taken apart.](images/AstroPi2-animation.gif)

To collect data that changes over time as the ISS orbits the Earth, you should focus on sensors such as the camera, gyroscope, magnetometer, accelerometer, or light and colour sensors.

Measurements such as temperature and humidity tend to stay fairly constant inside the ISS, while the passive infrared (PIR) movement sensor mainly detects crew activity near the Astro Pis. Although you can use movement, temperature, and humidity sensors in your project, they are less likely to produce data that changes significantly during your 10-minute experiment.

Choosing sensors that capture changing conditions will give you more interesting data to analyse.

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
Remember that your program must run completely autonomously from start to finish, and cannot rely on joystick inputs or button presses from an astronaut.
</p>


### The Astro Pi Python environment

The Astro Pi computers on the ISS use Python v3.13. If you are using a newer version on your own computer, be aware that some of the newer features may not work on the Astro Pis.
In addition to the Python standard library, the Astro Pis have several extra packages installed to help you complete your Mission Space Lab project. These are introduced below, with examples and links to more detailed documentation.

There are many different Python libraries available that can help you with your project. Choose the libraries that best support your project and help you achieve your investigation goals.

For security reasons, there are restrictions on which Python libraries you can use. [These library modules](https://docs.google.com/spreadsheets/u/0/d/1EoVzgA8gOiDXsJ1k9dQBdPyFC8U3bXFca2dRmdKNbcI/edit) are not permitted, and programs that use them will not be accepted.

--- collapse ---
---
title: Skyfield
---

### Usage

Skyfield is an astronomy package that computes the positions of stars, planets, and satellites in orbit around the Earth.

For example, you can use Skyfield to calculate the current position of Mars:

```python
from skyfield.api import Loader
from pathlib import Path

bsp_file = Path.home() / "de421.bsp"
load = Loader(bsp_file.parent)
planets = load(bsp_file.name)
mars = planets['Mars Barycenter']
ts = load.timescale()
barycentric = mars.at(ts.now())
print(barycentric)
```

This snippet works but the ephemeris file (`de421.bsp`) is too big to submit in your final payload. To get around this, import the ephemeris from the `astro_pi_orbit` library, which will take care of importing the file for you:

```python
from skyfield.api import load
from astro_pi_orbit import de421

planets = de421
mars = planets['Mars Barycenter']
ts = load.timescale()
barycentric = mars.at(ts.now())
print(barycentric)
```

### Documentation

- [rhodesmill.org/skyfield](https://rhodesmill.org/skyfield/)

--- /collapse ---

--- collapse ---
---
title: astro_pi_orbit
---
### Usage

The `astro_pi_orbit` library provides functionality to assist Astro Pi Mission Space Lab participants in working with orbital data. It can be used to:

1) Find the current location of the ISS
2) Access the `de421` or `de440s` ephemeris files (the files are too big to supply by yourself)
3) Access the trajectory of the ISS

### Documentation
- [https://astro-pi.github.io/astro-pi-orbit/]

--- /collapse ---

--- collapse ---
---
title: picamzero
---

The Python library for controlling the Raspberry Pi Camera Module on the Astro Pis is `picamzero`. To get started, check out this [project guide](https://raspberrypifoundation.github.io/picamzero/hello_world/) for a handy walkthrough of how to use it.

### Usage

```python
from picamzero import Camera
from time import sleep

camera = Camera()

# Take a picture every minute for 3 hours
for i in range(3*60):
    camera.take_photo(f'image_{i:03d}.jpg')
    sleep(60)
```

### Documentation

- [https://raspberrypifoundation.github.io/picamzero](https://raspberrypifoundation.github.io/picamzero)

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
The picamzero library is listed in Thonny, but it won’t install on Windows or macOS because it controls Raspberry Pi hardware and relies on Linux-only components. This is expected and won't stop you from developing your experiment — you can still run your code using the Astro Pi Replay Tool, either online or offline. To install picamzero on a Raspberry Pi, open the shell in Thonny and run:
```
pip install picamzero
```
</p>

--- /collapse ---

--- collapse ---
---
title: GPIO Zero
---

GPIO Zero is a simple but powerful GPIO (general-purpose input/output) library. Most of its functionality is restricted aboard the ISS — for example, the only pin you are allowed to access is GPIO pin 12, where the motion sensor is connected. However, some of its other features can be handy in your experiment, such as the internal device `CPUTemperature`.

### Usage

Compare the Raspberry Pi's CPU temperature to the Sense HAT's temperature reading:

```python
from sense_hat import SenseHat
from gpiozero import CPUTemperature

sense = SenseHat()
cpu = CPUTemperature()

while True:
    print(f'CPU: {cpu.temperature}')
    print(f'Sense HAT: {sense.temperature}')
```

### Documentation

- [gpiozero.readthedocs.io](https://gpiozero.readthedocs.io)

--- /collapse ---

[[[msl-numpy]]]

--- collapse ---
---
title: SciPy
---

SciPy is a free, open-source Python library used for scientific computing and technical computing. SciPy contains modules for optimisation, linear algebra, integration, interpolation, special functions, FFT (fast Fourier transform), signal and image processing, ODE (ordinary differential equations) solvers, and other tasks common in science and engineering. You may need to use this library to solve a particular equation.

### Documentation

- [docs.scipy.org/doc](https://docs.scipy.org/doc/)

--- /collapse ---

--- collapse ---
---
title: pandas
---

`pandas` is an open-source library providing high-performance, easy-to-use data structures and data analysis tools.

### Usage

```python
import pandas as pd

df = pd.read_csv("my_test_data.csv")
df.describe()
```

### Documentation

- [pandas.pydata.org](https://pandas.pydata.org/)

--- /collapse ---

--- collapse ---
---
title: logzero
---

`logzero` is a library used to make logging easier. Logs are records of what happened while a program was running, and can be really useful for debugging.

### Usage

Logs are categorised into different levels according to severity. By using the various levels appropriately, you will be able to tune the amount of information you get about your program according to your debugging needs.

```python
from logzero import logger

logger.debug("hello")
logger.info("info")
logger.warning("warning")
logger.error("error")
```

### Documentation

- [logzero.readthedocs.io](https://logzero.readthedocs.io/en/latest/)

--- /collapse ---

--- collapse ---
---
title: Matplotlib
---

`matplotlib` is a 2D plotting library that produces publication-quality figures in a variety of hard copy formats and interactive environments. You may want to use it to analyse the results of your test runs.

### Usage

```python
from sense_hat import SenseHat
from gpiozero import CPUTemperature
import matplotlib.pyplot as plt
from time import sleep

sense = SenseHat()
cpu = CPUTemperature()

st, ct = [], []
for i in range(100):
    st.append(sense.temperature)
    ct.append(cpu.temperature)
    sleep(1)

plt.plot(st)
plt.plot(ct)
plt.legend(['Sense HAT temperature sensor', 'Raspberry Pi CPU temperature'], loc='upper left')
plt.show()
```

![The output of the program is a temperature plot generated using matplotlib.](images/matplotlib_Figure_1.png)

### Documentation

- [matplotlib.org](https://matplotlib.org/)

--- /collapse ---

--- collapse ---
---
title: Pillow
---

Pillow is an image processing library. It provides extensive file format support, an efficient internal representation, and fairly powerful image processing capabilities.

The core image library is designed for fast access to data stored in a few basic pixel formats. It should provide a solid foundation for a general image processing tool.

### Documentation

- [pillow.readthedocs.io](https://pillow.readthedocs.io/)

--- /collapse ---

--- collapse ---
---
title: OpenCV
---

`opencv` is an open-source computer vision library. You may want to use OpenCV for [edge detection](https://projects.raspberrypi.org/en/projects/astropi-iss-speed/3), for example.

### Documentation

- [docs.opencv.org](https://docs.opencv.org/master/)

--- /collapse ---

--- collapse ---
---
title: exif
---

`exif` allows you to read and modify image Exif metadata using Python. You may want to use it to embed latitude and longitude coordinate data into any images you take, or to [analyse photos taken aboard the ISS](https://projects.raspberrypi.org/en/projects/astropi-iss-speed/1).

### Documentation

- [pypi.org/project/exif](https://pypi.org/project/exif/)

--- /collapse ---

--- collapse ---
---
title: scikit-learn
---

`scikit-learn` is a set of simple and efficient tools for data mining and data analysis that are accessible to everybody, and reusable in various contexts. It is designed to work with `numpy`, `scipy`, and `matplotlib`.

### Documentation

- [scikit-learn.org](https://scikit-learn.org)

--- /collapse ---

--- collapse ---
---
title: scikit-image
---

`scikit-image` is an open-source image processing library. It includes algorithms for segmentation, geometric transformations, colour space manipulation, analysis, filtering, morphology, feature detection, and more.

### Documentation

- [scikit-image.org](https://scikit-image.org/)

--- /collapse ---

--- collapse ---
---
title: reverse-geocoder
---

`reverse-geocoder` takes a latitude or longitude coordinate and returns the nearest town or city.

### Usage

When used with `skyfield`, `reverse-geocoder` can determine where the ISS currently is:

```python
import reverse_geocoder
from skyfield.api import Loader
from pathlib import Path

tle_file = Path.home() / "iss.tle"
load = Loader(tle_file.parent)
if not tle_file.exists():
    load.download("http://celestrak.com/NORAD/elements/stations.txt", filename=tle_file.name)
satellites = load.tle_file(tle_file.name)
iss = satellites[0]
ts = load.timescale()

coordinates = iss.at(ts.now()).subpoint()
coordinate_pair = (
    coordinates.latitude.degrees,
    coordinates.longitude.degrees)

location = reverse_geocoder.search(coordinate_pair)
print(location)
```
This output shows that the ISS is currently over Hamilton, New York:

```
[OrderedDict([
    ('lat', '42.82701'), 
    ('lon', '-75.54462'), 
    ('name', 'Hamilton'), 
    ('admin1', 'New York'), 
    ('admin2', 'Madison County'), 
    ('cc', 'US')
])]
```

Note: The library `reverse-geocoder` cannot be run using the online Replay Tool as it uses multiprocessing, which is incompatible with the environment of the tool. If you wish to use this library, you will have to test the relevent sections of your code locally in your code editor, or using the Thonny plug-in version of the Replay Tool.

### Documentation

- [github.com/thampiman/reverse-geocoder](https://github.com/thampiman/reverse-geocoder)

--- /collapse ---

--- collapse ---
---
title: sense_hat
---
The `sense_hat` library is the main library used to collect data using the Astro Pi Sense HAT. Look at [this project guide](https://projects.raspberrypi.org/en/projects/getting-started-with-the-sense-hat) to get started. 

### Usage

You can print the humidity using the code below:

```python
from sense_hat import SenseHat
sense = SenseHat()
print(str(sense.get_humidity()))
```

### Documentation

- [https://sense-hat.readthedocs.io/en/latest/](https://sense-hat.readthedocs.io/en/latest/)
- [Additional documentation for the light and colour sensor](https://gist.github.com/boukeas/e46ab3558b33d2f554192a9b4265b85f)

--- /collapse ---


--- collapse ---
---
title: "ai_edge_rt"
---

The `ai_edge_litert` library allows you to run machine learning and AI inference on the CPUs of the Astro Pis. It is the successor library to `tflite` and `pycoral`, both of which are no longer supported by Google.

### Usage

The example below shows how to load and execute the mobilenet v1 model:

```
from ai_edge_litert.interpreter import Interpreter
from PIL import Image

interpreter = Interpreter("mobilenet_v1_0.25_224_quant.tflite")
interpreter.allocate_tensors()

input_details = interpreter.get_input_details()
output_details = interpreter.get_output_details()

height = input_details[0]["shape"][1]
width = input_details[0]["shape"][2]
img = Image.open("black.jpg").resize((width, height))
input_data = np.expand_dims(img, axis=0)
interpreter.set_tensor(input_details[0]["index"], input_data)
interpreter.invoke()
output_data = interpreter.get_tensor(output_details[0]["index"])
print(f"tflite: {np.squeeze(output_data)}")
```
### Documentation

- [https://ai.google.dev/edge/litert/conversion/tensorflow/pretrained_models](https://ai.google.dev/edge/litert/conversion/tensorflow/pretrained_models)
- [https://ai.google.dev/edge/litert/migration#:~:text=Because%20LiteRT%20fully%20supports%20the,migration%20guides%20for%20specific%20platforms](https://developers.google.com/edge/litert/migration?_gl=1*1mlz13z*_up*MQ..*_ga*MzQ4NTMwNDUzLjE3ODU1MDQzMjk.*_ga_SM8HXJ53K2*czE3ODU1MDQzMjgkbzEkZzAkdDE3ODU1MDQzMjgkajYwJGwwJGgw)

--- /collapse ---


<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
Because of the strict security requirements for software running on the ISS, only the third-party libraries listed above can be used in your program. If you think something important is missing, or have suggestions for additional libraries, please let us know.
  
</p>

### Setting up your programming environment 

We recommend using Thonny to create your program. 

[[[thonny-install]]]

To install any of the Python libraries, open Thonny and click on **Tools > Manage packages...**.

![Screenshot of the 'Tools' menu in Thonny, with 'Manage packages...' highlighted.](images/skyfield_0.png){: width="50%"}

Search for the library you want by typing its name into the search bar.

![Screenshot of the package manager in Thonny, showing search results for the "skyfield" library.](images/skyfield_1.png)

Select the correct file from the search results, then press **Install**.

![Screenshot of the package manager in Thonny, showing the "skyfield" library and the 'Install' button.](images/skyfield_2.png)

If you are using a different IDE to write your code, you will need to follow local instructions for downloading the libraries you want from [PyPi](https://pypi.org/).

### Planning your project

Now that you have set up your programming environment, it's time to start planning your Mission Space Lab project.

Work with your team to decide what you want to investigate, how you will collect and analyse your data, and how you will divide up the work. Creating a plan before you start coding will help you stay organised and make steady progress.

Be sure to discuss your ideas, progress, and any challenges with your team mentor. They can help you refine your plans, solve problems, and keep your project moving forward.

