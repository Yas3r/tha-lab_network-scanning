####Instructions
1. The unicornscan application is installed on Backtrack at /pentest/scanners/unicornscan. Use this application to scan the same network range 10.16.0.0/24 to compare its accuracy with nmaps and fpings. Note that unicornscan cannot just perform a ping sweep, so this needs to be factored into the results and your expectations. An example command for using unicornscan is as follows:
```
unicornscan -Ir 10000 10.16.0.0/16
```
2. Explore the unicornscan help menu by typing the following command to gain a better understanding of some of the options available to you:
```
unicornscan -h
```
3. Much greater detailed information can be found by referencing the unicornscan manual pages by executing the following command:
```
man unicornscan
```

You may be surprised to see that unicornscan can render full port scan data as fast as the fping application can render simple ICMP ping sweeps.

This concludes the lab exercise.
