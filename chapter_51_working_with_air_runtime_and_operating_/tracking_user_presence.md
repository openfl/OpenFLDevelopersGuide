## Tracking user presence {#tracking-user-presence}

Adobe AIR 1.0 and later

The NativeApplication object dispatches two events that help you detect when a user is actively using a computer. If no mouse or keyboard activity is detected in the interval determined by the NativeApplication.idleThreshold property, the NativeApplication dispatches a userIdle event. When the next keyboard or mouse input occurs, the NativeApplication object dispatches a userPresent event. The idleThreshold interval is measured in seconds and has a default value of 300 (5 minutes). You can also get the number of seconds since the last user input from the NativeApplication.nativeApplication.lastUserInput property.

The following lines of code set the idle threshold to 2 minutes and listen for both the userIdle and userPresent

events:

NativeApplication.nativeApplication.idleThreshold = 120; NativeApplication.nativeApplication.addEventListener(Event.USER_IDLE, function(event:Event) {

trace(&quot;Idle&quot;);

});

NativeApplication.nativeApplication.addEventListener(Event.USER_PRESENT, function(event:Event) {

trace(&quot;Present&quot;);

});

**_Note:_ **_Only a single userIdle event is dispatched between any two userPresent events._