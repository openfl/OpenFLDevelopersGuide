## Detecting AIR capabilities {#detecting-air-capabilities}

Adobe AIR 1.0 and later

For a file that is bundled with the Adobe AIR application, the Security.sandboxType property is set to the value defined by the Security.APPLICATION constant. You can load content (which may or may not contain APIs specific to AIR) based on whether a file is in the Adobe AIR security sandbox, as illustrated in the following code:

if (Security.sandboxType == Security.APPLICATION)

{

// Load SWF that contains AIR APIs

}

else

{

// Load SWF that does not contain AIR APIs

}

All resources that are not installed with the AIR application are assigned to the same security sandboxes as would be assigned by Adobe® Flash® Player in a web browser. Remote resources are put in sandboxes according to their source domains, and local resources are put in the local-with-networking, local-with-filesystem, or local-trusted sandbox.

You can check if the Capabilities.playerType static property is set to &quot;Desktop&quot; to see if content is executing in the runtime (and not running in Flash Player running in a browser).

For more information, see

“AIR security” on page 1076

.