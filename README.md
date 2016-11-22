BGP Role for Dell EMC Networking OS
===================================

This role facilitates the configuration of Border Gateway Protocol (BGP) attributes. It supports the configuration of router ID, networks, neighbors, and maximum path.
This role is abstracted for OS6, OS9, and OS10.

Installation
------------

```
ansible-galaxy install Dell-Networking.dellos-bgp
```

Requirements
------------

This role requires an SSH connection for connectivity to your Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the ``provider``
dictionary.

Role Variables
--------------
 
``dellos_bgp``(dictionary) contains the hostname (dictionary). 
The hostname is the value of the variable ``hostname`` that corresponds to the name of the OS device.
This role is abstracted using the variable ``ansible_net_os_name`` that can take the following values: dellos6, dellos9, and dellos10.

Any role variable with a corresponding state variable setting to absent negates the configuration of that variable. 
For variables with no state variable, setting empty value for the variable negates the corresponding configuration.
The variables and its values are case-sensitive.

hostname contains the following keys:

|        Key | Type                      | Notes                                                                                                                                                                                     |
|------------|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| asn | string(required) | Autonomous system number of the local BGP instance. |
| router_id | string | IP address used to configure the local BGP router instance. |
| maxpath_ibgp | int, default=1 | Maximum number of paths to forward packets through iBGP. The range is from 1 to 64. |
| maxpath_ebgp | int, default=1 | Maximum number of paths to forward packets through eBGP. The range is from 1 to 64. |
| ipv4_network | list | Contains objects to configure IPv4 BGP networks. See the following ipv4_network.* keys for each list item. |
| ipv4_network.address | string (required)         | Configures the IPv4 address of the BGP network. The value must be in the form of A.B.C.D/E. For OS6 devices, the value should be in the form A.B.C.D E.F.G.H where E.F.G.H is the netmask.  |
|    ipv4_network.state | string, choices: absent, present* | Configuring this key as absent deletes the BGP network.                                                                                                                                 |
| ipv6_network | list | Contains objects to configure IPv6 BGP network. See the following ipv6_network.* keys for each list item. |
| ipv6_network.address | string (required)         | Configures the IPv6 address of the BGP network. The value should in the format 2001:4898:5808:ffa2::1/126  |
|    ipv6_network.state | string, choices: absent, present* | Configuring this key as absent deletes the BGP network.                                                                                                                                 |
| ipv4_neighbor | list | Contains objects to configure IPv4 BGP neighbors. See the following ipv4_neighbor.* keys for each list item. |
| ipv4_neighbor.remote_asn | string (required)         | Remote autonomous system number of the BGP neighbor.  |
| ipv4_neighbor.ip | string (required)         | IPv4 address of the BGP neighbor. The value must be in the form of 10.1.1.1  |
| ipv4_neighbor.timer | string          | Configures neighbor timers. The value must be in the form of 5 10, where 5 is the keepalive interval and 10 is the holdtime. |
|  ipv4_neighbor.default_originate | boolean: true, false*     | Originates the default routes to the BGP neighbor. This option is not supported on OS10 devices.                                                                                                                                |
| ipv4_neighbor.peergroup | string          | Configures neighbor to BGP peer-group. The value is the name of the peer-group. |
|    ipv4_neighbor.peergroup_state | string, choices: absent, present* | Configuring this key as absent deletes the IPv4 BGP neighbor from the peer-group.                                                                                                                                 |
|    ipv4_neighbor.state | string, choices: absent, present* | Configuring this key as absent deletes the IPv4 BGP neighbor.                                                                                                                                 |
| ipv6_neighbor | list | Contains objects to configure IPv6 BGP neighbors. See the following ipv6_neighbor.* keys for each list item. |
| ipv6_neighbor.remote_asn | string (required)         | Remote autonomous system number of the BGP neighbor.  |
| ipv6_neighbor.ip | string (required)         | Configures the IPv6 address of the BGP neighbor. The value must be in the form of 2001:4898:5808:ffa2::1  |
| ipv6_neighbor.peergroup | string          | Configures the neighbor to BGP peer group. The value is the name of the peer group. |
|    ipv6_neighbor.peergroup_state | string, choices: absent, present* | Configuring this key as absent deletes the IPv6 BGP neighbor from the peer group.                                                                                                                                 |
|    ipv6_neighbor.state | string, choices: absent, present* | Configuring this key as absent deletes the IPv6 BGP neighbor.                                                                                                                                 |
| peer_group | list | Contains objects to configure BGP peer group. See the following peer_group.* keys for each list item. |
|   peer_group.name | string (required)         | Configures the BGP peer group with this name.  |
|   peer_group.asn | string                    | Configures the remote AS for the BGP peer groups.                                                                                                                                         |
|     peer_group.passive | boolean: true, false*     | Configures the passive BGP peer group.                                                                                                                               |
| peer_group.subnet | string (required)         | Configures the passive BGP neighbor to this subnet. Only passive peer groups can be assigned a subnet. This key is required together with the ``peer_group.passive`` key.  |
|    peer_group.state | string, choices: absent, present* | Configuring this key as absent deletes the BGP peer group.                                                                                                                                 |
| redistribute | list | Contains objects to get information from other routing protocols. See the following redistribute.* keys for each list item. |
|   redistribute.route_type | string (required), choices: static,connected        | Name of the routing protocol to redistribute. It supports redistributing only static and connected routes.   |
|   redistribute.address_type | string (required), choices: ipv4,ipv6                  | Specifies the address type of the IPv4 or IPv6 routes.                                                                                                                                         |
|    redistribute.state | string, choices: absent, present* | Selecting absent deletes the redistribution information.                                                                                                                                 |
|     bgp_state |  string, choices: absent, present*    | Configuring this key as absent deletes the local router BGP instance.                                                                                                                                          |
```
Note: Asterisk (*) denotes the default value if none is specified.
```

