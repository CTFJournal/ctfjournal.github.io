---
layout: post
title: "test"
---


#Network Security - Technical Controls

-------------------
## IPTABLES

---------------

#### Display Rules
- View all rules: `iptables -L`
- View specific table: `iptables -L <table_name>`
- Display rules with line numbers: `iptables -L --line-numbers`

#### Rule Management
- Add a rule: `iptables -A <chain> <rule_spec>`
- Insert a rule at a specific position: `iptables -I <chain> <position> <rule_spec>`
- Delete a rule by number: `iptables -D <chain> <rule_number>`
- Delete all rules in a chain: `iptables -F <chain>`

#### Rule Specification
- `-s <source_ip>` : Set source IP
- `-d <dest_ip>` : Set destination IP
- `-p <protocol>` : Set protocol (e.g., tcp, udp, icmp)
- `--sport <port>` : Set source port
- `--dport <port>` : Set destination port
- `-j <target>` : Set target action (ACCEPT, DROP, REJECT, etc.)
- `-i <interface>` : Set incoming interface
- `-o <interface>` : Set outgoing interface

#### Common Chains
- `INPUT` : Incoming packets destined to the local system
- `OUTPUT` : Outgoing packets originating from the local system
- `FORWARD` : Packets passing through the system

#### Examples
- Allow incoming SSH: `iptables -A INPUT -p tcp --dport 22 -j ACCEPT`
- Block incoming HTTP: `iptables -A INPUT -p tcp --dport 80 -j DROP`
- Allow outgoing DNS: `iptables -A OUTPUT -p udp --dport 53 -j ACCEPT`
- Enable NAT: `iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE`
- Restrict outgoing initiated by a specific user: `iptables -A OUTPUT -o eth0 -m owner --uid-owner 1000 -j DROP`

#### Save/Restore Rules
- Save rules to a file: `iptables-save > rules.txt`
- Restore rules from a file: `iptables-restore < rules.txt`


--------------------



## Windows Firewall

-----------



##### Accessing Windows Firewall
- Open Windows Firewall settings: `Control Panel > System and Security > Windows Firewall`

##### Rule Management
- Create a new inbound rule: `Inbound Rules > New Rule...`
- Create a new outbound rule: `Outbound Rules > New Rule...`
- Follow the wizard to define rule properties and conditions.

##### Rule Types
- Program: Rule applies to a specific executable file.
- Port: Rule applies to a specific port number or range.
- Predefined: Rule based on predefined services or features.
- Custom: Rule with custom settings like specific IP addresses.

##### Rule Actions
- Allow the connection: Permits incoming/outgoing traffic as defined by the rule.
- Block the connection: Prevents incoming/outgoing traffic as defined by the rule.
- Allow if secure: Allows traffic if it is encrypted and matches the rule.

##### Rule Scope
- Any IP address: Rule applies to all IP addresses.
- These IP addresses: Define specific IP addresses for the rule.
- This computer: Rule applies only to traffic originating from the local machine.
- Local subnet: Rule applies to traffic within the same subnet.

##### Advanced Settings
- Edge traversal: Allows traffic from the internet to reach this program.
- Defer to user: Allows the user to decide whether to allow or block the connection.
- Authorized computers: Specifies remote computers that can connect.

##### Examples
- Allow incoming Remote Desktop (RDP):
  - Rule type: Port
  - TCP, specific local ports: 3389
  - Allow the connection
- Block outgoing traffic for a specific program:
  - Rule type: Program
  - Browse and select the program's executable
  - Block the connection

##### Notifications
- Customize notifications for blocked connections:
  - `Control Panel > System and Security > Windows Defender Firewall > Advanced settings > Windows Firewall Properties`

##### Export/Import Rules
- Export rules to a file: `Advanced settings > Export Policy`
- Import rules from a file: `Advanced settings > Import Policy`

-----------------------


## pfSense Firewall

------------

##### Accessing pfSense Firewall
- Open pfSense web interface: `https://<pfSense_IP_address>`
- Log in with your credentials.

##### Firewall Rules
- Navigate to: `Firewall > Rules`
- Add a new rule: `+ Add` button
- Choose interface, protocol, source/destination, and action.

##### Rule Types
- Pass: Allow traffic matching the rule.
- Block: Deny traffic matching the rule.

##### Rule Order
- Rules are evaluated from top to bottom.
- First match wins: Matching rule is applied.

##### Rule Attributes
- Interface: Select the network interface.
- Protocol: Choose the protocol (TCP, UDP, ICMP, etc.).
- Source/Destination: Define IPs, networks, or aliases.
- Port: Set source/destination port(s).

##### Advanced Options
- Gateway: Specify a gateway for the rule.
- Schedule: Apply rule based on a schedule.
- Logging: Log matching traffic.

##### Alias
- Create reusable IP or port aliases: `Firewall > Aliases`
- Use aliases in rules for easier management.

##### NAT (Port Forwarding)
- Navigate to: `Firewall > NAT > Port Forward`
- Add a new rule for port forwarding.
- Specify interface, protocol, source/destination ports, and destination IP.

##### NAT (Outbound)
- Navigate to: `Firewall > NAT > Outbound`
- Choose mode: Automatic, Hybrid, Manual.
- Automatic: Default NAT rules are created.
- Manual: Create custom outbound NAT rules.

##### Floating Rules
- Navigate to: `Firewall > Rules > Floating`
- Apply rules to multiple interfaces.
- Useful for traffic shaping, VPNs, etc.

##### VPN Rules
- Navigate to: `Firewall > Rules > <VPN_Interface>`
- Create rules specific to VPN interfaces.

##### Rule Groups
- Navigate to: `Firewall > Rules > Groups`
- Organize and apply rules using groups.

##### Rule Schedules
- Navigate to: `Firewall > Schedules`
- Create time-based schedules for rules.

-----------------------

