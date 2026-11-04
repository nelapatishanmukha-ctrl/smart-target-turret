Smart Turret (Arduino Nano + Processing)
A turret that follows a red object. A laptop webcam finds the red colour with a
Processing sketch. The sketch sends the position over USB (COM4) to an Arduino Nano.
The Nano moves two servos (pan and tilt) and switches a laser ON through a relay.
![circuit](docs/circuit_diagram.png)
Parts
Part	Qty
Arduino Nano (ATmega328P)	1
SG90 servo motor	2 (pan + tilt)
5V relay module (1 channel)	1
Laser module, 5V	1
External 5V 2A power supply	1
Jumper wires, breadboard, USB cable	-
Laptop with webcam	1
Wiring
From	To
Nano D9	Pan servo signal (orange)
Nano D10	Tilt servo signal (orange)
Nano D7	Relay IN
Nano GND	Common GND
5V supply +	Servo red wires, Relay VCC, Relay COM
Relay NO	Laser + (red)
Laser - (black)	GND
5V supply -	Common GND
All GND must be joined together (Nano, supply, servos, relay, laser).
No external supply? You can power the servos and relay from the Nano 5V pin. It can work
with small SG90 servos, but the Nano may restart when the servos pull current. An external
5V supply is better.
How to run
Open `arduino/smart_turret/smart_turret.ino` in Arduino IDE.
Board: Arduino Nano. Processor: ATmega328P (try Old Bootloader if upload fails).
Upload it.
Close the Arduino Serial Monitor (it blocks the COM port).
Open `processing/smart_turret/smart_turret.pde` in Processing 4.
Install the Video library: Sketch > Import Library > Manage Libraries > Video.
Check `PORT_NAME` at the top of the sketch (default `"COM4"`).
Press Run. Show a red object to the camera. The green crosshair follows it and the laser turns ON.
Keys in the Processing window
Key	What it does
Arrow keys	Fine-tune aim (so the laser lands on the target)
X / Y	Swap pan / tilt direction if the turret moves the wrong way
A	Arm / disarm the laser
M	Show detected red pixels (yellow dots)
+ / -	Red detection stricter / looser
C	Reset trim
R	Reconnect to the COM port
H	Hide / show help
Settings you may want to change
`smart_turret.ino`: pins (`PAN_PIN`, `TILT_PIN`, `RELAY_PIN`), `RELAY_ACTIVE_LOW`, angle limits.
`smart_turret.pde`: `PORT_NAME`, `CAM_INDEX`, `PAN_MIN/MAX`, `TILT_MIN/MAX`.
How the two programs talk
Processing sends a line of text about 30 times per second:
```
pan,tilt,laser\n        e.g.  95,82,1
```
If the Nano gets no data for 1 second, it switches the laser OFF.
Safety
A laser can hurt eyes. Never point it at people or animals. Use a low-power module.
