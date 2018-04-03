---
layout: post
title: Bypass Your ISP's DNS & Run A Private OpenNIC Server
issue: Volume 34, Issue 3
date: 2017-10-01 00:00:00
categories: articles
comments: false
en: true
description: Bypass Your ISP's DNS & Run A Private OpenNIC Server
keywords: "2600, bind, cron, dig, dns, opennic"
author: mikedank
---

## Introduction

With recent U.S. legislation regarding Internet privacy, we see another example of control moving away from consumers and towards service providers. Following the news of this change, many have taken a renewed interest in methods that can take back some of the control and privacy that ISPs and other organizations have slowly been chipping away.

One such service that consumers can liberate (and run) for themselves is DNS. The Domain Name System is responsible for retrieving IP addresses (like 123.45.67.89) from domain names (like 2600.com). For a simplified explanation, when you go to visit a website your machine hasn't seen before, your machine will query a caching server that is usually owned by your ISP or a company like Google or OpenDNS. This server will return the proper IP address, if they have it cached, or query its way along a chain of DNS servers to the authoritative one controlling that domain. Once found, the IP address for the domain entered will trickle back to you and complete the initial request, allowing your machine to resolve it.

Companies that control these services have a direct look into the sites you are trying to visit. You can bet that more than just a few of them are logging queries and using them for marketing purposes or creating profiles based on who is sitting behind the keyboard at the address of origin. However, there are alternative DNS providers out there who can offer more privacy than others are willing to supply.

One such project, OpenNIC, has been operating a network of DNS servers for many years. Unlike traditional DNS providers, OpenNIC provides an alternate root to the ICANN system (which resolves traditional TLDs, top level domains like .com, .net, etc.) while maintaining backwards compatibility with them. Using OpenNIC, you can still resolve all of the same sites, but also get access to those run by OpenNIC operators, with TLDs such as .geek, .pirate, and .bbs. OpenNIC is made up of hobbyists, engineers, and tinkerers who not only want to explore the ins and outs of DNS, but also offer enhanced privacy and free domain registration for TLDs within their root! You may see OpenNIC as just-another-organization to query, but many operators are privacy-oriented, running their own servers devoid of logging and/or in countries that don't poke around in your network traffic.

Aside from using an official OpenNIC DNS server to query your home traffic against directly, you can also set one up yourself. Using a modest VPS (512MB of RAM, 4GB of disk) hosted somewhere outside of the US (or the 14-eyes jurisdiction, if you prefer), you can subvert organizations who may be nefariously gathering information from your queries. While your server will still ultimately connect upstream to an OpenNIC server, any clients at home or on the go never will - they will only directly query your new DNS server directly.

## Installation & Configuration

Setting up a DNS server is relatively easy to do with just a basic understanding of the shell. I'm running a Debian system, so some of the configuration may be different depending on the distribution you are running. Additionally, the steps below are for configuring a BIND server. There are many different DNS server packages out there to choose from, though BIND is arguably the most widespread on GNU/Linux hosts.

After logging into our server we will first want to switch to the root account to configure BIND.

```
$ su -
```

Next, we will install bind9 and DNS utilities using the package manager. This will automatically configure a (non-publicly accessible) DNS server for us to work with and various DNS tools that will aid in setting up the server (specifically, dig).

```
$ apt-get install bind9 dnsutils -y
```

Now, we will pull down the OpenNIC root hints file for BIND to use. The root hints file simply contains information about OpenNIC's root DNS servers that control the alternative TLDs OpenNIC has to offer (as well as provide backwards compatibility to ICANN domains). On Debian, we save this information to `/etc/bind/db.root` for BIND to access.

```
$ dig . NS @75.127.96.89 > /etc/bind/db.root
```

While the root hints information does not change often, new TLDs can be added to OpenNIC periodically. We will set up a cron job that updates this file once a month (you can specify this to be more frequent is you wish) at 12:00AM on the first of the month. Let's edit the crontab to add this recurring job.

```
$ crontab -e
```

At the bottom of the file, paste the following and save, activating our job.

```
0 0 1 * * /usr/bin/dig . NS @75.127.96.89 > /etc/bind/db.root
```

Next, we will want to make some changes to the BIND configuration files. Specifically, we will allow recursive queries (so our BIND installation can query the OpenNIC root servers), enable DNSSEC validation (to verify integrity of DNS data on query to OpenNIC servers), and whitelist our client's IP address. Edit `/etc/bind/named.conf.options` and replace the contents with the following options block, making any edits as needed to specify a client's IP address.

```
options {        
    directory "/var/cache/bind";

    //Allow localhost and a client IP of 1.2.3.4        
    allow-query { localhost; 1.2.3.4; };        
    recursion yes;

    dnssec-enable yes;        
    dnssec-validation yes;        
    dnssec-lookaside auto;

    auth-nxdomain no;    # conform to RFC1035        
    listen-on-v6 { any; };  //Only use if your server has an ipv6 iface! 
};
```

Now, we will also change the logging configuration so that no logs are kept for any queries to our server. This is beneficial in that we know our own queries will never be logged on our server (as well as queries from anyone else we might authorize to use our server at a later date) for any reason. To make this change, edit `/etc/bind/named.conf` and add the following logging block to the bottom of the file.

```
logging {
    category default { null; };
};
```

Finally, restart BIND so it can use our new configuration.

```
$ /etc/init.d/bind9 restart
```

Now, make sure that our server is using itself for DNS by checking the `/etc/resolv.conf` file. If it doesn't exist already, place the following line above any other lines starting with `nameserver`.

```
nameserver 127.0.0.1
```

Testing resolution of both OpenNIC and ICANN TLDs can be done with a few simple ping commands.

```
$ ping -c4 2600.com 
$ ping -c4 opennic.glue
```

## Conclusion & Next Steps

Now that the server is in place, you are free to configure your client machine(s), home router, etc. to make use of the new DNS server. Provided you have port 53 open for both UDP and TCP on the server's firewall, you should be able to add a similar `nameserver` line to the `/etc/resolv.conf` file (as seen in the previous section) on any authorized client machine, using the server's external IP address instead of the loopback `127.0.0.1` address.
Instructions for DNS configuration on many different operating systems and devices are readily available from a myriad of sources online if you aren't using a Linux-based client machine. Upon successful configuration, your client should be able to execute the two ping commands in the previous section, verifying a proper setup!

As always, be sure to take precautions and secure your server if you have not done so already. With a functioning DNS server now configured, this project could be expanded upon (as a follow-up exercise/article) by implementing a tool such as DNSCrypt to authenticate and secure your DNS traffic.

## Sources
* [https://opennicproject.org](https://opennicproject.org)
* [http://www.zytrax.com/books/dns](http://www.zytrax.com/books/dns)
