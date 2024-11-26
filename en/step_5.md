## Improving the accuracy of your program

The average speed of the ISS is not a secret, but the ISS's speed is not really constant and there are several factors that can affect it, for example the altitude. If the altitude has changed, then the ISS must have fired its boosters and therefore the speed must have changed. To increase your chances of getting your program run on the ISS, ask yourself if your program is sensitive enough to the subtle changes that will affect the speed that the ISS is travelling.

On the [N2YO.com website](https://www.n2yo.com/?s=25544), you can see live data of the ISS which shows how its altitude changes during orbit. You can also see when the ISS will next be passing over your location.

### Averages 

If your program calculates multiple estimates for the speed of the ISS (for example, by calculating the speed from sequences of two photos), then you will need to decide how to reduce these estimates into a single number when writing your `result.txt` file. If you used a simple average ([mean](https://en.wikipedia.org/wiki/Mean)), could you explore the accuracy of other statistical measures, such as the median and other percentiles?

There is a lot of scope for being creative when improving the accuracy of your estimate. One method is to be selective about which photos or data you use to calculate your estimate. If you can determine that a specific sequence of data is the most reliable, then you could weight this data more highly in your final estimate.


<p style="border-left: solid; border-width:10px; border-color: #0faeb0; background-color: aliceblue; padding: 10px;">
  
Be cautious about training your program to be oversensitive to the exact sequence shown when using Astro Pi Replay — the sequence on the ISS will be different, and you want your program to be accurate on the ISS most of all!

</p>

If your method of calculating the speed is based on the [Calculate the speed of the ISS using photos](https://projects.raspberrypi.org/en/projects/astropi-iss-speed/0) project, then perhaps you could use techniques from computer vision or machine learning to classify photos that are easier to estimate from. For example, you could perform machine learning inference in real time to evaluate the accuracy of your estimate, or the conditions below the ISS. 

