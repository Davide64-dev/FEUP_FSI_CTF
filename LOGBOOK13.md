# LOGBOOK13 - Sniffing and Spoofing

The goal of the work this week is to understand mechanisms for monitoring and manipulating traffic.

## Task 1.1 - Sniffing Packets

The first step on this task is to go to the terminal of the attacker container and to use the `ifconfig` command to find out what is the name of the interface. We found out that the name of the interface is: `br-ced6656419bc`.

Then, we go to the shared folder `volumes` in the attacker terminal and create a file named `sniffer.py` with the following code that was suggested in the guide:

```py
#!/usr/bin/env python3

from scapy.all import *

def print_pkt(pkt):
  pkt.show()

pkt = sniff(iface='br-ced6656419bc', filter='icmp', prn=print_pkt)
```

Then, we used the command `chmod a+x sniffer.py` to make this program executable.

Then the objective is to ping the HostB from HostA. To do that, we need to get the ip from the Host B machine. That could be done with the command `ifconfig`. We found out that the IP of the Host B machine is `ping 10.9.0.6`. Then, what we must do is to `ping 10.9.0.6` in Host A machine.

