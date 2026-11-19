LAB 4 QNA

1. Why did the first firewall rule have no effect, and why did blocking h1 → h3 also break h3 → h1?
The first rule had no explicit priority, so it used OpenFlow's default priority of 0. An existing forwarding rule with a higher priority took precedence, allowing the ping. Setting the drop rule's priority to 40000 made it take effect. Blocking h1 → h3 prevents ICMP Echo Requests from reaching h3, so h1 receives no Echo Replies and the ping fails.

2. Why did one conversation survive the controller failure and another didn't?
The h1 → h2 conversation continued working because the controller had already installed matching flow entries in the switch. These entries allowed the switch to forward packets without contacting the controller. In contrast, h1 → h3 failed because its flow entries had not been installed, and the table-miss rule sent unmatched packets to the unavailable controller. After the 60-second idle timeout, the h1 → h2 flow entries expired, so that conversation failed as well.

3. Secure or standalone: which would you choose for a campus network?
Secure mode, because it prevents the switch from falling back to normal MAC-learning when the controller fails, helping maintain the intended security policies. However, this may interrupt connectivity during controller outages. Standalone mode improves availability by allowing normal Ethernet switching, but traffic may bypass OpenFlow security policies.

LAB 6 QNA
1. A Mininet host is built using a Linux network namespace with its own network interfaces, IP addresses, routing table and ARP table. A Mininet cable is a virtual Ethernet (veth) pair connecting two network namespaces or switches.

2. For Fix A (underlay MTU 1500, overlay MTU 1450), a 1422-byte ping payload succeeded, while 1423 bytes failed. TCP throughput was 786 Mbps. For Fix B (underlay MTU 9000, overlay MTU 1500), a 1472-byte ping payload succeeded, while 1473 bytes exceeded the tenant MTU. TCP throughput was 820 Mbps. VXLAN adds 50 bytes over IPv4: 14 bytes outer Ethernet, 20 bytes outer IPv4, 8 bytes UDP and 8 bytes VXLAN header. Over IPv6, the outer IP header increases from 20 to 40 bytes, making the total overhead 70 bytes.

3. Small pings continued working because their packet sizes were within the available MTU. Larger packets exceeded the MTU and were dropped or fragmented. TCP kept retransmitting lost packets, causing its throughput to drop significantly.

4. A Docker container has its own isolated filesystem, process space, environment and resource limits. A Mininet host mainly provides network isolation using a network namespace while sharing the host's Linux kernel.

5. VXLAN adds 50 bytes over IPv4, while my Week 5 P4 tunnel adds a 4-byte custom header. VXLAN requires VTEPs at both ends to understand the VXLAN header, while intermediate underlay routers only need to forward the outer IP packet. The P4 tunnel requires P4 switches to understand and process its custom header, while other intermediate devices can forward it without understanding the header. VXLAN uses the outer IP TTL to prevent infinite loops, while the P4 tunnel uses a custom hop-limit field that decreases at each switch until the packet is dropped.
