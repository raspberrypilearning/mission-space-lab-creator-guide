## Submitting your work

If you are happy with your program and have run it using Astro Pi Replay, have read the Mission Space Lab rulebook on the Astro Pi website, and have checked and double-checked the Mission Space Lab program checklist, all that is left to do is to zip up your work and ask your team mentor to submit it to us.

### Preparing a zip file

If you do not know how to compress your project folder into a zip file, speak to your mentor, who will be able to tell you the correct process for your operating system.

### Removing unnecessary files

Your zip file must not be more than 3MB in size, unless it includes a .tflite model, in which case it is allowed to be up to 7MB in size to accommodate the size of the model. This means, for example, that you cannot supply your own ephemeris files (e.g. [de421.bsp](https://naif.jpl.nasa.gov/pub/naif/generic_kernels/spk/planets/a_old_versions/de421.bsp) or [de440s.bsp](https://naif.jpl.nasa.gov/pub/naif/generic_kernels/spk/planets/de440s.bsp)), since they are above the size limit.

Both `de421.bsp` and `de440s.bsp` files are available on the Astro Pis. If your program needs them, you can access them via the included `orbit` library, as seen in the following code:

```Python
from orbit import de421, de440s
print(de440)
print(de440s)
```

You will need to run this snippet using the Astro Pi Replay tool in order to access the `orbit` library.

--- task ---

Generate a zip file for your project, and ask your mentor to submit it to us before the deadline.

--- /task --- 
