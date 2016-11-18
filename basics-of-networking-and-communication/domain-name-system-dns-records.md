# Domain Name System (DNS) records {#domain-name-system-dns-records}

Adobe AIR 2.0 and later

You can look up DNS resource records using the DNSResolver class. DNS resource records provide information like the IP address of a domain name and the domain name of an IP address. You can look up the following types of DNS resource records:

*   ARecord—IPv4 address for a host.
*   AAAARecord—IPv6 address for a host.
*   MXRecord—mail exchange record for a host.
*   PTRRecord—host name for an IP address.
*   SRVRecord—service record for a service.

To look up a record, you pass a query string and the class object representing the record type to the lookup() method of the DNSResolver object. The query string to use depends on the record type:

| **Record class** | **Query string** | **Example query string** |
| --- | --- | --- |
| ARecord | host name | “example.com” |
| AAAARecord | host name | “example.com” |
| MXRecord | host name | “example.com” |
| PTRRecord | IP address | “208.77.188.166” |
| SRVRecord | Service identifier: _service._protocol.host | “_sip._tcp.example.com” |

The following code example looks up the IP address of the host “example.com”.

package

{

import flash.display.Sprite;

import flash.events.DNSResolverEvent; import flash.events.ErrorEvent; import flash.net.dns.ARecord;

import flash.net.dns.DNSResolver;

public class DNSResolverExample extends Sprite

{

public function DNSResolverExample()

{

var resolver:DNSResolver = new DNSResolver(); resolver.addEventListener( DNSResolverEvent.LOOKUP, lookupComplete ); resolver.addEventListener( ErrorEvent.ERROR, lookupError );

resolver.lookup( &quot;example.com.&quot;, ARecord );

}

private function lookupComplete( event:DNSResolverEvent ):void

{

trace( &quot;Query string: &quot; + event.host );

trace( &quot;Record count: &quot; + event.resourceRecords.length ); for each( var record:* in event.resourceRecords )

{

if( record is ARecord ) trace( record.address );

}

}

private function lookupError( error:ErrorEvent ):void

{

trace(&quot;Error: &quot; + error.text );

}

}

}

For more information, see:

*   DNSResolver
*   DNSResolverEvent
*   ARecord
*   AAAARecord
*   MXRecord
*   PTRRecord
*   SRVRecord