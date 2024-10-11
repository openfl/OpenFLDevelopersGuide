# SOAP web service requests

SOAP builds on the general XML-RPC web service concept and provides a richer,
albeit more complex, means for transferring typed data. SOAP web services
typically provide a Web Service Description Language file (WSDL) that specifies
the web service calls, data types, and service URL. While OpenFL does not
provide direct support for SOAP, you can construct a SOAP XML message "by hand,"
post it to the server, and then parse the results. However, for anything except
the simplest SOAP web service, you can probably save a significant amount of
development time using an existing third-party SOAP library written in Haxe.