# Hello World

Nearly every new foray into programming starts with the venerable Hello World program, why should the CYD be any different?

<img src="cyd-hello-world-output.jpg" width="800" alt="CYD displaying the text Hello World">

Let's take a look at the code to see what's going on.

First up is the library to help us interact with the visual (not the tactile) aspect of the display:

```c++
#include <TFT_eSPI.h>
```

TFT refers to the type of display we're using: **t**hin **f**ilm **t**ransistor. Most of the LCD displays in use today use this technology. The other part of our library name is "eSPI" which means **e**nhanced **s**erial **p**eripheral **i**nterface. eSPI is a protocol to allow communication between your CPU and a peripheral device like your display. If you want to get into the weeds of how this library works, [check out its GitHub page](https://github.com/Bodmer/TFT_eSPI)!

This library gives us a set of commands to make manipulating the pixels on the screen easier.

Next, we tap into that library we just included and create an object called `tft` to weild the power of the TFT_eSPI library.

```c++
TFT_eSPI tft = TFT_eSPI();
```

The bit at the beginning, `TFT_eSPI`, is a class defined in the library we included. `tft` is the name of the object we'll be using. The `TFT_eSPI` part has to be exactly as it's written but we can change the variable name to anything we want. The final part, `TFT_eSPI()`, is a constructor and it's going to set up our `tft` object with all of the values it needs, many of which will come from the custom User_Setup.h file we added to our library earlier.

Next we have the `setup()` function that runs one time when we power up our CYD. Everything that comes before this is considered global and is available in the scope of both the `setup()` and `loop()` functions. Likewise, anything inside the `setup()` function is local to that function and isn't available in the `loop()` function. So, you could put the `TFT_eSPI tft = TFT_eSPI();` line inside `setup()` and the program would still work the same, but, you wouldn't be able to update the display later on because `loop()` wouldn't have access to it.

The line `tft.init();` initializes the display and gets it ready to do stuff. Your code will compile fine without this line but without being setup to recieve instructions, your screen won't display anything.

`tft.setRotation(1);` orients the screen in landscape with the USB-C port on the right. This value can be set between 0 and 3. A value of 0 will give you a portrait orientation with the USB-C port at the bottom. A value of 2 is portrait orientation with the USB-C port at the top, and 3 is landscape with the port on the left.

The next line is pretty self-explanatory. `tft.fillScreen(TFT_BLACK);` makes the entire screen black. You can leave this out if you want but it won't look as pretty. The TFT_eSPI.h library has [24 predefined colors you can use](https://github.com/Bodmer/TFT_eSPI/blob/5793878d24161c1ed23ccb136f8564f332506d53/TFT_eSPI.h#L305) or you can input your own. If you do want to use custom colors, be advised that the CYD display only supports 16-bit RGB, not the 24-bit RGB you're likely used to. That means that you'll only have 65,536 colors to work with instead of the 16,777,216 available on the web. You'll also have to input that value as a four-digit hexadecimal number, so red would be `0xF800` and blue would be `0X001F`. Also note, that it doesn't matter if the letters in your hex number are upper case or lower case: `0x001F` is the same as `0x001f`. Check out [this page](https://rgbcolorpicker.com/565) for a good color picker.

