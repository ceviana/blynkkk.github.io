#Widgets
Widgets are interface modules. Each of them performs a specific input/ output function when communicating with the hardware.

There are 4 types of Widgets: 

- **Controllers** - they send commands to hardware. Use them to control your stuff;
- **Displays** - used for various visualizations of data that comes from hardware to the smartphone;
- **Notifications** - are various widgets to send messages and notifications;
- **Others** - widgets that don't belong to any category;

Each Widget has it's own settings. 

Some of the Widgets (e.g. E-mail Widget) are used to enable some functionality and they don't have any settings.
 
## Common Widget Settings
### Pin Selector
This is one of the main parameters you need to set. It defines which pin to control or to read from. 

<img src="images/pin_selection.png" style="width: 200px; height:360px"/>

**Digital Pins** - represent physical Digital IO pins on your hardware. PWM-enabled pins are marked with the ```~``` symbol

**Digital Pins** - represent physical Analog IO pins on your hardware

**Virtual Pins** - have no physical representation. They are used for any data transfer between Blynk App and your hardware.
Read more about Virtual Pins [here](http://docs.blynk.cc/#blynk-main-operations-virtual-pins).

### Data Mapping

In case you want to map incoming values to specific range you may use mapping button : 

<img src="images/display_edit_mapping.png" style="width: 200px; height:360px"/>

Let's say your sensor sends values from 0 to 1023. But you want to display values in a range 0 to 100 in the application. With data mapping enabled, incoming value 1023 will be mapped to 100.

### SPLIT/MERGE
Some of the Widgets can send more than one value. And with this switch you can control how to send them.

<img src="images/split_merge.gif" style="width: 300px; height:280px"/>


- **SPLIT**:
Each of the parameters is sent directly to the Pin on your hardware (e.g D7). You don't need to write any code.

	**NOTE:** In this mode you send multiple commands from one widget, which can reduce performance of your hardware.

	Example: If you have a Joystick Widget and it's set to D3 and D4, it will send 2 commands over the Internet:

	```cpp
	digitalWrite(3, value);
	digitalWrite(4, value);
```

- **MERGE**:
When MERGE mode is selected, you are sending just 1 message, consisting of array of values. But you'll need to parse it on the hardware. 

	This mode can be used with Virtual Pins only.
	
	Example: Add a zeRGBa Widget and set it to MERGE mode. Choose Virtual Pin V1
	
	```cpp
	BLYNK_WRITE(V1) // There is a Widget that WRITEs data to V1 
	{
	  int r = param[0].asInt(); // get a RED channel value
	  int g = param[1].asInt(); // get a GREEN channel value
	  int b = param[2].asInt(); // get a BLUE channel value
	}
```

##Controllers
### Button
Works in push or switch modes. Allows to send 0/1 (LOW/HIGH) values.

<img src="images/button.png" style="width: 77px; height:80px"/>

<img src="images/button_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino#L48)

### Slider
Similar to potentiometer. Allows to send values between MIN and MAX.

<img src="images/slider.png" style="width: 77px; height:80px"/>

<img src="images/slider_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino#L48)

### Timer
Timer triggers actions at a specific time. Even if smartphone is offline. Start time sends 1 (HIGH). Stop time sends 0 (LOW).

<img src="images/timer.png" style="width: 77px; height:80px"/>

<img src="images/timer_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [Timer](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Timer/Timer.ino#L16)

### Joystick
Control servo movements in 4 directions

####Settings:
- SPLIT/MERGE modes - read [here](http://docs.blynk.cc/#widgets-common-widget-settings-splitmerge)

- **Rotate on Tilt**

When it's ON, Joystck will automatically rotate if you use your smartphone in landscape orientation  
- **Auto-Return**
- 
When it's OFF, Joystick handle will not return back to center position. It will stay where you left it. 

<img src="images/joystick.png" style="width: 77px; height:80px"/>

<img src="images/joystick_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [JoystickTwoAxis](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/JoystickTwoAxis/JoystickTwoAxis.ino#L24)

##Displays
### Value Display
Displays incoming data from your sensors or Virtual Pins.

<img src="images/display.png" style="width: 77px; height:80px"/> 

<img src="images/display_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino#L48)

### LED
A simple LED for indication. You need to send 0 in order to turn LED off. And 255 in order to turn LED on. Or just use
Blynk API as described below :

```cpp
WidgetLED led1(V1); //register to virtual pin 1
led1.off();
led1.on();
```
    
All values between 0 and 255 will change LED brightness :

```cpp
WidgetLED led2(2);
led2.setValue(127); //set brightness of LED to 50%.
```

<img src="images/led.png" style="width: 77px; height:80px"/>

<img src="images/led_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [LED](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/LED/LED.ino#L31)

### Gauge
A great visual way to display incoming numeric values.

<img src="images/gauge.png" style="width: 77px; height:80px"/>

<img src="images/gauge_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino#L48)

### LCD
This is a regular 16x2 LCD display made in our secret facility in China.
#### SIMPLE / ADVANCED MODE

#### Commands
You need to use special commands with this widget:

```
lcd.print(x, y, "Your Message");
```
Where x is a symbol position (0-15), y is a line number (0 or 1), 

```
lcd.clear();
```

<img src="images/lcd.png" style="width: 77px; height:80px"/>

<img src="images/lcd_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [LCD](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/LCD/LCD.ino#L22)

### Graph
Easily plot incoming data from your project in various designs.

<img src="images/graph.png" style="width: 77px; height:80px"/>

<img src="images/graph_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [BlynkBlink](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/BlynkBlink/BlynkBlink.ino#L48)

### History Graph
Allows you to see any data your hardware had sent to server previously. History graph has 3 granularities :

- Minute granularity - ```1h```, ```6h```;
- Hour granularity - ```1d```, ```1w```;
- Day granularity - ```1m```, ```3m```;

This means that minimum graph update interval is 1 minute for ```1h``` and ```6h``` periods. 
1 hour for ```1d``` and ```1w``` periods. 1 day for ```1m``` and ```3m``` periods. 

<img src="images/history_graph.png" style="width: 77px; height:80px"/>

<img src="images/history_graph_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [PushData](https://github.com/blynkkk/blynk-library/blob/master/examples/GettingStarted/PushData/PushData.ino#L58)

### Terminal
Display data from your hardware. Allows also to send any string to your hardware.

You need to use special commands with this widget:

```
terminal.print();
terminal.flush();
```

<img src="images/terminal.png" style="width: 77px; height:80px"/>

<img src="images/terminal_edit.png" style="width: 200px; height:360px"/>

**Sketch:** [Terminal](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Terminal/Terminal.ino#L23)

##Notifications
###Twitter

Twitter widget connects your Twitter account to Blynk and allows you to send Tweets from your hardware.

<img src="http://static1.squarespace.com/static/54765ba7e4b0d055ee0b47a6/54e92d39e4b0c31341b33a9a/55813d09e4b0ba8aa77ab230/1434533129525/TwitterON.png" style="width: 77px; height:80px"/>

<img src="images/twitter_edit.png" style="width: 200px; height:360px"/>

Example code:
```cpp
Blynk.tweet("Hey, Blynkers! My Arduino can tweet now!");
```

Limitations :

- you cant' send 2 tweets with same message (it's Twitter policy)
- only 1 tweet per minute is allowed

**Sketch:** [Twitter](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Twitter/Twitter.ino#L26)

###Email

Email widget allows you to send email from your hardware to any address.

<img src="images/mail.png" style="width: 77px; height:80px"/>

<img src="images/mail_edit.png" style="width: 200px; height:360px"/>

Example code:
```cpp
Blynk.email("my_email@example.com", "Subject", "Your message goes here");
```

Limitations :

- only 1 email per minute is allowed

**Sketch:** [Email](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Email/Email.ino#L26)

###Push Notifications

Push Notification widget allows you to send push notification from your hardware to your device. Currently it also
 contains 2 additional options :

- "Notify when hardware offline" - you will get push notification in case your hardware went offline.
- "Priority" - high priority gives more chances that your message will be delivered without any delays. 
See detailed explanation [here](https://developers.google.com/cloud-messaging/concept-options#setting-the-priority-of-a-message). 

WARNING : high priority contributes more to battery drain compared to normal priority messages.

<img src="images/push.png" style="width: 77px; height:80px"/>

<img src="images/push_edit.png" style="width: 200px; height:360px"/>

Example code:
```cpp
Blynk.notify("Hey, Blynkers! My hardware can push now!");
```

Limitations :

- maximum allowed body length is 255 symbols.
- only 1 notification per minute is allowed

**Sketch:** [PushNotification](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/PushNotification/PushNotification.ino#L26)

## Other
### Bridge

Bridge can be used for Device-to-Device communication (no app. involved). You can send digital/analog/virtual write commands from one device to another, knowing it's auth token.
At the moment Bridge widget is not required on application side (it is mostly used for indication that we have such feature).  
**And yes, you can use multiple bridges to control multiple devices.**

<img src="images/bridge.png" style="width: 77px; height:80px"/>

<img src="images/bridge_edit.png" style="width: 200px; height:360px"/>

Example code:
```cpp
WidgetBridge bridge1(V1); //Bridge widget on virtual pin 1
...
void setup() {
    Blynk.begin(...);
    while (Blynk.connect() == false) {
        // Wait until Blynk is connected
    }
    bridge1.digitalWrite(9, HIGH);
    bridge1.analogWrite(10, 123);
    bridge1.virtualWrite(V1, "hello");
    bridge1.virtualWrite(V2, "value1", "value2", "value3");
}

BLYNK_CONNECTED() {
  bridge1.setAuthToken("OtherAuthToken");
}
```

WARNING: Bridge widget takes a virtual pin, and turns it into a channel to control another device. It means you can control any virtual, digital or analog pins of the target device.
Be careful not to use pins like ```A0, A1, A2 ...``` when communicating between different device types, as Ardiono Core may refer to wrong pins in such cases.
Also have in mind ```bridge.virtualWrite``` doesn't send any value to mobile application, in order to do that you need call ```Blynk.virtualWrite```.

**Sketch:** [Bridge](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/Bridge/Bridge.ino#L33)

### RTC

Realtime clock allows you to get time from server.

**Sketch:** [RTC](https://github.com/blynkkk/blynk-library/blob/master/examples/Widgets/RTC/RTC.ino)
