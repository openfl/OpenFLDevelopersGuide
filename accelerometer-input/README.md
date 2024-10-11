# Chapter 34: Accelerometer input {#chapter-34-accelerometer-input}

The Accelerometer class dispatches events based on activity detected by the
device's motion sensor. This data represents the device's location or movement
along a three-dimensional axis. When the device moves, the sensor detects this
movement and returns the acceleration coordinates of the device. The
Accelerometer class provides methods to query whether accelerometer is
supported, and also to set the rate at which acceleration events are dispatched.

The accelerometer axes are normalized to the display orientation, not the
physical orientation of the device. When the device re-orients the display, the
accelerometer axes are re-oriented as well. Thus, the y-axis is always roughly
vertical when the user is holding the phone in a normal, upright viewing
position — no matter which way the phone is rotated. If auto-orientation is off,
for example, when OpenFL content in a browser is in full-screen mode, then the
accelerometer axes are not re-oriented as the device is rotated.

## Checking accelerometer support

Use the `Accelerometer.isSupported` property to test the runtime environment for
the ability to use this feature:

```haxe
import openfl.display.Sprite;
import openfl.sensors.Accelerometer;

class AccelerometerSupportedExample extends Sprite {
	public function new() {
		super();

		if (Accelerometer.isSupported) {
			// Set up Accelerometer event listeners and code.
		}
	}
}
```

The Accelerometer class and its members are accessible to all OpenFL targets.
However, the current environment at run time determines the availability of this
feature. For example, you can compile code using the Accelerometer class
properties for desktop, mobile, and web, but you need to use the
`Accelerometer.isSupported` property to test for the availability of the
Accelerometer feature on the user's device. If `Accelerometer.isSupported` is
`true` at runtime, then Accelerometer support currently exists.

## Section Contents

- [Detecting accelerometer changes](./detecting-accelerometer-changes.md)

## More Help Topics

- [openfl.sensors.Accelerometer](https://api.openfl.org/openfl/sensors/Accelerometer.html)
- [openfl.events.AccelerometerEvent](https://api.openfl.org/openfl/events/AccelerometerEvent.html)
