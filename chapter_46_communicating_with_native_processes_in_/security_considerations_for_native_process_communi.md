## Security considerations for native process communication {#security-considerations-for-native-process-communication}

Adobe AIR 2 and later

The native process API can run any executable on the user’s system. Take extreme care when constructing and executing commands. If any part of a command to be executed originates from an external source, carefully validate that the command is safe to execute. Likewise, your AIR application should validate any data passed to a running process.

However, validating input can be difficult. To avoid such difficulties, it is best to write a native application (such as an EXE file on Windows) that has specific APIs. These APIs should process only those commands defined by the application. For example, the application may accept only a limited set of instructions via the standard input stream.

AIR on Windows does not allow you to run .bat files directly. The command interpreter application (cmd.exe) executes Windows .bat files. When you invoke a .bat file, this command application can interpret arguments passed to the command as additional applications to launch. A malicious injection of extra characters in the argument string could cause cmd.exe to execute a harmful or insecure application. For example, without proper data validation, your AIR application may call myBat.bat myArguments c:/evil.exe. The command application would launch the evil.exe application in addition to running your batch file.