## Submitting your work

If you are happy with your program and have run it using Astro Pi Replay, have read the guidelines on the Astro Pi website, and have checked and double-checked the MSL program checklist, all that remains to be done is to zip up your work and get your mentor to submit it to us.

### Preparing a zip 

If you do not know how to compress your project folder into a zip file, speak to your mentor, who will be able to tell you the correct process for your operating system.

### Removing unnecessary files

Your zip file must not be more than 3 MB in size, unless it includes a .tflite model, in which case it is allowed to be up to 7 MB in size to accommodate the size of the model. This means, for example, that you cannot supply your own ephemeris files (e.g. [de421.bsp](https://naif.jpl.nasa.gov/pub/naif/generic_kernels/spk/planets/a_old_versions/de421.bsp) or [de440s.bsp](https://naif.jpl.nasa.gov/pub/naif/generic_kernels/spk/planets/de440s.bsp)) since they are above the size limit.

Both `de421.bsp` and `de440s.bsp` files are available on the Astro Pis. If your program needs them, you can access them via the `orbit` library, as seen in the following code:

add code block

--- task ---

Generate a zip for your project, and get your mentor to submit it to us before the deadline.

--- /task --- 
