BGP role
========

This role facilitates the configuration of border gateway protocol (BGP) attributes. It supports the configuration of router ID, networks, neighbors, and maximum path. This role is abstracted for dellos9, dellos6, and dellos10 devices.

The BGP role requires an SSH connection for connectivity to a Dell EMC Networking device. You can use any of the built-in Dell EMC Networking OS connection variables, or the *provider* dictionary.

Installation
------------

    ansible-galaxy install Dell-Networking.dellos-bgp

Role variables
--------------
 
- Role is abstracted using the *ansible_net_os_name* variable that can take dellos9, dellos6, and dellos10 values
- If variable *dellos_cfg_generate* is set to true, it generates the role configuration commands in a file
- Any role variable with a corresponding state variable setting to absent negates the configuration of that variable
- Setting an empty value for any variable negates the corresponding configuration
- Variables and values are case-sensitive

**dellos_bgp keys**

| Key        | Type                      | Description                                             | Support               |
|------------|---------------------------|---------------------------------------------------------|-----------------------|
| ``asn`` | string (required) | Configures the autonomous system (AS) number of the local BGP instance | dellos6, dellos9, dellos10 |
| ``router_id`` | string | Configures the IP address of the local BGP router instance | dellos6, dellos9, dellos10 |
| ``graceful_restart`` | boolean | Configures graceful restart capability | dellos9, dellos10 |
| ``graceful_restart.state`` | string: absent,present\* | Removes graceful restart capability if set to absent | dellos9 |
| ``maxpath_ibgp`` | integer | Configures the maximum number of paths to forward packets through iBGP (1 to 64; default 1) | dellos6, dellos9, dellos10 |
| ``maxpath_ebgp`` | integer | Configures the maximum number of paths to forward packets through eBGP (1 to 64; default 1) | dellos6, dellos9, dellos10 |
| ``log_neighbor_changes`` | boolean | Configures log neighbors up/down | dellos10 |
| ``fast_ext_fallover`` | boolean | Configures reset session if a link to a directly connected external peer goes down.| dellos10. |
| ``always_compare_med`` | boolean | Configure comparing MED from different neighbors. | dellos10. |
| ``default_loc_pref`` | integer | Configure default local preference value. | dellos10. |
| ``confederation`` | dictionary | Configures AS confederation parameters.(see ``confederation.*``) | dellos10. |
| ``confederation.identifier`` | integer | Configure routing domain confederation AS. | dellos10 |
| ``confederation.peers`` | string | Configure peer ASs in BGP confederation. | dellos10 |
| ``confederation.peers_state`` | string, choices: absent, present\* | If set to absent, removes peer ASs in BGP confederation.   | dellos10 |
| ``route_reflector`` | dictionary | Configures route reflection parameters.(see ``route_reflector.*``) | dellos10. |
| ``route_reflector.client_to_client`` | boolean | Configure client to client route reflection.| dellos10 |
| ``route_reflector.cluster_id`` | string | Configure Route-Reflector Cluster-id. | dellos10 |
| ``address_family_ipv4`` | dictionary | Configures ipv4 address family parameters. (see ``address_family_ipv4.*``) | dellos10. |
| ``address_family_ipv4.aggregate_addr`` | list | Configures ipv4 BGP aggregate entries. (see ``aggregate_addr.*``) | dellos10 |
| ``aggregate_addr.ip_and_mask`` | string | Configures ipv4 BGP aggregate address. | dellos10 |
| ``aggregate_addr.state`` | string, choices: absent, present\* | If set to absent, removes ipv4 BGP aggregate entry.   | dellos10 |
| ``address_family_ipv4.dampening`` | dictionary | Configures route-flap dampening.(see ``dampening.*``) | dellos10 |
| ``dampening.value`` | dictionary | Configure the values for dampening. The value should be in format &lt;half-life time&gt; &lt;start value to reuse&gt; &lt;start value to suppress&gt; &lt;max duration&gt;. The default value is 15 750 2000 60. | dellos10 |
| ``dampening.route_map`` | string | Configure the Route-map to specify criteria for dampening. | dellos10 |
| ``dampening.state`` | string, choices: absent, present\* | If set to absent, removes dampening.   | dellos10 |
| ``best_path`` | list | Configures the default best-path selection (see ``best_path.*``) | dellos9, dellos10 |
| ``best_path.as_path`` | string (required): ignore,multipath-relax     | Configures the AS path used for the best-path computation   | dellos9, dellos10 |
| ``best_path.as_path_state`` | string: absent,present\*     | Deletes the AS path configuration if set to absent  | dellos9, dellos10 |
| ``best_path.ignore_router_id`` | boolean: true,false | Ignores the router identifier in best-path computation if set to true | dellos9, dellos10 |
| ``best_path.med`` | list | Configures the MED attribute (see ``med.*``) | dellos9, dellos10 |
| ``med.attribute`` | string (required): confed,missing-as-best     | Configures the MED attribute used for the best-path computation   | dellos9, dellos10 |
| ``med.state`` | string: absent,present\* | Deletes the MED attribute if set to absent | dellos9, dellos10 |
| ``ipv4_network`` | list | Configures an IPv4 BGP networks (see ``ipv4_network.*``) | dellos6, dellos9, dellos10 |
| ``ipv4_network.address`` | string (required)         | Configures the IPv4 address of the BGP network (A.B.C.D/E format)   | dellos6, dellos9, dellos10 |
| ``ipv4_network.state`` | string: absent,present\* | Deletes an IPv4 BGP network if set to absent | dellos6, dellos9, dellos10 |
| ``ipv6_network`` | list | Configures an IPv6 BGP network (see ``ipv6_network.*``) | dellos6, dellos9, dellos10 |
| ``ipv6_network.address`` | string (required)         | Configures the IPv6 address of the BGP network (2001:4898:5808:ffa2::1/126 format)  | dellos6, dellos9, dellos10 |
| ``ipv6_network.state`` | string: absent,present\* | Deletes an IPv6 BGP network if set to absent | dellos6, dellos9, dellos10 |
| ``neighbor`` | list | Configures IPv4 BGP neighbors (see ``neighbor.*``) | dellos6, dellos9, dellos10 |
| ``neighbor.ip`` | string (required)         | Configures the IPv4 address of the BGP neighbor (10.1.1.1)  | dellos6, dellos9, dellos10 |
| ``neighbor.name`` | string (required)         | Configures the BGP peer-group with this name; supported only when the neighbor is a peer group; mutually exclusive with *neighbor.ip* | dellos6, dellos9, dellos10 |
| ``neighbor.type`` | string (required): ipv4,ipv6,peergroup       | Specifies the BGP neighbor type   | dellos6, dellos9, dellos10 |
| ``neighbor.password`` | string      | Configures the BGP neighbor password  | dellos10  |
| ``neighbor.route_reflector_client`` | boolean      | Configures router reflector client on the BGP neighbor. | dellos10  |
| ``neighbor.local_as`` | integer     | Configure local AS for the BGP peer. | dellos10  |
| ``neighbor.weight`` | integer     | Configure default weight for routes from the neighbor interface. | dellos10  |
| ``neighbor.send_community`` | list | Configures the send community attribute to the BGP neighbor (see ``send_community.*``) | dellos10 |
| ``send_community.type`` | string (required)         | Configures the send community attribute to the BGP neighbor | dellos10 |
| ``send_community.state`` | string: absent,present\* | Deletes the send community attribute of the BGP neighbor if set to absent | dellos10 |
| ``neighbor.address_family`` | list | Configures address family commands on the BGP neighbor (see ``address_family.*``)| dellos10 |
| ``address_family.type`` | string (required): ipv4,ipv6         | Configures IPv4/IPv6 address family command mode on the BGP neighbor  | dellos10 |
| ``address_family.activate`` | boolean   | Configures activation/deactivation of IPv4/IPv6 address family command mode on the BGP neighbor  | dellos10 |
| ``address_family.allow_as_in`` | integer  | Configures local AS number in as-path.  | dellos10 |
| ``address_family.next_hop_self`` | boolean   | Configures disabling the next hop calculation for the neighbor.  | dellos10 |
| ``address_family.soft_reconf`` | boolean   | Configures per neighbor soft reconfiguration.  | dellos10 |
| ``address_family.add_path`` | string  | Configures send or receive multiple paths. The value can be 'both &lt;no of paths&gt;' , 'send &lt;no of paths&gt;', 'receive'.| dellos10 |
| ``address_family.route_map`` | list   | Configures route map on the BGP neighbor. (see ``route_map.*``) | dellos10 |
| ``route_map.name`` | string  | Configures name of the route map on the BGP neighbor.   | dellos10 |
| ``route_map.filter`` | string  | Configures filter for routing updates.   | dellos10 |
| ``route_map.state`` | string, choices: absent,present* | If set to absent deletes the route map of the BGP neighbor. | dellos10 |
| ``address_family.state`` | string: absent,present\* | Deletes the address family command mode of the BGP neighbor if set to absent | dellos10 |
| ``neighbor.remote_asn`` | string (required)         | Configures the remote AS number of the BGP neighbor  | dellos6, dellos9, dellos10 |
| ``neighbor.remote_asn_state`` | string: absent,present\* | Deletes the remote AS number from the peer group if set to absent; supported only when *neighbor.type* is "peergroup" | dellos6, dellos9, dellos10 |
| ``neighbor.timer`` | string          | Configures neighbor timers (<int> <int>); 5 10, where 5 is the keepalive interval and 10 is the holdtime | dellos6, dellos9, dellos10 |
| ``neighbor.default_originate`` | boolean: true, false\*     | Configures default originate routes to the BGP neighbor | dellos6, dellos9 | 
| ``neighbor.peergroup`` | string          | Configures neighbor to BGP peer-group (configured peer-group name) | dellos6, dellos9, dellos10 |
| ``neighbor.peergroup_state`` | string: absent,present\* | Deletes the IPv4 BGP neighbor from the peer-group if set to absent | dellos6, dellos9, dellos10 |
| ``neighbor.distribute_list`` | list | Configures the distribute list to filter networks from routing updates (see ``distribute_list.*``) | dellos9, dellos10 |
| ``distribute_list.in`` | string       | Configures the name of the prefix-list to filter incoming packets  | dellos9, dellos10 |
| ``distribute_list.in_state`` | string: absent,present\* | Deletes the filter at incoming packets if set to absent           | dellos9, dellos10 |
| ``distribute_list.out`` | string       | Configures the name of the prefix-list to filter outgoing packets   | dellos9, dellos10 |
| ``distribute_list.out_state`` | string: absent,present\* | Deletes the filter at outgoing packets if set to absent          | dellos9, dellos10 |
| ``neighbor.admin`` | string: up,down       | Configures the administrative state of the neighbor  | dellos6, dellos9, dellos10 |
| ``neighbor.adv_interval`` | integer       | Configures the advertisement interval of the neighbor  | dellos9, dellos10 |
| ``neighbor.fall_over`` | string: absent,present       | Configures the session fall on peer-route loss  |  dellos9, dellos10 |
| ``neighbor.sender_loop_detect`` | boolean: true,false         | Enables/disables the sender-side loop detect for neighbors | dellos9, dellos10 |
| ``neighbor.src_loopback`` | integer         | Configures the source loopback interface for routing packets | dellos6, dellos9  |
| ``neighbor.src_loopback_state`` | string: absent,present\* | Deletes the source for routing packets if set to absent                 | dellos6, dellos9 |
| ``neighbor.ebgp_multihop`` | integer | Configures the maximum-hop count value allowed in eBGP neighbors that are not directly connected (default 255) | dellos6, dellos9, dellos10 |
| ``neighbor.passive`` | boolean: true,false\*     | Configures the passive BGP peer group; supported only when neighbor is a peer-group | dellos9 |                 
| ``neighbor.subnet`` | string (required)         | Configures the passive BGP neighbor to this subnet; required together with the *neighbor.passive* key for dellos9 devices | dellos6, dellos9, dellos10 |
| ``neighbor.subnet_state`` | string: absent,present\* | Deletes the subnet range set for dynamic IPv4 BGP neighbor if set to absent            | dellos6, dellos9, dellos10 |
| ``neighbor.limit`` | integer    | Configures maximum dynamic peers count. This key is required together with the ``neighbor.subnet`` key. | dellos10.|
| ``neighbor.state`` | string: absent,present\* | Deletes the IPv4 BGP neighbor if set to absent | dellos6, dellos9, dellos10 |
| ``redistribute`` | list | Configures the redistribute list to get information from other routing protocols (see ``redistribute.*``) | dellos6, dellos9, dellos10 |
| ``redistribute.route_type`` | string (required): static,connected        | Configures the name of the routing protocol to redistribute | dellos6, dellos9, dellos10 |
| ``redistribute.route_map_name`` | string        | Configures the route-map to redistribute | dellos9, dellos10 |
| ``redistribute.route_map`` |  string: absent,present\*    | Deletes the route-map to redistribute if set to absent        | dellos9, dellos10 |
| ``redistribute.address_type`` | string (required): ipv4,ipv6                  | Configures the address type of IPv4 or IPv6 routes | dellos6, dellos9, dellos10 |
| ``redistribute.state`` | string: absent,present\* | Deletes the redistribution information if set to absent | dellos6, dellos9, dellos10 |
| ``state`` |  string: absent,present\*    | Deletes the local router BGP instance if set to absent      | dellos6, dellos9, dellos10 |

