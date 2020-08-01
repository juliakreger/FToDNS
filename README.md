# FToDNS: File Transfer over Domain Name Service
Christopher Neitzert <chris@neitzert.com>

### What:
A proof of concept that utilizes the Domain Name Service protocol, loosely, as it was intended for data delivery and exfiltration across network boundaries.

### Why:
The use of DNS for data exfiltration or VPN has been a hacker trope for the four decades that DNS has existed.
Recently with the creation of [DNS over HTTPS](https://en.wikipedia.org/wiki/DNS_over_HTTPS) in order to protect those that may be censored we inadvertadnly create an uncontrolable channel for data in/exfiltration.

There have been several impliementations of this concept and this Proof of Concept is not unique. 
Although this implementation is not based on the previous PoCs, several interesting methods will be linked in the [Erratum.txt](https://github.com/neitzert/FToDNS/blob/master/Erratum.txt)

### How it works
There are two types of landscape a DNS server exists in, for the purposes of this PoC we will call them Direct and Recursive.
Additionally, there are two verbs in a basic file transfer; We will call them 'Put File' and 'Get file'.

#### Landscape:
##### Direct Client to Server
![DNS](/images/DNS_Generic.png)
* A typical DNS client queries a DNS server for a host or zone.
* It might traverse a firewall or two as it crosses the internet
* The DNS server responds to the DNS client with an answer or error

##### Client to Recursive Server to Authoritative Server
![DNS](/images/DNS_Recursion.png)
* A typical DNS client queries a DNS server for a host or zone.
* Local rules might require the DNS client use a local server.
* The Local Server likely does not have the zone, so it queries the Authoritative Server on behalf of the DNS client.
* The query might traverse a firewall or two as it crosses the internet
* The Authoritative Server responds with an answer or error to the Local Server.
* The Local Server then relays that information to the DNS client.

#### Verbs

#### Put File
![DNS](/images/FToDNS_PutFile.png)
* The user chooses a file and Base64 encodes the file with 56 Byte long lines
	* One consideration is a multiple encoding method to correct for out of order query-server-log issues.
		* ex: base64 -w56 $FILE | cat -n | base64 -w56 > file_to_be_put


#### Get File
![DNS](/images/FToDNS_GetFile.png)
* A typical DNS client queries



### Proof of Concept Criteria and Requirements
1. PoC Criteria
	1. Proof of concept must be limited to tools built into the OS plus Bind9 for DNS service.
	1. Some sort of TCP-IP network between client & server that permits DNS between Client and Server. 
	1. Proof of concept must be able to, via the DNS protocol, 
		1. Copy a file from a remote server to a local client across a network
		1. Copy a file on a local client to a remote server across a network

1. Client Requirements:
	1. Standard Linux Distribution with standard GNU Core utils
	1. Host(1) ISC's standard host utility.  
	1. Base64, grep, cut, sed, awk, general sh scripting

1. Server Requirements:
	1. Standard Linux Distribution with standard GNU Core utils
	1. ISC's Bind9 DNS server 
		1. Ability to write zone files
		1. Ability to read log files
		1. Ability to change named.conf to allow zone transfers.
		1. named.conf configuration changes TBD
	1. Base64, grep, cut, sed, awk, general sh scripting


1. Types of Test 
	1. Due to the complexity of the PoC, this will be broken down into multiple types of PoC
		1. - [x]Direct Client to Server
			1. - [x]Read file 
			1. - [x]Write file
		1. - [ ]Client to Recursive to Authoritative
			1. - [ ]Read file
			1. - [ ]Write file
		1. - [ ]Client via DoH to HTTPS/DNS server 
			1. - [ ]Read file
			1. - [ ]Write file





### How to Mitigate:
This specific Proof of Concept creates several issues with its issuance and this document would not be complete without a short discussion on mitigation.

1. Network
	1. Limit Talkers
	1. Record DNS 'meta data'
	1. 
	
1. DNS Server Architecture
	1. Restrict zone transfers 
	1. Disable recursive checks and retrievals.	
	
1. Fuzz
	1. Timing and Frequency
	1. Query length
	1. Text Encoding
	1. 



### File Descriptions:

####Resolver.sh

A simple script to encode a file into a very large number of DNS host look up queries for later collection and reassembly on the upstream DNS server 


####ZoneGet.sh

Simple script to query a DNS server and decode the output of an entire DNS zone where each CNAME entry in the zone is a line fragment of a double base64 encoded file created by the script ZoneMaker.sh. 


####ZoneMaker.sh

Simple script to take a file and base64 encode it, write it into a DNS zone file for remote serving on a Bind9 DNS server.



 
### Disclaimers 
All standard disclaimers apply, no warranty, claims, or promises declared, made or implied.

For educational, research, and entertainment purposes only. 

Usability expires while you wait.

