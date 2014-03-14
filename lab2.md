####Instructions
Nmap offers a wide variety of options for customizing the techniques used for host discovery. Host discovery is sometimes called ping scan, but it goes well beyond the simple ICMP echo request packets associated with the ubiquitous ping tool. Users can skip the ping step entirely with a list scan `-sL` or by disabling ping `-Pn`, or engage the network with arbitrary combinations of multi-port TCP SYN/ACK, UDP, SCTP INIT and ICMP probes. The goal of these probes is to solicit responses, which demonstrate that an IP address is actually active (is being used by a host or network device). On many networks, only a small percentage of IP addresses are active at any given time. This is particularly common with private address space such as `10.0.0.0/8`. That network has 16 million IPs, but we have seen it used by companies with less than a thousand machines. Host discovery can find those machines in a sparsely allocated sea of IP addresses.

If no host discovery options are given, Nmap sends an ICMP echo request, a TCP SYN packet to port 443, a TCP ACK packet to port 80, and an ICMP timestamp request. These defaults are equivalent to the `-PE` `-PS443` `-PA80` `-PP` options. An exception to this is that an ARP scan is used for any targets, which are on a local Ethernet network. For unprivileged Unix shell users, the default probes are a SYN packet to ports 80 and 443 using the connect system call. This host discovery is often sufficient when scanning local networks, but a more comprehensive set of discovery probes is recommended for security auditing.

 *Tip #1: In all of the scanning exercises in this lab, adding the -n switch can greatly speed up your scans. By default, nmap performs DNS Resolution of every host you’re trying to scan. The -n switch disables the DNS resolution and makes the overall scan/sweep execute faster.
 *Tip #2: As you’re learning how scanning works, try turning on the -vv (that’s 2 v’s, not the letter w) switch with nmap. This switch puts nmap in “Very Verbose” mode, which means it will print back to the screen everything that it’s doing. Not only does this make long scans more interesting, it’s also a great learning tool to see how nmap performs its tasks.

1. To perform the default host discovery in nmap we need to provide the `-sn` argument. It should be noted that in previous versions of nmap the `-sP` argument was used to perform the default host discovery. To perform this host discover in our lab execute the following command:
```
nmap -sn -n 10.16.0.0/24
```
**Note**: that this same scan in fping to considerably longer to execute, as nmap is extremely efficient with a magnitude of effort put into optimizing it’s scanning engines algorithms.
2. During the namp host discovery scan nmap performed 4 individual probes per host, as opposed to just sending an ICMP echo request to each host. To verify this we are going to use a network sniffer called tcpdump to capture out scan traffic to a single host by executing the following commands:
Open a second terminal window and execute the following command:
tcpdump -i eth0 -nntttt dst host 10.16.0.8
Return to your original terminal console window and execute the following nmap host discovery scan:
nmap -sn -n 10.16.0.8
Once the scan has finished return to the terminal console window that is currently running tcpdump. Press “Ctrl-c” to terminate tcpdump.
Examine this simple packet capture, as you will see 4 individual packets being sent from your THA BT host to the IP address 10.16.0.8.
The first packet is an ICMP echo request and should be very similar to this example:
IP 172.16.189.5 > 10.16.0.8: ICMP echo request, id 1091, seq 0, length 8
The second packet is a TCP SYN packet sent to port 443 of our target IP and should be very similar to this example:
IP 172.16.189.5.43756 > 10.16.0.8.443: Flags [S], seq 2146750298,
 win 2048, options [mss 1460], length 0
The third packet is a TCP ACK packet to port 80 of our target IP and should be very similar to this example:
IP 172.16.189.5.43756 > 10.16.0.8.80: Flags [.], ack 2146750298, win 2048, length 0
The fourth and final packet is a ICMP timestamp request to our target IP and should be very similar to this example:
IP 172.16.189.5 > 10.16.0.8: ICMP time stamp query id 55606 seq 0, length 20
As you can see, nmap did in fact send 4 individual probe requests to our target. The main reason nmap does this is to provide us with a very quick and efficient host discovery method that doesn’t rely on a single technique or protocol to work. In many environments ICMP echo requests and replies are filtered, so nmap also sends a ICMP timestamp request along with two TCP probes to common ports open on many servers. It isn’t uncommon for administrators to block ICMP echo requests and replies but forget to filter ICMP timestamp requests, making this technique deployed by nmap very effective.
Although nmap does not perform a simple ICMP ping sweep by default we can definitely configure nmap to do so. To do this we simply need to specify with arguments exactly the type of scan we want nmap to perform. Nmap can be configured to send 3 different types of ICMP packets using the following arguments:
-PE = ICMP type 8 (echo request)
-PP = ICMP code 14 (timestamp reply)
-PM = ICMP code 18 (address mask reply)
To have nmap perform just an ICMP ping sweep execute the following command:

nmap -sn -n -PE 10.16.0.0/24
It should be easy to note how much faster nmap returns results when compared to the fping application in the previous exercises.  Feel free to run a tcpdump capture to verify that nmap is only sending ICMP echo requests for this exercise.

Now that we have covered the basic host discovery methods used by nmap compare the actual results of the following scan types against the 10.16.0.0/24 network range and record the total hosts identified as up here:
Default host discovery
ICMP echo request
ICMP Timestamp reply
ICMP address reply
Where there any differences in the total number of hosts identified as being up, and if so why do you believe this is the case?
 
One extremely useful feature of fping is being able to construct a target list based on a simple output without having to do any complex parsing. Remember this command from the previous lab:
fping -an -g 10.16.0.0/24
To replicate this in nmap we can use some simple command line-fu! Execute the following command using nmap to get the same exact display/output format:
nmap -sn -n -PE 10.16.0.0/24 | grep report | awk ‘{print$5}’
Although we have to pipe the output from nmap into two separate applications to get this same output it should still be about 10 times faster than using fping. One thing that will highly benefit you as a penetration tester is to become very comfortable with simple and common bash parsing and formatting tools like grep, awk, sed, uniq, and sort, just to name a few.
Nmap also has some built in scan timing options that can drastically change the amount of time it takes to scan a network. By default nmap uses the “-T3” option. To demonstrate how much these options change nmaps overall scanning speed execute the following commands and record reported scanned speed along with the real, user, and sys times resulting from prepending the time application to our nmap scans:
time nmap -sn -n -PE -T2 10.16.0.0/24
Scanned time (nmap):
real: 
user:
sys:
 
time nmap -sn -n -PE -T3 10.16.0.0/24
Scanned time (nmap):
real:
user:
sys:
 
time nmap -sn -n -PE -T4 10.16.0.0/24
Scanned time (nmap):
real: 
user:
sys:
 
time nmap -sn -n -PE -T5 10.16.0.0/24
Scanned time (nmap):
real: 
user:
sys:
As you can see the built in timing options can have a huge impact on how quickly nmap can carry out a scan.
