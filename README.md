# Lemonbar node.js bindings
This is a library around [Lemonboy's Bar](https://github.com/LemonBoy/bar), which lets you easily make use of the great formatting functionalities of the Lemonbar, *together* with node.js which is ideal for event-based processing.

## Launching Lemonbar
```javascript
var lb = require("lemonbar");
lb.launch({
    
    lemonbar: "/usr/bin/lemonbar",         // Lemonbar binary
    shell: "/bin/sh",                      // Shell to use for actions
    shelloutput: true,                     // Print shell STDOUT

    // More information on the following arguments at https://github.com/LemonBoy/bar#options

    background: "#888",                    // -B    Background color (#rgb, #rrggbb, #aarrggbb)
    foreground: "#333",                    // -F    Foreground color
    lineWidth: 1, lineColor: "#666",       // -[Uu] Underline/Overline config
    geometry: {                            // -g    Window geometry
        x: 0, y: 0,
        width: null, height: 50
    },
    fonts: ["Arial-10", "FontAwesome-10"], // -f    Load fonts
    
    bottom: false,                         // -b    Dock bar at bottom instead of top
    forceDocking: false,                   // -d    Force docking without asking the window manager
    name: null,                            // -n    Set the WM_NAME atom value for the bar
    areas: 10                              // -a    Number of clickable areas

})
```

## Writing to the Lemonbar

#### `lb.append(<text>)`
Append *text* to the buffer for the current line.

#### `lb.write([text])`
Send the current line plus *text* (if given) to Lemonbar, and thus updates the current view.

#### Example
[Show a more complete example](https://github.com/moqmar/node-lemonbar/blob/master/example.js)

```javascript
var lb = require("lemonbar"); lb.launch();

lb.append("Hello World".lbCenter)
lb.write("It's ".lbRight + (" " + new Date().toString() + " ").lbSwap)
```

## Formatting
Formatting is handled via the String prototype. You have the following helper functions:

#### `"text".lbSwap`
Swap the current background and foreground colors.

#### `"text".lbLeft`, `lb.left`
Aligns the following text to the left side of the screen.

#### `"text".lbCenter`, `lb.center`
Aligns the following text to the center of the screen.

#### `"text".lbRight`, `lb.right`
Aligns the following text to the right side of the screen.

#### `"text".lbUnderline`
Draw a line under the text.

#### `"text".lbOverline`
Draw a line over the text.

#### `"text".lbBg(<color>)`
Set the text background color. The parameter *color* has to be a color in one of the formats lemonbar accepts (`#rgb`, `#rrggbb`, `#aarrggbb`).

#### `"text".lbFg(<color>)`
Set the text foreground color. The parameter *color* has to be a color in one of the formats lemonbar accepts (`#rgb`, `#rrggbb`, `#aarrggbb`).

#### `"text".lbFont(<font>)`
Set the font used to draw the following text. The parameter *font* has to be the exact name (with all XCB parameters) of the font as given in the `fonts` lemonbar parameter.

#### `"text".lbLinecolor(<color>)`
Set the text underline color. The parameter *color* has to be a color in one of the formats lemonbar accepts (`#rgb`, `#rrggbb`, `#aarrggbb`).

#### `"text".lbAction(<command>, [button])`
Create a clickable area, when the area is clicked *command* is sent to *shell*.

The *button* field is optional, it defaults to the left button, and it's a number ranging from 1 to 5 which maps to the left, middle, right, scroll up and scroll down movements. Your mileage may vary.  
You may want to use the more descriptive values `lb.BUTTON_{LEFT|RIGHT|MIDDLE|SCROLLUP|SCROLLDOWN}`.

Nested clickable areas can trigger different commands:

    "shutdown".lbAction("poweroff").lbAction("reboot", lb.BUTTON_RIGHT)

#### `"text".lbMonitor(<monitor>)`, `lb.monitor`
Change the monitor the bar is rendered to. *monitor* can be either:

- `next`, `previous`: Next/previous monitor.
- `first`, `last`: First/last monitor.
- `0-9`: Nth monitor.
