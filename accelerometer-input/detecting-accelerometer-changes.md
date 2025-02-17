# Detecting accelerometer changes {#detecting-accelerometer-changes}

To use the accelerometer sensor, instantiate an Accelerometer object and
register for `update` events it dispatches. The `update` event is an
Accelerometer event object. The event has four properties, and each are numbers:

- `accelerationX` — Acceleration along the x-axis, measured in g's. The x-axis
  runs from the left to the right of the device when it is in the upright
  position. (The device is upright when the top of the device is facing up.) The
  acceleration is positive if the device moves toward the right.

- `accelerationY` — Acceleration along the y-axis, measured in g's. The y-axis
  runs from the bottom to the top of the device when it is in the upright
  position. (The device is upright when the top of the device is facing up.) The
  acceleration is positive if the device moves up relative to this axis.

- `accelerationZ` — Acceleration along the z-axis, measured in g's. The Z axis
  runs perpendicular to the face of the device. The acceleration is positive if
  you move the device so that the face of the device points up. The acceleration
  is negative if the face of the device points towards the ground.

- `timestamp` — The number of milliseconds at the time of the event since the
  runtime was initialized.

1 g is the standard acceleration due to gravity, roughly 9.8 m/sec<sup>2</sup>.

Here is a basic example that displays accelerometer data in a text field:

```haxe
import openfl.display.Sprite;
import openfl.events.AccelerometerEvent;
import openfl.sensors.Accelerometer;
import openfl.text.TextField;

class AccelerometerUpdateIntervalExample extends Sprite {
	private var accTextField:TextField;

	public function new() {
		super();

		accTextField = new TextField();
		accTextField.autoSize = LEFT;
		addChild(accTextField);

		var accl:Accelerometer;
		if (Accelerometer.isSupported) {
			accl = new Accelerometer();
			accl.setRequestedUpdateInterval(1000);
			accl.addEventListener(AccelerometerEvent.UPDATE, updateHandler);
		} else {
			accTextField.text = "Accelerometer feature not supported";
		}
	}

	private function updateHandler(evt:AccelerometerEvent):Void {
		accTextField.text = "acceleration X: " + evt.accelerationX + "\n" + "acceleration Y: " + evt.accelerationY + "\n" + "acceleration Z: "
			+ evt.accelerationZ;
	}
}
```

You can adjust the desired time interval for accelerometer events by calling the
`setRequestedUpdateInterval()` method of the Accelerometer object. This method
takes one parameter, `interval`, which is the requested update interval in
milliseconds:

```haxe
var accl:Accelerometer;
accl = new Accelerometer();
accl.setRequestedUpdateInterval(1000);
```

The actual time between accelerometer updates may be greater or lesser than this
value. Any change in the update interval affects all registered listeners. If
you don't call the `setRequestedUpdateInterval()` method, the application
receives updates based on the device's default interval.

Accelerometer data has some degree of inaccuracy. You can use a moving average
of recent data to smooth out the data. For example, the following example
factors recent accelerometer readings with the current reading to get a rounded
result:

```haxe
import openfl.display.Sprite;
import openfl.events.AccelerometerEvent;
import openfl.sensors.Accelerometer;
import openfl.text.TextField;

class AccelerometerMovingAverageExample extends Sprite {
	private static final FACTOR:Float = 0.25;

	private var accTextField:TextField;
	private var rollingX:Float = 0;
	private var rollingY:Float = 0;
	private var rollingZ:Float = 0;

	public function new() {
		super();

		accTextField = new TextField();
		accTextField.autoSize = LEFT;
		addChild(accTextField);

		var accl:Accelerometer;

		if (Accelerometer.isSupported) {
			accl = new Accelerometer();
			accl.setRequestedUpdateInterval(200);
			accl.addEventListener(AccelerometerEvent.UPDATE, updateHandler);
		} else {
			accTextField.text = "Accelerometer feature not supported";
		}
	}

	private function updateHandler(event:AccelerometerEvent):Void {
		accelRollingAvg(event);
		accTextField.text = rollingX + "\n" + rollingY + "\n" + rollingZ + "\n";
	}

	private function accelRollingAvg(event:AccelerometerEvent):Void {
		rollingX = (event.accelerationX * FACTOR) + (rollingX * (1 - FACTOR));
		rollingY = (event.accelerationY * FACTOR) + (rollingY * (1 - FACTOR));
		rollingZ = (event.accelerationZ * FACTOR) + (rollingZ * (1 - FACTOR));
	}
}
```

However, this moving average is only desirable if the accelerometer update
interval is small.
