#TODO


| VLAN Source  | Zone Destination | IP Destination      | Port Destination    | Protocole   | Service       |
|-------------|------------------|--------------------|---------------------|------------|--------------|
| **Temp**vlan_user   | vlan_srv         | 192.168.4.4, 192.168.4.3, 192.168.4.2 | All (TCP/UDP/ICMP) | TCP/UDP/ICMP | All Services  |
| vlan_user   | vlan_srv         | 192.168.4.3        | 389                 | TCP        | LDAP         |
| vlan_user   | vlan_srv         | 192.168.4.3        | 135                 | TCP        | RPC          |
| vlan_user   | vlan_srv         | 192.168.4.4        | 80                  | TCP        | HTTP         |
| vlan_user   | vlan_srv         | 192.168.4.3        | 53                  | UDP        | DNS          |
| vlan_user   | vlan_srv         | 192.168.4.3        | 445                 | TCP        | SMB          |
| vlan_user   | vlan_srv         | 192.168.4.3        | 139                 | UDP        | NetBIOS      |
| vlan_user   | vlan_srv         | 192.168.4.3        | 49152-65535         | TCP        | AD           |
| vlan_user   | vlan_srv         | 192.168.4.3        | 88                  | TCP        | Kerberos     |
| wifi_guest  | vlan_srv         | 192.168.4.3        | 53                  | UDP        | DNS          |
| wifi_emp    | vlan_srv         | 192.168.4.3, 192.168.4.4 | 389        | TCP        | LDAP         |
| wifi_emp    | vlan_srv         | 192.168.4.4        | 80                  | TCP        | HTTP         |
| wifi_emp    | vlan_srv         | 192.168.4.3        | 53                  | UDP        | DNS          |
| vlan_admin  | lan              | 192.168.0.115      | All                 | All        | Switch Access |
| vlan_srv    | All              | 192.168.4.1        | 67                  | UDP        | DHCP         |
