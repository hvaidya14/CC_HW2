Task 1
1. What is the output of “nodes” and “net”

The output of "nodes" is: 
available nodes are: 
c0 h1 h2 h3 h4 h5 h6 h7 h8 s1 s2 s3 s4 s5 s6 s7

The output of "net" is: 
h1 h1-eth0:s3-eth2
h2 h2-eth0:s3-eth3
h3 h3-eth0:s4-eth2
h4 h4-eth0:s3-eth4
h5 h5-eth0:s6-eth2
h6 h6-eth0:s6-eth3
h7 h7-eth0:s7-eth2
h8 h8-eth0:s7-eth3
s1 lo:  s1-eth1:s2-eth1 s1-eth2:s5-eth1
s2 lo:  s2-eth1:s1-eth1 s2-eth2:s3-eth1 s2-eth3:s4-eth1
s3 lo:  s3-eth1:s2-eth2 s3-eth2:h1-eth0 s3-eth3:h2-eth0 s3-eth4:h4-eth0
s4 lo:  s4-eth1:s2-eth3 s4-eth2:h3-eth0
s5 lo:  s5-eth1:s1-eth2 s5-eth2:s6-eth1 s5-eth3:s7-eth1
s6 lo:  s6-eth1:s5-eth2 s6-eth2:h5-eth0 s6-eth3:h6-eth0
s7 lo:  s7-eth1:s5-eth3 s7-eth2:h7-eth0 s7-eth3:h8-eth0
c0

2. What is the output of “h7 ifconfig”

The output of "h7 ifconfig" is: 
h7-eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.0.0.7  netmask 255.0.0.0  broadcast 10.255.255.255
        inet6 fe80::4c9d:66ff:fe38:ee63  prefixlen 64  scopeid 0x20<link>
        ether 4e:9d:66:38:ee:63  txqueuelen 1000  (Ethernet)
        RX packets 69  bytes 5302 (5.3 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 11  bytes 866 (866.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


Tast - 2
1. Function call diagram is as follows
   ->start switch : 
       first called _handle_PacketIn ()
       second called act_like_switch ()
       Third called resend_packet ()
       forth called send(msg) which is forwarded to port.

2. 
a. h1 ping -c 100 h2 = avg = 1.85
b. h1 ping -c 100 h2 = min = 2.96
                      = max = 14.72

a.h1 ping -c100 h8 = avg = 15.23
b.h1 ping -c 100 h8 = min = 9.45 
                    = max = 30.58

c. Difference seen is in ping values for h1 ping h2 and h1 ping h8 h1 ping h8 is longer compared with h1 ping h2 because h1 only has one switch within itself and h2, where as there are several hops between h1 and h8 through in between switches s3, s2, s1, s5, s7 from h1 to h8

3.
a.Iperf: used for testing TCP bandwidth between h1 and h2
- Results: ['8.12 Mbits/sec', '9.63 Mbits/sec']
b.Iperf: testing TCP bandwidth between h1 and h8
- Results: ['3.45 Mbits/sec', '4.87 Mbits/sec']
c.The throughput is higher between h1 and h2 than h1 -> h8 because it might be due to less hops as less number of switches and within less time data can be transferred.

4. I added a log -> log.debug("Switch analyzing  traffic testing: %s" % (self.connection)) which showed me that all of the switches seems to view the traffic. The _handle_PacketIn function is the event listener so its called everytime a packet is received.


Task 3.
1. act_like_switch is making switches more smarter as described in h3 document. Switches will extract the destination MAC address as they receive the packet from ports and forward the packet to exact port if they have already known about the same address earlier.

2.
a. h1 ping h2 avg is 1.32 ms
   h1 ping h2 min is 1.73 ms max is 7.23 ms.
b. h1 ping h8 avg 9.72 ms
   h1 ping h8 min 4.96 ms max is 19.35 ms.
c. task 2 the value for h1 ping h2 takes sligtly less time in task 3, the difference is significant for ping time values for h1 ping h8 switches are made intelligent due to act_like_switch code due to mac_port mapping logic

3.
a.
For "iperf h1 h2", the throughputs are:
- Results: ['32.86 Mbits/sec', '34.23 Mbits/sec']
For "iperf h1 h8",  the throughputs are:
- Results: ['2.63 Mbits/sec', '3.48 Mbits/sec']
The throughput for task 3 is larger than that for task 2 as the switches are made smarter due to mac_port mapping as switches upon knowing the destination MAC address attached with packet will forward to port mapped to that destination MAC address.