> **NOTE**: Asterisk (\*) denotes the default value if none is specified.

Connection variables
--------------------

Ansible Dell EMC Networking roles require connection information to establish communication with the nodes in your inventory. This information can exist in the Ansible *group_vars* or *host_vars* directories, or in the playbook itself.

| Key         | Required | Choices    | Description                                         |
|-------------|----------|------------|-----------------------------------------------------|
| ``host`` | yes      |            | Specifies the hostname or address for connecting to the remote device over the specified transport. |
| ``port`` | no       |            | Specifies the port used to build the connection to the remote device; if value is unspecified it defaults to 22 |
| ``username`` | no       |            | Specifies the username that authenticates the CLI login for connection to the remote device; if value is unspecified, the ANSIBLE_NET_USERNAME environment variable value is used |
| ``password`` | no       |            | Specifies the password that authenticates the connection to the remote device; if value is unspecified, the ANSIBLE_NET_PASSWORD environment variable value is used |
| ``authorize`` | no       | yes, no\*   | Instructs the module to enter privileged mode on the remote device before sending any commands; if value is unspecified, the ANSIBLE_NET_AUTHORIZE environment variable value is used and the device attempts to execute all commands in non-privileged mode |
| ``auth_pass`` | no       |            | Specifies the password to use if required to enter privileged mode on the remote device; if key is set to no, this argument not applicable; if value is unspecified, the ANSIBLE_NET_AUTH_PASS environment variable value is used |
| ``provider`` | no       |            | Passes all connection arguments as a dictonary object; all constraints (such as required or choices) must be met either by individual arguments or values in this dictionary |

