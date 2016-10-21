---
layout: post
title: Getting Started with Johnny Five and Proteus
summary: Getting Started with Johnny Five and Proteus.
published: false
---

In this tutorial we will learn how to use Proteus Design Suite to simulate an Arduino board and how to use Johnny-Five library to connect to it through a serial port.

Thus, we will need the following programs:
+ [Proteus Design Suite 8.1](http://www.labcenter.com/index.cfm)
+ [com0com](http://sourceforge.net/projects/com0com/)
+ [Arduino IDE](http://www.arduino.cc/)
+ [Node.js](http://nodejs.org/)

To complete our goal, we will build a simple LED blink example (or as someones said, the "Hello world" of the hardware world).

There are two ways to do this: using the existing components that already comes with Proteus software or adding a extra component in Proteus library that represent all the Arduino board. We will see later how to make both ways. Just keep in mind that both ways follow the same logic, changing only one or others aspect in the flow of development. 

Furthermore, in this tutorial, I'm assuming that you already have the Arduino IDE and Node.js softwares installed. 

So, let's getting started!


###What is Proteus Design Suite?
Briefly, Proteus Design Suite is a combination of schematic capture, simulation and design of PCB, that help you to make a complete eletronic circuit. Aside this, Proteus has the capability of simulate populars micro-controllers, executing it’s current firmware.

By this way, Proteus has:
+ **ISIS Schematic Capture** - easy to use but it is an extremely powerful tool to enter your projects. 
+ **PROSPICE Mixed mode SPICE simulation** - industrial standard simulator SPICE3F5, combined with high-speed digital simulator.
+ **ARES PCB Layout** - system for high-performance PCB design with automatic component placement, rip-up and self-routing and verification of interactive design rule. 
+ **VSM** - Virtual System Modelling to simulate embedded software for popular micro-controllers alongside your design of hardware.

Using simulation in the Proteus software, instead of real physics devices, we have some advantages and disadvantages. To list some:

Advantagens:
+ Since Proteus have a extensive library of components, you can try several different components that you do not have on hand.
+ The integrated package with common user interface and the context-sensible help can make the learning process more quick and easy.
+ Virtual prototype with Proteus VSM reduce the time and cost of software and hardware development.
+ Faster and easier to connect the components; not losing in tangles of wires.
+ Do not burn the components, such as LEDs; so common in the learning process.

Disadvantages:
+ Not very exciting and fun as in real life :(

###Why com0com?
com0com is a free program that we will use to create virtual serial ports. These virtual ports will be used to connect the serial port component from Proteus simulation with the node.js application that are using johnny-five. Using com0com we are able to create and configure how much virtual ports that we want. So, we can connect more then one Arduino board or others components in the simulation. We will see later how to properly configure the virtual ports and some useful commands to do this.

> **Note**: You can use other program to create the virtual ports. Just make sure that you are using the correct configuration for the virtual serial ports. This can bring some headache.

###Installing and cofigure the Proteus and com0com softwares
After download Proteus Design Suite 8.1, you can install it in normal way. If everything is ok, it will create the folder of installation like this: 

`C:\Program Files\Labcenter Electronics\Proteus 8 Professional`

Now, let's import the extras Arduino boards components. Download the zip file from this [link](http://goo.gl/f7z3N) and extract it to the following folder:

`C:\ProgramData\Labcenter Electronics\Proteus 8 Professional\LIBRARY`

Nice, now download com0com from [here](http://sourceforge.net/projects/com0com/) and install it. During the instalation process, it will create two virtual serial ports and maybe install the drivers. Let's call these two serial ports 'COM1' and 'COM2' for convenience. After the installation is complete, let's configure it to work with our simulated Arduino board and johnny-five app.

As [@randallagordon](https://github.com/randallagordon) point [here](https://github.com/rwaldron/johnny-five/issues/314#issuecomment-38187208), johnny-five use Firmata protocol to comunicate with Arduino and because this we need set the baud-rate of our serial ports to 57600. To do this, open your command-line and run the follow line:

`mode COM1: baud=57600 parity=n data=8 stop=1`

In this command we are also setting some others configuration to our virtual serial port, like parity and stop bit, just to make sure. Once the definitions for our first serial port is complete, make the same thing for 'COM2'.

> **Note**: You can learn more about the 'mode' command [here](http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/mode.mspx?mfr=true).

If you prefer, you can go to the Control Panel > System and Security > System > Device Manager. Choose the item Ports and select the 'COM1' port. Next, select Action > Properties in the menu (or right click in it and select Properties) and define the correct configuration it.

###Creating our circuit in Proteus
Open the Proteus Design Suite 8.1 and create a new project. Choose "DEFAULT" option for Schematic design and hit next button. In the PCB Layout screen, keep the default "No PCB layout" and hit next. In the Firmware, keep the default "No Firmware Project" and click in next button. In the next screen, revise your options and click in the finish button. This will create a empty project for you with default schematic design. Save it in a new folder like "NodeProteusExample".

Now, let's choose the components that will be used. In the menu, click in the "Library" option and select the "Pick parts from libraries" (or you can use the hotkey "P" - or click in the "P" button at the left painel). This will open a screen to you choose the device for your project. In the "Keyword" input, type "arduino". Next, select the "ARDUINO UNO R3" in the results list (double click to select). Now, type "COMPIM" in the search box again and select the "COMPIM" device listed. The last thing to choose ir our Led device. Type "LED-RED" and select it in the list (you can choose other color if you prefer). Close the Pick Device window. You can see the three devices listed in the Devices painel at left.

Select the Arduino device an place it in the schematic area. Next do the same thing for the serial port and led devices. The last thing that we will need use is an "ground" terminal. You can pick it clicking in the "Terminal Mode" button and select the "Ground" terminal. Place it next to the Led device in the schematic.

Now, let's connect the devices. Click in the TX (1) pin from Arduino and connect it to the TXD (3) pin from serial. Also, connect the RX (0) to the RXD (2). 

Because we will use the pin 13 in our program, let's connect the pin 13 from Arduino to the Led device. To finish, connect the other pin of the Led to the Ground terminal that we placed before.

You can see the image bellow to get the idea:
![place_connect](https://cloud.githubusercontent.com/assets/641525/3139911/c5466f90-e8f8-11e3-9175-a050c88b58a6.gif)


> **Tip**: As your project grows and more components are put, manage the wired connections between the devices can be a little boring. So, you can create the mapping between two terminals by using the "Default" terminal.


###Configuring our components
Now that our components are conected, let's configure it to work accordingly.

Firts, double click at the Serial component and put the baud-rate option to 57600, stop bit to '1', parity to 'none' and so on. The same configuration that we do before to our virtual serial ports. Remember to define the Physical Port to 'COM1'.

![serial_configuration](https://cloud.githubusercontent.com/assets/641525/3139998/5efb016e-e8fe-11e3-99fe-cd19dca14a4c.png)

Now, as the same way that we do when we are working with physical world, we need upload the firmware to the Arduino compoment. 

Open your Aduino IDE and select File > Examples > Firmata > StandartFirmata.

Before compile, just make sure that the "Show verbose" option are enabled. To do this, go to File > Preferences and in the "Show verbose output during" option, check the "compilation" box. 

Now, compile the code. As you can see in the console log at bottom, the code is compiled and the hexadecimal file are generated. Go to the folder that have this file and copy it (something like "C:\Users\user\AppData\Local\Temp\build123.tmp\StandardFirmata.cpp.hex"). Paste it in a local that are easy to find (I suggest to you that put it in the same folder of the Proteus project that we created before). 

Now, double click at Arduino board. In the configuration window of Arduino board that open, in the Firmware option, select the hexadecimal file that we copy. Adjust the other option to match like the image bellow, if necessary:

![arduino_configuration](https://cloud.githubusercontent.com/assets/641525/3140008/9b913ba2-e8fe-11e3-81f6-0607003aa6a1.png)


###Writing the johnny-five example
Now, let's write the Blink example that will do all de magic with our virtual components.

Inside the "NodeProteusExample" that we create before, in your command-line, run the follow command.

`npm install johnny-five`

This will download and install the johnny-five node module inside the "node_modules" subfolder. As you can see, the johnny-five is here.

Next, create a new file "app.js" and write the follow code:
```js
var five = require("johnny-five"),
    board = new five.Board({port:"COM2"});

board.on("ready", function() {
  (new five.Led(13)).strobe(100);
});
```
In this very simple code, we are creating a board that will connect to the our virtual serial port 'COM2' and also creating a Led that will connect to the pin 13 of this board.

###Running the example
Now that all components are created and connected, the configurations are defined and our example are written, let's run it.
First, in Proteus software, start the simulation (at the bottom, hit play). You will see some green dots in serial component change to red and go back to green. The same thing for Arduino component. 
Make sure that all components are correct connected and the simulation is running without problem.

So, now open your command-line in the "NodeProteusExample" folder and run:

`node app.js`

As you can see, the terminal will show that the program is connected to the serial port 'COM2' and starts send commands to it. In Proteus simulation, you can see that the LED component starts blink, as we are waiting!
 
![running](https://cloud.githubusercontent.com/assets/641525/3208936/548ed638-ee4e-11e3-9c1c-1365cd2a1fc7.gif)

The LED component is turnning on and off every 100ms but due the simulation update rate this time can not be so precise.

###Using the components that already comes with Proteus
If you would like use the components that already come with Proteus software, an easy way to start with this is create a new project in Proteus and select the "Arduino Pro 328" in Development Board section. This will create the Arduino board component in your scketch and, as you will see, a new tab is opening and displays the default Arduino code that will be compiled to generate de hexadecial code (firmware) and uploaded to the Arduino board. 

But, since the johnny-five use the Firmata protocol to communicate with Arduino, we need put the StandartFirmata code here to be compiled and uploaded. To do this, open your Arduino IDE and go to File > Examples > Firmata > StandartFirmata. Copy all the code and past in the source code tab in Proteus. 

Now, before hit build button, make sure that you configure the Arduino AVR to compile it. To do this, go to System menu and select Compilers Configuration and then hit Download button in Arduino AVR line. The program will download and install the Arduino IDE. 

Next, press the Build button from the menu (F7) and the StandartFirmata code will be compiled and uploaded to the Arduino component.
Now that your Arduino board is ok put the Serial port and the LED component in the sketch and follow the steps that we do before to run the Blink example.

> **Note**: You can learn more about the Arduino example and how to use the VSMStudio IDE [here](http://labcenter.s3.amazonaws.com/movies/v8/Arduino.cfm). There are also a bunch of snippets that already comes in Proteus that you can use with Arduino component. Check it!


###Robot baby steps
Now that you complete this tutorial the world of simulation is yours. Try reproduce the examples from the johnny-five repository in the Proteus simulation or create something new and share with us. You can also do the opposite, start with examples that comes with Proteus software and reproduce it with real physical compoments that are connected with your johnny-five.

I'm pretty sure that great samples will be comming. Have fun!


###Source code
All the source code can be found [here](https://github.com/rodrigopandini/NodeProteusExample). After download, run the command `npm install` to install the johnny-five module and all dependencies.
