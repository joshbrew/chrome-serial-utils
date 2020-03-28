# chrome-serial-utils

Chrome Serial Utilities for Chrome Extensions, simple class you can call as an instance of chrome's serial monitor and connect to devices or send/receive information using the console. Originally by Diego Schmaedech (MIT License) and generalized by Joshua Brewster (MIT License).

Comes with a default UI you can use to test by just calling the class with default parameters for use in your chrome extension.

## Usage example:

#### HTML file serialExample.html:

```
<!DOCTYPE html>
<html>
  <head>
    <script src="serialUtils.js"></script>
  </head>
  
  <body>
    <h3>Check your console by right clicking with "Inspect"</h3>
    <div id="serialmenu"></div>
    <script src="serialExample.js"></script>
  </body>
</html>
```

#### JS File serialExample.js:

There is a finalCallback() function you can customize to update your UI upon successful connection.

```
var serialMonitor = new chromeSerial(true,"serialmenu"); //Initialize with defaultUI set to true and the parent id to append it to as "serialmenu"

serialMonitor.finalCallback = () => { //Set this so USB devices bind to the interface once connected.

  serialMonitor.onReadLine = (line) => { //Connect the serial monitor data to the session handler
      console.log("RECEIVED: ", line); //Check your console by right clicking with "Inspect"
    }
  }
}
```

#### Chrome extension launch.js:

```
chrome.app.runtime.onLaunched.addListener(function () {
    chrome.app.window.create('serialExample.html', {
        'bounds': {
            'width': 1280,
            'height': 800
        }
    }); 
});
```

#### Chrome extension manifest.json

The fullscreen and storage permissions are not required for the utilities but are still useful.

```
{
    "name": "Chrome Serial Monitor",
    "version": "0.1",
    "description": "Chrome Serial Monitor, allows your console to become a simple serial monitor.",
    "short_name": "Chrome Serial",
    "permissions": [
        "serial", 
        "webview", 
        "fullscreen",
        "storage" 
    ],
    "minimum_chrome_version": "46",
    "app": {
        "background": {
            "scripts": [
                "launch.js"
            ]
        }
    } 
  }
```
