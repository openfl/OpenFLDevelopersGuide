## Getting the runtime version and patch level {#getting-the-runtime-version-and-patch-level}

Adobe AIR 1.0 and later

The NativeApplication object has a runtimeVersion property, which is the version of the runtime in which the application is running (a string, such as &quot;1.0.5&quot;). The NativeApplication object also has a runtimePatchLevel property, which is the patch level of the runtime (a number, such as 2960). The following code uses these properties:

trace(NativeApplication.nativeApplication.runtimeVersion); trace(NativeApplication.nativeApplication.runtimePatchLevel);