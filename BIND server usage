## Bind server

1. configure BIND to run on ipv4 only:

	`sudo vi /etc/default/named`

	`OPTIONS="-4 -u bind"`

2. Configure DNS Server
	edit the named configuration file.The location is different in different linux distributions.
	
	ex. in ubuntu, it's /etc/bind/named.conf.options ; in centos, it's /etc/named.conf
    
* access control list:
```
acl "group_name" {
	ip1;
	!ip2; //! to exclude ip
 	ip3/subnet; ...
}; 
```
* BIND ACL uses only the first matching, so in case `192.0.2.0/24; !192.0.2.1;`,  192.0.2.1 still pass because it matches first record

under /var/cache/bind
* listen to specific port:

	* `listen-on port [port number] { 127.0.0.1; [ip1]; [ip2];... };`

* allowed client

	* `allow-query     { localhost; [ip/subnet]; };`

* forwarder

	* `forwarders{[forwarder_ip1]; [forwarder_ip2]; ...};`

* to disable ipv6:

	* comment out `listen-on-v6 { any; }; `

* verify the BIND configuration

	* `sudo named-checkconf`


3. Define Zone 

    `sudo vi /etc/bind/named.conf.local`

    * zone file for the [hostname] forward map
    ```
    zone "hostname"{...
    };
	```
    
    * zone file for reverse map
    	* note that IP addresses are read backward.
    ```
    zone "xxx.xxx.xxx.in-addr.arpa"{
    	...
    };
    ```
    
    * show this dns is primary/secondary dns server
    	* `type [master/primary/slave]`

    * forward/ reverse lookup file
    	* `file [filename]`

    * (in primary dns server) transfer to secondary ip address
    	* allow-transfer { secondary_dns_ip; ...};

    * (in secondary dns server) point to primary DNS server
    	* masters { primary_dns_ip; };

4. Forward/ Reverse Zone File
	* note: use `;` to comment, every hostname need a `.` as suffix 

	* $ORIGIN indicates a DNS node tree and will typically start a DNS zone file.
	* “@” symbol: a special label that indicates the $ORIGIN should replace the “@” symbol

	* Start Of Authority(SOA) Record Format
    ```
	@     IN     SOA    {primary-name-server}     {hostmaster-email} (
                    {serial-number}
                    {time-to-refresh}
                    {time-to-retry}
                    {time-to-expire}
                    {minimum-TTL} )
	```
	* NS records (nameservers for the zone):
		* `IN NS [hostnames]`
	* MX records (Mail for Exchange)
		* IN MX [numeric priority] [hostname for SMTP]
		* **low number high priority**
	* A records (map hname to ip, for forward zone files):
		* `[hostname] IN A [ip]`
	* AAAA records (for ipv6)
		* `[hostname] IN A [ip]`
	* Canonial names
		* `[nickname] IN CNAME [true name]`
	* PTR records(map ip to hname, for reverse zone files):
		* `[ip address] IN PTR [hostname]`


## Client

1. check network interface: 
	`ip address show to [ip/subnet]`
    
2. Configure
	1. ubuntu > 18.04 : netplan
		`sudo vi /etc/netplan/private-nameservers.yaml`
		```
        network:
        version: 2
        ethernets:
            [network interface]:
                nameservers:
                    addresses:
                    - [dns-ip1]
                    - [dns-ip2]
                    search: [ [base domain name] ]

        ```
	2. Ubuntu 16.04 and Debian Clients
		`sudo vi /etc/network/interfaces`
        1. find the dns-nameservers line
        2. move to correct network interface if attached to `lo` interface
        3.
        ```
        ...
            dns-nameservers [dns-ip1] [dns-ip2] ...
            dns-search [base domain name]
        ...
        ```


3. centos:

	`sudo vi /etc/sysconfig/network-scripts/ifcfg-[network interface]`

    ```
    . . .
    DNS1=[DNS ip1]
    DNS2=[DNS ip2]
    DOMAIN=[base domain name]
    . . .
    ```
