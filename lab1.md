#### PING SWEEPING WITH FPING
**fping** is a program like ping which uses the Internet Control Message Protocol (ICMP) echo request to determine if a target host is responding. fping differs from ping in that you can specify any number of targets on the command line, or specify a file containing the lists of targets to ping. Instead of sending to one target until it times out or replies, fping will send out a ping packet and move on to the next target in a round-robin fashion.

In the default mode, if a target replies, it is noted and removed from the list of targets to check; if a target does not respond within a certain time limit and/or retry limit it is designated as unreachable. fping also supports sending a specified number of pings to a target, or looping indefinitely (as in ping).

1. Pinging an entire network range with fping is easy and fairly quick. To scan the network address space `10.16.0.0/24` with fping we execute the following command:

  ```
  fping -g 10.16.0.0/24
  ```

    **Note**: the results of this ping scan and how the output shows both live hosts and unreachable hosts.

2. To show only live hosts in the output we can add the `-a` argument and execute the following command:

  ```
  fping -ag 10.16.0.0/24
  ```

    **Note**: these results could easily be used to create a target file for more in-depth port scanners such as nmap or a vulnerability scanner like Nessus. This is useful, as many times it is faster to perform just a ping sweep before turning a full port scan loose on a network. In our experience this can really pay off in timesavings for internal network penetration tests on large networks where the ICMP protocol is allowed internally. Note this strategy doesn’t normally work for external penetration tests, as many organizations block the ICMP protocol at their border or demark points.

3. fping can also output some useful statistics regarding the ping sweep using the `-s` argument. Execute the following command and pay attention to the closing statistics.

  ```
  fping -asg 10.16.0.0/24
  ```

4. fping allows us to not only specify entire subnet ranges using the standard CIDR notation, but also allows us to simply supply a list of IPs to scan. Execute the following command to demonstrate this:

  ```
  fping -as 10.16.0.8 10.16.0.15 10.16.0.17 10.16.0.53
  ```

5. fping allows us to specify how many pings we send to our targets as well. This can be useful in network troubleshooting if a host is experiencing intermittent network connectivity. To send 10 ping packets to a single host and gather the statistics we can execute the following command:

  ```
  fping -as -c 10 10.16.0.8
  ```

    **Note**: the statistics are reported on the top line of the results returned to the console.

6. We can also gather these statistics for an entire network range. Execute the following command:

  ```
  fping -as -c 5 -g 10.16.0.0/28
  ```

    Notice how the ICMP statistics are reported on a per host basis.

7. fping allows us to specify the intervals in which we send ICMP request packets in milliseconds. By default fping sends these requests at an interval of 25 milliseconds. We use the `-i` argument to specify how many milliseconds between each ICMP request packet sent. Execute the following command to demonstrate an interval of 100 milliseconds between ICMP request packets being sent by fping:

  ```
  fping -as -i 100 -g 10.16.0.0/24
  ```

    Notice this scan takes significantly longer to complete. Setting the milliseconds to a larger value can be extremely useful in avoiding intrusion prevention and intrusion detection systems. It can also aid in bypassing firewall limit rules.

8. fping also allows us to specify the timeout per target for our ICMP request packets, which can be extremely useful in speeding up our total scan time when we know the network is reliable. By default fping uses a 500-millisecond timeout. To modify this behavior we use the `-t` argument. Execute the following command to demonstrate this by setting a timeout of only 10 milliseconds:

  ```
  fping -as -t 10 -g 10.16.0.0/24
  ```

    Notice how much faster the scan finished as we did not have to wait the default 500 milliseconds for all ICMP request packets to expire.

9. Another option we can use on a reliable network to speed up our scans is to lower the number of retries attempted by fping when carrying out a ping sweep. By default fping will attempt to ping a host 3 times. We can modify be this default setting using the `-r` argument. Execute the following command to demonstrate this behavior:

  ```
  fping -as -r 1 -g 10.16.0.0/24
  ```

    **Note**: how much faster the scan finishes when we don’t have to wait for 3 ICMP requests to timeout per target host that is unreachable.

10. You have completed this lab. You can continue to lab 2 by following the instructions found at 
    ```
    /root/THA/network-scanning/lab2.md
    ```
