Hacker Academy
==============
http://hackeracademy.com

### NETWORK SCANNING

#### Purpose
Network scanning is the logical extension to the network orientation work that you’ve already done. The goal is to get an understanding of which hosts exist throughout the network topology you have, as well as understanding what’s running on those hosts.

#### Requirements
This lab requires the following THA virtual machines
* Kali
* Network Emulator

#### Lab Credentials

* Kali VM: `root / toor`

#### Setup
The targets for the exercise are located on the Network Emulator VM.

Note: If you are using HACKS, you will not see the Network Emulator because its functionality exists in the back end.

1. Boot the THA Network Emulator VM (You don't need to log in or configure anything).

2. Boot your THA Kali VM and login.

3. Clone this repo on your Kali VM by opening a terminal and issuing the following command:

  ```bash
  git clone git://github.com/madsec/tha-lab_network-scanning.git /root/THA/network-scanning
  ```

4. After logging into Kali we need to temporarily disable the eth0 interface by executing the following command in a terminal console:

  ```
  ifdown eth0
  ```

5. We also need to set up a new network route by entering the following command in a terminal console:

  ```
  route add -net 10.0.0.0/8 gw 172.16.189.241
  ```

6. Test connectivity by pinging the ip `10.16.0.8`. If it fails, reboot the Network Emulator.

##### Note
* If the Kali VM network connection continually disconnects please reboot the VM.

#### Start the lab
* Follow the instructions for lab 1 found on your Kali machine at 
  ```
  /root/THA/network-scanning/lab1.md
  ```