Connection Variables
--------------------

Ansible Dell EMC Networking roles require the following connection information to establish 
communication with the nodes in your inventory. This information can exist in
the Ansible group_vars or host_vars directories, or in the playbook itself.


|         Key | Required | Choices    | Description                              |
| ----------: | -------- | ---------- | ---------------------------------------- |
|        host | yes      |            | Hostname or address for connecting to the remote device over the specified ``transport``. The value of this key is the destination address for the transport. |
|        port | no       |            | Port used to build the connection to the remote device. If this key does not specify the value, the value defaults to 22. |
|    username | no       |            | Configures the username that authenticates the connection to the remote device. The value of this key authenticates the CLI login. If this key does not specify the value, the value of environment variable ANSIBLE_NET_USERNAME is used instead. |
|    password | no       |            | Specifies the password that authenticates the connection to the remote device. If this key does not specify the value, the value of environment variable ANSIBLE_NET_PASSWORD is used instead. |
|   authorize | no       | yes, no*   | Instructs the module to enter privileged mode on the remote device before sending any commands. If this key does not specify the value, the value of environment variable ANSIBLE_NET_AUTHORIZE is used instead. If not specified, the device attempts to execute all commands in non-privileged mode.|
|   auth_pass | no       |            | Specifies the password to use if required to enter privileged mode on the remote device. If this key is set to no, then this argument not applicable. If the ``auth_pass`` does not specify the value, the value of environment variable ANSIBLE_NET_AUTH_PASS is used instead. |
|   transport | yes      | cli*       | Configures the transport connection to use when connecting to the remote device. This key supports connectivity to the device over CLI (SSH).  |
|    provider | no       |            | Convenient method that passes all of the above connection arguments as a dictonary object. All constraints (such as required or choices) must be met either by individual arguments or values in this dictonary. |



```
Note: Asterisk (*) denotes the default value if none is specified.
```

Dependencies
------------

The dellos-bgp role is built on modules included in the core Ansible code.
These modules were added in Ansible version 2.2.0.


Example Playbook
----------------
The following example uses the dellos.dellos-bgp role to configure the BGP network and neighbors. 
It creates a ``hosts`` file with the switch details, a ``host_vars`` file with connection variables, and the corresponding 
variables defined in the ``vars/main.yml`` file at the role path.
This example writes a simple playbook that only references the dellos-bgp role. 
By including the role, you can automatically access all of the tasks to configure BGP. 


Sample ``hosts`` file:

    leaf1 ansible_host= <ip_address> ansible_net_os_name= <OS name(dellos9/dellos6/dellos10)>


Sample ``host_vars/leaf1``:

    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: xxxxx
      password: xxxxx
      authorize: yes
	  auth_pass: xxxxx 
      transport: cli
	  
Sample ``vars/main.yml``:

    dellos_bgp:
      leaf1:
        asn: 11
        router_id: 192.168.3.100
        maxpath_ibgp: 2
        maxpath_ebgp: 2
        ipv4_network:
          - address: 102.1.1.0/30
            state: present
        ipv6_network:
          - address: "2001:4898:5808:ffa0::/126"
            state: present
        ipv4_neighbor:
          - remote_asn: 12
            ip: 192.168.10.2
            timer: 5 10
            default_originate: False
            peergroup: per
            peergroup_state: present
            state: present
        ipv6_neighbor:
          - remote_asn: 14
            ip: 2001:4898:5808:ffa2::1
            peergroup: per
            peergroup_state: present 
            state: present
        peer_group:
          - name: peer0
            asn: 13
            state: present
          - name: peer1
            asn: 14
            passive: True
            subnet: 10.128.4.192/27
            state: present
        redistribute:
          - route_type: static
            state: present
            address_type: ipv4
          - route_type: connected
            address_type: ipv6
            state: present

A simple playbook to configure BGP, ``leaf.yml``:

    - hosts: leafs
      roles:
         - Dell-Networking.dellos-bgp

Then run with:

    ansible-playbook -i hosts leaf.yml

License
--------

Copyright (c) 2016, Dell Inc. All rights reserved.
 
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at
 
    http://www.apache.org/licenses/LICENSE-2.0
 
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
