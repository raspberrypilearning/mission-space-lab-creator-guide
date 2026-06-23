# Introduction
In Mission Space Lab, your challenge is to design and carry out a scientific investigation using an Astro Pi on board the International Space Station (ISS). To do this, you will write a Python program that gathers and logs data using the Astro Pi's sensors and camera. These tools give you the opportunity to investigate questions about the environment on the ISS and collect real data from space.

Your program will run autonomously for 10 minutes aboard the ISS. During this time, it must collect data using the Astro Pi's sensors or camera and save that data to a file for later analysis.

What you do with those 10 minutes is up to you. You could investigate a scientific question, collect and analyse sensor data, take photographs of the Earth, or create a more complex program that combines several of these ideas. The choice is yours.

![A sequence of photos of the Earth's surface taken by an Astro Pi computer.](images/Atlas.gif)

Before it can run on the ISS, your code must pass a series of checks carried out by Astro Pi Mission Control. These checks ensure that your program runs correctly and meets the mission requirements.

If your code passes these checks, it will achieve official flight status and be scheduled to run aboard the ISS. If it contains errors or does not meet the core operational requirements, it will not be able to fly.

This is not a complete step-by-step guide to creating your program. Instead, it will help you get started. You and your team will need to make decisions about what you want your investigation to achieve and how to implement your ideas in code.

<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">

Getting help

Head over to the [Astro Pi website](https://astro-pi.org/mission-space-lab/) for more information about Mission Space Lab.

If you get stuck, please [contact us](mailto:enquiries@astro-pi.org) and we will do our best to help you!

We will also be running two livestreams where you can ask questions to the Astro Pi Mission Control team.

</p>

 ![Two views of an Astro Pi computer, showing the front panel (with some of the sensors) and the camera.](images/astro-pi-double.png)

## What could you investigate?

With an Astro Pi on board the ISS, there are many different scientific questions you could explore. The sensors and camera allow you to collect real data from space and use it to investigate a topic that interests you. 

To give you some inspiration, here are a few examples of projects created by previous Mission Space Lab teams:

- [Analysing vegetation using NDVI analysis and image stitching](https://esamultimedia.esa.int/docs/edu/aretusa.pdf)
- [Identifying seaweed rafts in the Oceans](https://esamultimedia.esa.int/docs/edu/stmarks.pdf)
- [Measuring the Earth's magenetic field](https://esamultimedia.esa.int/docs/edu/dahspace.pdf)
- [Analysing cloud types](https://esamultimedia.esa.int/docs/edu/t5clouds.pdf)

**Please note:** These projects were designed under previous Mission Space Lab rules, which allowed programs to run for up to 3 hours on the ISS. In the current challenge, your program must run for no more than 10 minutes, so you would need to adapt these ideas to fit within the time limit.

## What you will need

To complete this project, you will need:

- **A computer running Python 3.13 or above.** You can use a Raspberry Pi or any computer that runs Microsoft Windows, macOS, or Linux. You can find [instructions for installing Python here](https://projects.raspberrypi.org/en/projects/generic-python-install-python3). A full description of the Python requirements for Mission Space Lab appears later in this guide.
- **An internet connection.** You will need to access the internet to use the [Astro Pi Replay Tool](https://rpf.io/replay) to simulate your code running live on the Astro Pis on the ISS. You will also need internet access to submit your program.
- **A code editor.** This is where you will write and edit your Python code. Any text editor will work, but we recommend using a dedicated code editor like Thonny. In this guide, our examples will use Thonny.
