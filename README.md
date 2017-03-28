# Ansible Role: cnos-vlag-2tier-leaf - Multiple Layer vLAG Leaf Configuration
---
<add role description below>

This is a template driven playbook for a multiple layer vLAG configuration. The role applies to the access switches that are downstream from the vLAG peers. These switches will be referred to as leaf switches.

The templates used in this configuration example are the same. They will be used to configure the leaf switches. The templates will be executed with different settings suitable to each leaf switch. Since there are two leaf switches, there will be two sets of values used in the templates.

The Ansible modules will execute or skip tasks given in the playbook depending on the flag setup of each IP address.

The configuration commands and their results can be verified in the *commands* and *results* directories.

For more details, see [Configuring a multiple layer vLAG network](http://systemx.lenovofiles.com/help/index.jsp?topic=%2Fcom.lenovo.switchmgt.ansible.doc%2Fconfiguring_a_multiple_layer_vlag_using_ansible.html&cp=0_3_1_0_7).


## Requirements
---
<add role requirements information below>

- Ansible version 2.3 or later ([Ansible installation documentation](http://docs.ansible.com/ansible/intro_installation.html))
- Lenovo switches running CNOS version 10.2.1.0 or later
- an SSH connection to the Lenovo switch (SSH must be enabled on the network device)


## Role Variables
---
<add role variables information below>

The values of the variables used need to be modified to fit the specific scenario in which you are deploying the solution. To change the values of the variables, you need to visits the *vars* directory of each role and edit the *main.yml* file located there. The values stored in this file will be used by Ansible when the template is executed.

The syntax of *main.yml* file for variables is the following:

```
<template variable>:<value>
```

You will need to replace the `<value>` field with the value that suits your topology. The `<template variable>` fields are taken from the template and it is recommended that you leave them unchanged.

Available variables are listed below, along with description:

Variable | Description
--- | ---
`username` | Specifies the username used to log into the switch
`password` | Specifies the password used to log into the switch
`flag` | Configures the condition flag associated with the switch
`stp_mode1` | Configures the STP mode (**mst** - MSTP, **rapid-pvst** - Rapid PVST+, **disable** - STP is disabled)
`slot_chassis_number1` | Specifies the ethernet port (*slot number/port number*)
`portchannel_interface_number1` | Specifies the LAG number (*1-4096*)
`portchannel_mode1` | Configures the LAG type (**on** - static LAG, **active** - active member of a LACP LAG, **passive** - passive member of a LACP LAG)
`slot_chassis_number2` | Specifies the ethernet port (*slot number/port number*)
`switchport_mode1` | Configures the switch port mode (**access** - the port can be part of only a single VLAN, **trunk** - the port can be part of any number of VLANs)


## Dependencies
---
<add dependencies information below>

- username.iptables - Configures the firewall and blocks all ports except those needed for web server and SSH access.
- username.common - Performs common server configuration.
- /etc/ansible/hosts - You must edit the */etc/ansible/hosts* file with the device information of the switches designated as leaf switches. You may refer to *cnos-vlag-2tier-leaf-hosts* for a sample configuration.

Ansible keeps track of all network elements that it manages through a hosts file. Before the execution of a playbook, the hosts file must be set up.

Open the */etc/ansible/hosts* file with root privileges. Most of the file is commented out by using **#**. You can also comment out the entries you will be adding by using **#**. You need to copy the content of the hosts file for the role into the */etc/ansible/hosts* file. The sample hosts file for the role is located in the main directory.

```
[cnos-vlag-2tier-leaf]
10.240.178.74   username=<username> password=<password> deviceType=g8272_cnos condition=leaf_switch1
10.240.178.75   username=<username> password=<password> deviceType=g8272_cnos condition=leaf_switch2
```
**Note:** You need to change the IP addresses to fit your specific topology. You also need to change the `<username>` and `<password>` to the appropriate values used to log into the specific Lenovo network devices.


## Example Playbook
---
<add playbook samples below>

To execute an Ansible playbook, use the following command:

```
ansible-playbook cnos-vlag-2tier-leaf.yml -vvv
```

`-vvv` is an optional verbos command that helps identify what is happening during playbook execution. The playbook for each role of the multiple layer vLAG configuration solution is located in the main directory of the solution.

```
- hosts: cnos-vlag-2tier-leaf
  roles:
    - cnos-vlag-2tier-leaf
```


## License
---
<add license information below>
Copyright (C) 2017 Lenovo, Inc.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
