# 21110114, Le Thuy Tuong Vy
# Lab 8: Firewall Lab

## Task 1: Setup rules on router to block all access into it except ping
First, after accessing the router, I listed the initial iptables policy.</br>

<img width="726" alt="firewall1" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall1.png"><br>

Next, I configured the router to block all access except for ping requests:</br>

<span style="color:yellow">iptables -P INPUT DROP:</span> This command sets the default policy for incoming traffic (INPUT chain) to DROP, meaning any packet not matching a specific rule will be dropped or denied.</br>

<span style="color:yellow">iptables -P OUTPUT DROP:</span> Similarly, this command sets the default policy for outgoing traffic (OUTPUT chain) to DROP, meaning all outgoing traffic is denied unless a specific rule permits it.</br>

<span style="color:yellow">iptables -A INPUT -p icmp --icmp-type 8 -j ACCEPT:</span> This rule allows incoming ICMP Echo Request (ping) packets. It permits incoming ICMP packets with type 8, which are used for ping requests. These packets are accepted (ALLOWed).</br>

<span style="color:yellow">iptables -A OUTPUT -p icmp --icmp-type 0 -j ACCEPT:</span> The command iptables -A OUTPUT -p icmp --icmp-type 0 -j ACCEPT allows outgoing ICMP Echo Reply packets, which are responses to ping requests. It permits outgoing ICMP packets with type 0, which are the responses to ping requests. These packets are accepted (ALLOWed). </br>

<span style="color:yellow">This rule drops (DENYs) incoming TCP traffic on port 80, commonly used for HTTP web traffic, thereby blocking incoming web traffic.</br>

<span style="color:yellow">This rule drops (DENYs) incoming TCP traffic on port 23, commonly used for Telnet, thereby blocking incoming Telnet traffic.</br>

<span style="color:yellow">iptables -A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT:</span> This rule allows incoming traffic that is part of an existing or related connection. For example, if your system initiates an outgoing connection to a remote server, this rule allows the response traffic back into your system. It is used for established and related connections.</br>

<span style="color:yellow">iptables -A OUTPUT -m state --state RELATED,ESTABLISHED -j ACCEPT:</span> Similar to the previous rule, this one allows outgoing traffic that is part of an existing or related connection, ensuring that responses to outgoing requests are allowed back into your system. </br>

<img width="726" alt="firewall2" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall2.png"><br>

Thirdly, I check again list policy of iptables</br>
<img width="726" alt="firewall3" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall3.png"><br>

Finally, when I accessed outsider-10.9.0.5, I verified that all access to the router is blocked except for ping requests.</br>
<img width="726" alt="firewall4" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall4.png"><br>
<img width="726" alt="firewall5" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall5.png"><br>

## Task 2: Setup rules on router to prevent computers on subnet 10.9.0.0/24 from accessing the internal web server (iweb).

Firstly, like what I did in above, I continue to access into router to setup rules to prevent computers on subnet 10.9.0.0/24 from accessing the iweb-172.16.10.110</br>

<span style="color:yellow">iptables -A FORWARD -s 10.9.0.0/24 -d 172.16.10.110 -j DROP:</span> This rule affects the FORWARD chain, which is used for packets passing through the system (typically from one network interface to another). It specifies that any packet with a source IP address in the range 10.9.0.0/24 and a destination IP address of 172.16.10.110 should be dropped or denied when attempting to traverse the system. This rule restricts forwarding traffic from the specified source network to the specified destination IP address.</br>
<img width="726" alt="firewall6" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall6.png"><br>

After setting up, I can check by accessing into outsider and try access into iweb and we’re successfull</br>
<img width="726" alt="firewall7" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall7.png"><br>

## Task 3: The badsite was found to contain malwares and source of delivering bots. Setup rules on router to stop computers on subnet 172.16.10.0/24 from accessing the badsite.

In router, I setup some below rules to stop computer on subnet 172.16.10.0/24 from accessing the badsite but outsider computer still can telnet into iweb and inner computer still can telnet into outsider</br>

<span style="color:yellow">iptables -P INPUT DROP:</span> This command sets the default policy for incoming traffic (INPUT chain) to DROP. This means that unless a packet matches a specific rule allowing it, all incoming traffic will be denied.</br>

<span style="color:yellow"> This command sets the default policy for
forwarding traffic (FORWARD chain) to ACCEPT. It means that any traffic passing through the system (from one network interface to another) will be allowed by default unless specific rules block it.</br>

<span style="color:yellow">iptables -A INPUT -i lo -j ACCEPT:</span> This rule allows all traffic on the loopback interface (lo), which is used for local communication within the same system. It permits traffic that originates and terminates on the same machine (localhost) to be accepted.</br>

<span style="style:yellow">iptables -A FORWARD -s 172.16.10.0/24 -d 10.9.0.5 -j ACCEPT:</span> This rule allows forwarding of packets originating from the source subnet 172.16.10.0/24 to the destination IP address 10.9.0.5. It permits this specific traffic to be forwarded through the system.</br>

<span style="color:yellow">iptables -A FORWARD -s 172.16.10.0/24 -d 10.9.0.10 -j DROP:</span> This rule blocks forwarding of packets from the source subnet 172.16.10.0/24 to the destination IP address 10.9.0.10. It denies this specific traffic from being forwarded.</br>

<span style="color:yellow">
    iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT:
</span> This rule allows incoming traffic that is part of an established or related connection. It permits responses to outgoing connections and established connections to be accepted.</>

<img width="726" alt="firewall7" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall7.png"><br>

After that, I access into inner computer and try to access to badsite-10.9.0.10, and as you can see, all access are blocked</br>

<img width="726" alt="firewall7" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall7.png"><br>

But outsider computer (external) still can telnet into iweb (internal), inner computer (internal) still can telnet into outsider.</br>

<img width="726" alt="firewall7" src="https://raw.githubusercontent.com/Dilele2509/Labs_InformationSecurity/main/Images/firewall7.png"><br>