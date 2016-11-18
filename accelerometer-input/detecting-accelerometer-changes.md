# Detecting accelerometer changes {#detecting-accelerometer-changes}

To use the accelerometer sensor, instantiate an Accelerometer object and register for update events it dispatches. The

update event is an Accelerometer event object. The event has four properties, and each are numbers:

• accelerationX—Acceleration along the x-axis, measured in g’s. The x-axis runs from the left to the right of the device when it is in the upright position. (The device is upright when the top of the device is facing up.) The acceleration is positive if the device moves toward the right.

• accelerationY—Acceleration along the y-axis, measured in g’s. The y-axis runs from the bottom to the top of the device when it is in the upright position. (The device is upright when the top of the device is facing up.) The acceleration is positive if the device moves up relative to this axis.

• accelerationZ—Acceleration along the z-axis, measured in g’s. The Z axis runs perpendicular to the face of the device. The acceleration is positive if you move the device so that the face of the device points up. The acceleration is negative if the face of the device points towards the ground.

• timestamp—The number of milliseconds at the time of the event since the runtime was initialized. 1 g is the standard acceleration due to gravity, roughly 9.8 m/sec2..

Here is a basic example that displays accelerometer data in a text field:

var accl:Accelerometer;

if (Accelerometer.isSupported)

{

accl = new Accelerometer(); accl.addEventListener(AccelerometerEvent.UPDATE, updateHandler);

}

else

{

accTextField.text = &quot;Accelerometer feature not supported&quot;;

}

function updateHandler(evt:AccelerometerEvent):void

{

accTextField.text = &quot;acceleration X: &quot; + evt.accelerationX.toString() + &quot;\n&quot;

+ &quot;acceleration Y: &quot; + evt.accelerationY.toString() + &quot;\n&quot;

+ &quot;acceleration Z: &quot; + evt.accelerationZ.toString()

}

To use this example, be sure to create the accTextField text field and add it to the display list before using this code. You can adjust the desired time interval for accelerometer events by calling the setRequestedUpdateInterval()

method of the Accelerometer object. This method takes one parameter, interval, which is the requested update

interval in milliseconds:

var accl:Accelerometer; accl = new Accelerometer();

accl.setRequestedUpdateInterval(1000);

The actual time between accelerometer updates may be greater or lesser than this value. Any change in the update interval affects all registered listeners. If you don’t call the setRequestedUpdateInterval() method, the application receives updates based on the device&#039;s default interval.

Accelerometer data has some degree of inaccuracy. You can use a moving average of recent data to smooth out the data. For example, the following example factors recent accelerometer readings with the current reading to get a rounded result:

var accl:Accelerometer; var rollingX:Number = 0; var rollingY:Number = 0; var rollingZ:Number = 0;

const FACTOR:Number = 0.25;

if (Accelerometer.isSupported)

{

accl = new Accelerometer(); accl.setRequestedUpdateInterval(200); accl.addEventListener(AccelerometerEvent.UPDATE, updateHandler);

}

else

{

accTextField.text = &quot;Accelerometer feature not supported&quot;;

}

function updateHandler(event:AccelerometerEvent):void

{

accelRollingAvg(event);

accTextField.text = rollingX + &quot;\n&quot; + rollingY + &quot;\n&quot; + rollingZ + &quot;\n&quot;;

}

function accelRollingAvg(event:AccelerometerEvent):void

{

rollingX = (event.accelerationX * FACTOR) + (rollingX * (1 - FACTOR)); rollingY = (event.accelerationY * FACTOR) + (rollingY * (1 - FACTOR)); rollingZ = (event.accelerationZ * FACTOR) + (rollingZ * (1 - FACTOR));

}

However, this moving average is only desirable if the accelerometer update interval is small.