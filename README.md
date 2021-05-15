The files in this repository were used to configure the network depicted below.

![image](https://user-images.githubusercontent.com/78704210/118347017-8a9de680-b505-11eb-86a1-1f7b72627b5d.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the YAML file may be used to install only certain pieces of it, such as Filebeat.

- filebeat-playbook.yml
- filebeat-config.yml
- install-elk.yml
- pentest.yml
- filebeat.yml

This document contains the following details:

- Description of the Topology
- Access Policies
- ELK Configuration
    - Beats in Use
    - Machines Being Monitored
- How to Use the Ansible Build

Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
Load balancing ensures that the application will be highly secured and available, in addition to restricting in-bound traffic to the network.

-One way to mitigate DoS attacks is to have multiple servers running the same website, with a load balancer in front of them.
-the advantage of a jump box: it's essentially identila to a gateway router, exposed to the public internet, sits in front of other VMs that are not exposed to the public internet; it controls accessto the other machines that are not exposed to the public internet.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jumpbox and system network.

What does Filebeat watch for?
- file changes on system logs on the machine
What does Metricbeat record?
- metrics on operating system and from services running on the server.

The configuration details of each machine may be found below.

Name              Function            IP Address                        Operating System
Jump Box          Gateway             40.121.39.52(10.2.0.4)            Linux
Web1              VM                  10.2.0.5                          Linux
Web2              VM                  10.2.0.6                          Linux
Web3              VM                  10.2.0.7                          Linux
Elk-server        monitoring          13.89.72.48 (10.0.0.4)            Linux

Access Policies
The machines on the internal network are not exposed to the public Internet.
Only the Jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 5601 Kibana

Machines within the network can only be accessed by Jump box.

Which machine did you allow to access your ELK VM? What was its IP address?
- My IP: 192.168.0.103

A summary of the access policies in place can be found in the table below.

Name          Publicly Accessible         Allowed IP Addresses
Jump box      yes                         192.168.0.103
Web1          no                          10.0.0.4
Web2          no                          10.0.0.4
Elk-server    no                          10.0.0.4


Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

What is the main advantage of automating configuration with Ansible?
- Free, simple, flexible, powerful, agentless

The playbook implements the following tasks:

In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc.
- in install-elk.yml:
    - install docker.io
    - install python3-pip
    - install Docker module
    - increase virtual memory
    - use more memory
    - download and launch a docker elk container
    - enable service docker on boot
    
The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.
- azadmin@Elk-to-Red1:~$ sudo docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED      STATUS       PORTS                                                                              NAMES
72511e3569bf   sebp/elk:761   "/usr/local/bin/star…"   7 days ago   Up 3 hours   0.0.0.0:5044->5044/tcp, 0.0.0.0:5601->5601/tcp, 0.0.0.0:9200->9200/tcp, 9300/tcp   elk

Target Machines & Beats
This ELK server is configured to monitor the following machines:
- web1 10.2.0.5
- web2 10.2.0.6

We have installed the following Beats on these machines:
- Microbeats

These Beats allow us to collect the following information from each machine:
- Filebeat:collect system logs data about the files
- Metribeat: collect metrics on operating system

Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:
SSH into the control node and follow the steps below:

Copy the playbooks file to ansible .
Update the hosts file to include private IP address of VMs and elk 
Run the playbook, and navigate to Kibana (http://40.121.39.52:5601/app/kibana) to check that the installation worked as expected.


As a Bonus, provide the specific commands the user will need to run to download the playbook, update the files, etc.
- ansible-playbook install-elk.yml
- dpkg -i filebeat-7.4.0-amd64.deb
- ansible-playbook filebeat-playbook.yml
  - filebeat modeles enable system
  - filebeat setup
  - service filebeat start
- dpkg -i metricbeat-7.4.0-amd64.deb
- ansible-playbook metricbeat-playbook.yml
  - metricbeat modules enable docker
  - metricbeat setup
  - matricbeat -e
