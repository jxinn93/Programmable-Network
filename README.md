LAB 4 QNA

1. Why did the first firewall rule have no effect, and why did blocking h1 → h3 also break h3 → h1?
The first rule had no explicit priority, so it used OpenFlow's default priority of 0. An existing forwarding rule with a higher priority took precedence, allowing the ping. Setting the drop rule's priority to 40000 made it take effect. Blocking h1 → h3 prevents ICMP Echo Requests from reaching h3, so h1 receives no Echo Replies and the ping fails.

2. Why did one conversation survive the controller failure and another didn't?
The h1 → h2 conversation continued working because the controller had already installed matching flow entries in the switch. These entries allowed the switch to forward packets without contacting the controller. In contrast, h1 → h3 failed because its flow entries had not been installed, and the table-miss rule sent unmatched packets to the unavailable controller. After the 60-second idle timeout, the h1 → h2 flow entries expired, so that conversation failed as well.

3. Secure or standalone: which would you choose for a campus network?
Secure mode, because it prevents the switch from falling back to normal MAC-learning when the controller fails, helping maintain the intended security policies. However, this may interrupt connectivity during controller outages. Standalone mode improves availability by allowing normal Ethernet switching, but traffic may bypass OpenFlow security policies.