> **NOTE**: Asterisk (\*) denotes the default value if none is specified.

Dependencies
------------

The *dellos-bgp* role is built on modules included in the core Ansible code. These modules were added in Ansible version 2.2.0.

Example playbook
----------------

This example uses the *dellos.dellos-bgp* role to configure the BGP network and neighbors. It creates a *hosts* file with the switch details, a *host_vars* file with connection variables and the corresponding role variables.

When *dellos_cfg_generate* is set to true, the variable generates the configuration commands as a .part file in *build_dir* path. By default, the variable is set to false. This example writes a simple playbook that only references the *dellos-bgp* role. The sample host_vars given below is for dellos9. 

**Sample hosts file**

    leaf1 ansible_host= <ip_address> ansible_net_os_name= <OS name(dellos9/dellos6/dellos10)>

**Sample host_vars/leaf1**

    hostname: leaf1
    provider:
      host: "{{ hostname }}"
      username: xxxxx
      password: xxxxx
      authorize: yes
      auth_pass: xxxxx 
    build_dir: ../temp/dellos9
	  
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
        state: present

**Simple playbook to configure BGP - leaf.yaml**

    - hosts: leaf1
      roles:
         - Dell-Networking.dellos-bgp

**Run**

    ansible-playbook -i hosts leaf.yaml

(c) 2017 Dell EMC
