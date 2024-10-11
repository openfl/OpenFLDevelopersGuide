# Web service requests {#web-service-requests}

There are a variety of HTTP-based web services. The main types include:

- REST

- XML-RPC

- SOAP

To use a web service in OpenFL, you create a URLRequest object, construct the
web service call using either URL variables or an XML document, and send the
call to the service using a URLLoader object. Third-party libraries may contain
additionally classes that make it easier to use web services—especially useful
when accessing complex SOAP services.

When your application runs in a browser, you can only use web services in the
same Internet domain as the OpenFL application — unless the server hosting the
web service also supports Cross-Origin Resource Sharing (CORS) that permits
access from other domains. A technique that is sometimes used when a CORS is not
available is to proxy the requests through your own server.

In OpenFL applications targeting native platforms, such as iOS, Android,
Windows, macOS or Linux, CORS is not required. OpenFL native application content
is never served from a remote domain, so it cannot participate in the types of
attacks that cross-origin policies prevent.

- [REST-style web service requests](./rest-style-web-service-requests.md)
- [XML-RPC web service requests](./xml-rpc-web-service-requests.md)
- [SOAP web service requests](./soap-web-service-requests.md)

## More Help topics

[REST architecture](http://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)

[XML-RPC](http://en.wikipedia.org/wiki/XML-RPC)

[SOAP protocol](http://www.w3.org/TR/soap/)
