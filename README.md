# NetProtectionLab_TP1
Exploring some basic Networking Commands including nmap, ping, traceroute and more

## Nmap - for Network Mapping

![image](https://user-images.githubusercontent.com/91763346/234418687-e2022f6b-9b8a-43ed-9ce8-e202bd728994.png)


Nmap is short for Network Mapper. It is an open-source Linux command-line tool that is used to scan IP addresses and ports in a network and to detect installed applications.

Nmap allows network admins to find which devices are running on their network, discover open ports and services, and detect vulnerabilities.

Why use Nmap?
There are a number of reasons why security pros prefer Nmap over other scanning tools.

First, Nmap helps you to quickly map out a network without sophisticated commands or configurations. It also supports simple commands (for example, to check if a host is up) and complex scripting through the Nmap scripting engine.

Other features of Nmap include:

Ability to quickly recognize all the devices including servers, routers, switches, mobile devices, etc on single or multiple networks.
Helps identify services running on a system including web servers, DNS servers, and other common applications. Nmap can also detect application versions with reasonable accuracy to help detect existing vulnerabilities.
Nmap can find information about the operating system running on devices. It can provide detailed information like OS versions, making it easier to plan additional approaches during penetration testing.
During security auditing and vulnerability scanning, you can use Nmap to attack systems using existing scripts from the Nmap Scripting Engine.
Nmap has a graphical user interface called Zenmap. It helps you develop visual mappings of a network for better usability and reporting.

Commands

Nmap has many options and scan options and it's quite hard to remember all of them, so this cheatsheet can be a small reminded for the options of the tool.

![image](https://user-images.githubusercontent.com/91763346/234418977-a4c34b3b-2830-4505-a5ef-3aadc0548d4b.png)

Let's take a look at some nmap commands

* Scan a single host 
Scans a single host for 1000 well-known ports. These ports are the ones used by popular services like SQL, SNTP, apache, and others.

```
> nmap scanme.nmap.org
```

* Stealth Scan

Stealth scanning is performed by sending an SYN packet and analyzing the response. If SYN/ACK is received, it means the port is open, and you can open a TCP connection.

However, a stealth scan never completes the 3-way handshake, which makes it hard for the target to determine the scanning system.
```
> nmap -sS scanme.nmap.org
```

You can use the ‘-sS’ command to perform a stealth scan. Remember, stealth scanning is slower and not as aggressive as the other types of scanning, so you might have to wait a while to get a response.

* Version scanning
Finding application versions is a crucial part in penetration testing.

It makes your life easier since you can find an existing vulnerability from the Common Vulnerabilities and Exploits (CVE) database for a particular version of the service. You can then use it to attack a machine using an exploitation tool like Metasploit.

```
> nmap -sV scanme.nmap.org
```

To do a version scan, use the ‘-sV’ command. Nmap will provide a list of services with its versions. Do keep in mind that version scans are not always 100% accurate, but it does take you one step closer to successfully getting into a system.

* OS Scanning

In addition to the services and their versions, Nmap can provide information about the underlying operating system using TCP/IP fingerprinting. Nmap will also try to find the system uptime during an OS scan.

```
> nmap -sV scanme.nmap.org
```

You can use the additional flags like osscan-limit to limit the search to a few expected targets. Nmap will display the confidence percentage for each OS guess.

Again, OS detection is not always accurate, but it goes a long way towards helping a pen tester get closer to their target.

* Agressive Scan
Nmap has an aggressive mode that enables OS detection, version detection, script scanning, and traceroute. You can use the -A argument to perform an aggressive scan.

```
> nmap -A scanme.nmap.org
```

Aggressive scans provide far better information than regular scans. However, an aggressive scan also sends out more probes, and it is more likely to be detected during security audits.

* Port Scanning

Port scanning is one of the most fundamental features of Nmap. You can scan for ports in several ways.

Using the -p param to scan for a single port

```
> nmap -p 973 192.164.0.1
```

If you specify the type of port, you can scan for information about a particular type of connection, for example for a TCP connection.

```
> nmap -p T:7777, 973 192.164.0.1
```

A range of ports can be scanned by separating them with a hyphen.

```
> nmap -p 76–973 192.164.0.1
```

You can also use the -top-ports flag to specify the top n ports to scan.

```
> nmap --top-ports 10 scanme.nmap.org
```

## Scanning with nmap

Let's start with a simple scan, our target now is google.com

![image](https://user-images.githubusercontent.com/91763346/234421755-5c1d48bd-94fc-474d-99f7-8b09d76c5b05.png)

Let's try to fine tune our scan with more options from the cheatsheet

![image](https://user-images.githubusercontent.com/91763346/234422203-55c1cc24-093a-4cd5-bb89-fd3b7411ada6.png)

![image](https://user-images.githubusercontent.com/91763346/234423230-326d3fdd-3d35-4338-9dec-f9a0db7a01a4.png)

![image](https://user-images.githubusercontent.com/91763346/234423710-5a1e8dd6-d3e0-4fca-967b-99f093c36c27.png)

We can see that we have found 2 open ports, which is very normal for a website, as they use http (port 80) and https (port 443).

## Traceroute and Ping

* How does Ping work?

Ping uses ICMP (Internet Control Message Protocol) Echo messages to see if a remote host is active or inactive, how long a round trip message takes to reach the target host and return, and any packet loss.

It sends a request and waits for a reply (which it receives if the destination responds back within the timeout period).

It's basically a quick, easy way to verify that you can reach a destination on the internet. If you can, great! If not, you can use traceroute to investigate what's happening at every step between your device and the destination.

* How does Traceroute work?

By default, traceroute sends three packets of data to test each 'hop' (when a packet is passed between routers it is called a 'hop').

It will first send 3 packets to an unreachable port on the target host, each with a Time-To-Live (TTL) value of 1. This means that as soon as it hits the first router in the path (within your network), it will timeout. The first router will respond with an ICMP Time Exceeded Message (TEM), as the datagram has expired.

Then another 3 datagrams are sent, with the TTL set to 2, causing the second router (your ISP) in the path to respond with an ICMP TEM.

This continues until the datagrams eventually have a TTL long enough to reach the destination. When it does, as the messages are being sent to an invalid port, an ICMP port unreachable message is returned, signaling that the traceroute is finished.

Let's run a simple ping on google.tn

![image](https://user-images.githubusercontent.com/91763346/234424473-90cac502-8347-4bd8-8967-171b5b8f02cb.png)

We can see that our ping on avg is 90ms.

Now let's try to see the route between us and google.tn

![image](https://user-images.githubusercontent.com/91763346/234424941-187f3c84-37b3-4695-9a15-a75cf1c0520d.png)

We can see that we have an increasing number of hops between us and google.tn but for the sake of this lab, I decided to simply suspend the process midway.
