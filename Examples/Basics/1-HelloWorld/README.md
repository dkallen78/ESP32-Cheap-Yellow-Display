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

Next we have the `setup()` function that runs one time when we power up our CYD. 

```c++
void setup() {
  ...
}
```

Everything that comes before this is considered global and is available in the scope of both the `setup()` and `loop()` functions. Likewise, anything inside the `setup()` function is local to that function and isn't available in the `loop()` function. So, you could put the `TFT_eSPI tft = TFT_eSPI();` line inside `setup()` and the program would still work the same, but, you wouldn't be able to update the display later on because `loop()` wouldn't have access to it.

```c++
tft.init();
``` 

This line initializes the display and gets it ready to do stuff. Your code will compile fine without this line, but without being setup to recieve instructions, your screen won't display anything.

```c++
tft.setRotation(1);
``` 

This orients the screen in landscape with the USB-C port on the right. The parameter can be set between 0 and 3. A value of 0 will give you a portrait orientation with the USB-C port at the bottom. A value of 2 is portrait orientation with the USB-C port at the top, and 3 is landscape with the port on the left.

```c++
tft.fillScreen(TFT_BLACK);
```

This line is pretty self-explanatory.  makes the entire screen black. You can leave this out if you want but it won't look as pretty. The TFT_eSPI.h library has [24 predefined colors you can use](https://github.com/Bodmer/TFT_eSPI/blob/5793878d24161c1ed23ccb136f8564f332506d53/TFT_eSPI.h#L305) or you can input your own. If you do want to use custom colors, be advised that the CYD display only supports 16-bit RGB, not the 24-bit RGB you're likely used to. That means that you'll only have 65,536 colors to work with instead of the 16,777,216 available on the web. You'll also have to input that value as a four-digit hexadecimal number, so red would be `0xF800` and blue would be `0X001F`. Also note, that it doesn't matter if the letters in your hex number are upper case or lower case: `0x001F` is the same as `0x001f`. Check out [this page](https://rgbcolorpicker.com/565) for a good color picker.

```c++
tft.setTextColor(TFT_WHITE, TFT_BLACK);
```
This establishes the foreground and background color of the text to be displayed. The first argument is the text color and the second argument is the background color. If you're feeling lazy, you don't have to set a background color and it will default to transparent.

```c++
int x = 5;
int y = 10;
int fontNum = 2;
```

These three lines make three variables we'll use to set the x and y position of where we want our text to start from, and set which of the available fonts to use. They'll show up in the next line.

```c++
tft.drawString("Hello", x, y, fontNum); // Left Aligned
```

This line draws the text on the screen. It takes four arguments: the text to display, the x and y position of where to start the text, and which font to use. Keep in mind that the coordinate system used by the CYD starts in the top left corner (0, 0) and extends to the bottom right corner (319, 239). Negative numbers are allowed, as are numbers that are "outside" of the display, but those pixels won't be rendered.

```c++
 x = 320 /2;
y += 16;
```

These lines are changing the x and y values in anticipation of the next draw command.

```c++
tft.setTextColor(TFT_BLUE, TFT_BLACK);
```

Changes the text color to blue and keeps the background color the same.

```c++
tft.drawCentreString("World", x, y, fontNum);
```

This line will draw the text centered on the given x value, otherwise it works the same as `drawString()`. *Technically*, according to the TFT_eSPI documentation, `drawCentreString()` is a deprecated function. What we should use instead is a function called `setTextDatum()` which will give us more control over which direction and from which point our text will be rendered. To achieve the same thing, `drawCentreString()` should be replaced with two lines:

```c++
tft.setTextDatum(TC_DATUM);
tft.drawString("World", x, y, fontNum);
```
`TC_DATUM` is a constant in the TFT_eSPI library and is short for **t**op **c**enter which refers how the text should be rendered. If we changed the argument to `TR_DATUM` our text would render to the left of our given x value, making it right aligned. You can see all the possible arguments for `setTextDatum()` [here](https://github.com/Bodmer/TFT_eSPI/blob/5793878d24161c1ed23ccb136f8564f332506d53/TFT_eSPI.h#L281).