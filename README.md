# BGP Role for Dell EMC Networking OS
This role facilitates the configuration of border gateway protocol (BGP) attributes. It supports the configuration of router ID, networks, neighbors, and maximum path. This role is abstracted for dellos9, dellos6 and dellos10 devices.

## Installation

    ansible-galaxy install Dell-Networking.dellos-bgp


## Requirements
This role requires an SSH connection for connectivity to your Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the ``provider``
dictionary.

## Role variables
This role is abstracted using the variable ``ansible_net_os_name`` that can take dellos9, dellos6, and dellos10 values.

Any role variable with a corresponding state variable setting to absent negates the configuration of that variable. 
For variables with no state variable, setting empty value for the variable negates the corresponding configuration.
The variables and its values are case-sensitive.

#### ``dellos_bgp`` keys

| Key | Type | Notes |
|--|--|--|
| asn | string (required) | Configures the autonomous system number of the local BGP instance. |
| router_id | string | Configures IP address of the local BGP router instance. |
| graceful_restart | boolean | Configures graceful restart capability. |
| graceful_restart.state | string, choice(absent, present*) | If set to absent, removes graceful restart capability.  |
| maxpath_ibgp | integer, default=1 | Configures maximum number of paths to forward packets through iBGP (1 to 64). |
| maxpath_ebgp | integer, default=1 | Configures maximum number of paths to forward packets through eBGP (1 to 64). |
| best_path | list | Configures default best-path selection (see best_path.* keys for each list item) — key is not supported in dellos6. |
| best_path.as_path | string (required), choice(ignore, multipath-relax) | Configures the AS path used for the best path computation. |
| best_path.as_path_state | string, choice(absent, present*) | If set to absent, deletes the AS path configuration. |
| best_path.ignore_router_id | boolean(true, false) | If set to true, ignores the router identifier in best path computation. |
| best_path.med | list | Configures MED attribute (see med.* keys for each list item). |
| med.attribute | string (required), choice(confed, missing-as-best) | Configures the MED attribute used for the best path computation.  |
| med.state | string, choice(absent, present*) | If set to absent, deletes MED attribute.|
| ipv4_network | list | Configures IPv4 BGP networks (see ipv4_network.* keys for each list item). |
| ipv4_network.address | string (required) | Configures the IPv4 address of the BGP network — value must be in the form of A.B.C.D/E. |
| ipv4_network.state | string, choices: absent, present* | If set to absent, deletes the IPV4 BGP network. |
| ipv6_network | list | Configures IPv6 BGP network (see ipv6_network.* keys for each list item). |
| ipv6_network.address | string (required) | Configures the IPv6 address of the BGP network — value should in the format 2001:4898:5808:ffa2::1/126. |
| ipv6_network.state | string, choice(absent, present*) | If set to absent, deletes the IPV6 BGP network. |
| neighbor | list | Configures IPv4 BGP neighbors (see neighbor.* keys for each list item). |
| neighbor.ip | string (required)  | Configures the IPv4 address of the BGP neighbor — value must be in the form of 10.1.1.1. |
| neighbor.name | string (required) | Configures the BGP peer group with this name — key is supported only when the neighbor is a peer group, and is mutually exclusive with the key neighbor.ip. |
| neighbor.type | string (required), choice(ipv4, ipv6, peergroup) | Specifies the type of the BGP neighbor. |
| neighbor.remote_asn | string (required) | Configures the remote autonomous system number of the BGP neighbor. |
| neighbor.remote_asn_state | string, choice(absent, present*) |This key is supported only when neighbor.type is "peergroup". If set to absent, deletes the remote autonomous system number from peer group. |
| neighbor.timer | string | Configures neighbor timers — value should be in the form &lt;int&gt; &lt;int&gt; for example 5 10, where 5 is the keepalive interval and 10 is the holdtime. |
| neighbor.default_originate | boolean(true, false*) | Configures default originate routes to the BGP neighbor — key is not supported in dellos10. | 
|neighbor.peergroup | string | Configures neighbor to BGP peer-group — value is the name of the configured peer-group. |
| neighbor.peergroup_state | string, choice(absent, present*) | If set to absent, deletes the IPv4 BGP neighbor from the peer-group. |
| neighbor.distribute_list | list | Configures distribute list to filter networks from routing updates (see distribute_list.* keys for each list item — key is not supported in dellos6. |
| distribute_list.in | string | Configures the name of the prefix list to filter incoming packets. |
| distribute_list.in_state | string, choice(absent, present*) | If set to absent, deletes the filter at incoming packets. |
| distribute_list.out | string | Configures the name of the prefix list to filter outgoing packets. |
| distribute_list.out_state | string, choice(absent, present*) | If set to absent, deletes the filter at outgoing packets. |
| neighbor.admin | string, choice(up, down) | Configures the administrative state of the neighbor. |
| neighbor.adv_interval | integer  | Configures the advertisement interval of the neighbor. |
| neighbor.fall_over | string, choice(absent, present*) | Configures the session fall on peer route loss. |
| neighbor.sender_loop_detect | boolean(true, false) | Enable/Disable the sender side loop detect for neighbor — key is not supported in dellos6. |
| neighbor.src_loopback | integer | Configures the source loopback interface for the routing packets — key is not supported in dellos10. |
| neighbor.src_loopback_state | string, choice(absent, present*) | If set to absent, deletes the source for routing packets. |
| neighbor.ebgp_multihop | integer, default=255 | Configures maximum hop count value allowed in EBGP neighbors that are not directly connected. |
| neighbor.passive | boolean(true, false*) | Configures the passive BGP peer group — key is supported only when neighbor is a peer group, and is supported only in dellos9. |
| neighbor.subnet | string (required) | Configures the passive BGP neighbor to this subnet — key is required together with the ``neighbor.passive`` key for dellos9 devices. |
| neighbor.subnet_state | string, choice(absent, present*) | If set to absent, deletes the subnet range set for dynamic IPv4 BGP neighbor.  |
| neighbor.state | string, choice(absent, present*) | If set to absent, deletes the IPv4 BGP neighbor. |
| redistribute | list | Configures redistribute list to get information from other routing protocols (see redistribute.* keys for each list item). |
| redistribute.route_type | string (required), choice(static, connected) | Configures name of the routing protocol to redistribute. |
| redistribute.route_map_name | string | Configures route map to redistribute. |
| redistribute.route_map |  string, choice(absent, present*)  | If set to absent, deletes the route map to redistribute. |
| redistribute.address_type | string (required), choice(ipv4, ipv6) | Configures the address type of the IPv4 or IPv6 routes. |
| redistribute.state | string, choice(absent, present*) | If set to absent, deletes the redistribution information. |
| state |  string, choice(absent, present*) | If set to absent, deletes the local router BGP instance. |

> **NOTE**: Asterisk (*) denotes the default value if none is specified.

## Connection variables
Ansible Dell EMC Networking roles require the following connection information to establish 
communication with the nodes in your inventory. This information can exist in
the Ansible *group_vars* or *host_vars* directories, or in the playbook itself.


| Key | Required | Choices | Description |
|--|--|--|--|
| host | yes | | Hostname or address for connecting to the remote device over the specified ``transport`` — value of this key is the destination address for the transport. |
| port | no | | Port used to build the connection to the remote device. If this key does not specify the value, the value defaults to 22. |
| username | no | | Configures the username that authenticates the connection to the remote device — value of this key authenticates the CLI login. If this key does not specify the value, the value of environment variable ANSIBLE_NET_USERNAME is used instead. |
| password | no | | Specifies the password that authenticates the connection to the remote device. If this key does not specify the value, the value of environment variable ANSIBLE_NET_PASSWORD is used instead. |
| authorize | no | yes, no* | Instructs the module to enter privileged mode on the remote device before sending any commands. If this key does not specify the value, the value of environment variable ANSIBLE_NET_AUTHORIZE is used instead. If not specified, the device attempts to execute all commands in non-privileged mode.|
| auth_pass | no |  | Specifies the password to use if required to enter privileged mode on the remote device. If this key is set to no, then this argument not applicable. If the ``auth_pass`` does not specify the value, the value of environment variable ANSIBLE_NET_AUTH_PASS is used instead. |
| transport | yes | cli* | Configures the transport connection to use when connecting to the remote device — key supports connectivity to the device over CLI (SSH). |
| provider | no | | Convenient method that passes all of the above connection arguments as a dictonary object. All constraints (such as required or choices) must be met either by individual arguments or values in this dictonary. |

> **NOTE**: Asterisk (*) denotes the default value if none is specified.

## Dependencies
The dellos-bgp role is built on modules included in the core Ansible code, version 2.2.0.

## Example playbook
This example uses the dellos.dellos-bgp role to configure the BGP network and neighbors. It creates a ``hosts`` file with the switch details, a ``host_vars`` file with connection variables and the corresponding role variables. This example writes a simple playbook that only references the dellos-bgp role. 

#### Sample ``hosts`` file

    leaf1 ansible_host= <ip_address> ansible_net_os_name= <OS name(dellos9/dellos6/dellos10)>

#### Sample ``host_vars/leaf1``

    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: xxxxx
      password: xxxxx
      authorize: yes
      auth_pass: xxxxx 
      transport: cli
	  
    dellos_bgp:
        asn: 11
        router_id: 192.168.3.100
        maxpath_ibgp: 2
        maxpath_ebgp: 2
        graceful_restart: true
        best_path:
           as_path: ignore
           ignore_router_id: true
           med:
            - attribute: confed
              state: present
            - attribute: missing-as-best
              state: present
        ipv4_network:
          - address: 102.1.1.0/30
            state: present
        ipv6_network:
          - address: "2001:4898:5808:ffa0::/126"
            state: present
        neighbor:
          - ip: 192.168.10.2
            type: ipv4
            remote_asn: 12
            timer: 5 10
            adv_interval: 40
            fall_over: present
            default_originate: False
            peergroup: per
            peergroup_state: present
            sender_loop_detect: false
            src_loopback: 1
            src_loopback_state: present
            distribute_list:
               in: aa
               in_state: present
            ebgp_multihop: 25
            admin: up
            state: present
          - ip: 2001:4898:5808:ffa2::1
            type: ipv6
            remote_asn: 14
            peergroup: per
            peergroup_state: present
            distribute_list:
               in: aa
               in_state: present
            src_loopback: 0
            src_loopback_state: present
            ebgp_multihop: 255
            admin: up
            state: present
          - name: peer1
            type: peergroup
            remote_asn: 14
            distribute_list:
               in: an
               in_state: present
               out: bb
               out_state: present
            passive: True
            subnet: 10.128.4.192/27
            subnet_state: present
            state: present
        redistribute:
          - route_type: static
            route_map_name: aa
            state: present
            address_type: ipv4
          - route_type: connected
            address_type: ipv6
            state: present

#### Simple playbook to configure BGP, ``leaf.yaml``

    - hosts: leaf1
      roles:
         - Dell-Networking.dellos-bgp

#### Then run with

    ansible-playbook -i hosts leaf.yaml

## License
Copyright © 2017 Dell EMC. All rights reserved.
 
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
 
Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
