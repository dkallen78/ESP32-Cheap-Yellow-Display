# Hello World

Nearly every new foray into programming starts with the venerable Hello World program, why should the CYD be any different?

<img src="cyd-hello-world-output.jpg" width="800" alt="CYD displaying the text Hello World">

Let's take a look at the code to see what's going on.

First up is the library to help us interact with the visual (not the tactile) aspect of the display:

```c++
#include <TFT_eSPI.h>
```

TFT refers to the type of display we're using: **t**hin **f**ilm **t**ransistor. Most of the LCD displays in use today use this technology. The other part of our library name is "eSPI" which means **e**nhanced **s**erial **p**eripheral **i**nterface. eSPI is a protocol to allow communication between your CPU and a peripheral device like your display. If you want to get into the weeds of how it works, [check out the GitHub page](https://github.com/Bodmer/TFT_eSPI)!