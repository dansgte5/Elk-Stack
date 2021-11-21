## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

[Install Elk Playbook](https://github.com/dansgte5/Elk-Stack/blob/main/Ansible/Install%20Elk.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting traffic to the network.
- DDoS attacks
- Protection from emerging threats
- Authenticates users for access
- Simplifies PCI compliance In addition to the added layer of security provided by the Load Balancer, the Jump Box:
- Acts as a "bridge" between two trusted networks and is treated as a single entryway to a server group

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
- Filebeat: Watches and records log files from locations specified and forwards them to Logstash for indexing.
- Metricbeat: Monitors and collects data such as system CPU and memory to load in a docker environment.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| WebVM-1  | WebVM    | 10.0.0.5   | Linux            |
| WebVM-2  | WebVM    | 10.0.0.6   | Linux            |
| WebVM-3  | WebVM    | 10.0.0.7   | Linux
| ELK VM   | ELK Stack| 10.1.0.5   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
24.140.176.26

Machines within the network can only be accessed by Jumpbox Provisioner.
The Jumpbox-Provisioner is the only machine I allowed access to the ELK Server. The Private IP of the Jumpbox-Provisioner is used to access the machines within the nextwork.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 24.140.176.26        | 
| WebVM-1  | No                  | 10.0.0.5             |
| WebVM-2  | No                  | 10.0.0.6             |
| WebVM-3  | No                  | 10.0.0.7 
| ELK VM   | No                  | 10.1.0.5

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
it allows us to simplfy tasks freeing up time and increasing efficiency

The playbook implements the following tasks:
- Install Docker.10
- Install Python3-pip
- Install Docker Module
- Increase Virtual Memory
- Use More Memory
- Download and Launch a Docker Elk Container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[Elk Server Docker Image](/Users/danielnordick/Desktop/Elk-Stack/Images/Elk Stack Docker Image.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- WebVM-1 10.0.0.5
- WebVM-2 10.0.0.6
- WebVM-3 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat 
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat allow for the monitoring of log files 
- Metricbeat allos for the monitoring of server's metrics such as inbound and outbound traffic.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Nano into hosts file in order to run the playbooks to run on specified machines. There you are going to locate webservers, updating by entering the web-vm's IP address as follows:
- 10.0.0.5 ansible_python_interpreter=/usr/bin/python3
- 10.0.0.6 ansible_python_interpreter=/usr/bin/python3
- 10.0.0.7 ansible_python_interpreter=/usr/bin/python3
- Ansible_python_interpreter=/usr/bin/python3' specifies the container that it will be running.
- Make sure to un-comment [webservers].
- Next you will create a new group called '[elk]' underneath '[webservers]', update that section by entering the ELK-Server's private IP as follows:
- 10.1.0.5 ansible_python_interpreter=/usr/bin/python3
- Copy the Filebeat.cfg file to the /etc/ansible/ directory inside the ansible container.
- Update the Filebeat.cfg file to include the Elk Server's Private IP on the following lines:
- line 1105 'hosts: ["10.1.0.5:9200"]'
- line 1806 'host: "[10.1.0.4:5601"]'
- Run the playbook Filebeat.yml located in the /etc/ansible/roles/ directory inside the ansible container, then navigate to Kibana using your web browser by inputing the URL http://40.77.107.185.133:/app/kibana which would be the ELK-Server public IP, specifying port '5601' to check that the installation worked as expected.
- Copy the Metricbeat.cfg file to /etc/ansible/ directory inside the ansible container.
- Update the Metricbeat.cfg file in the /etc/ansible/ to include the Elk-Server's private IP on lines 62 and 95 as follows:
- line 62 'host: "[10.1.0.5:5601"]'
- line 95 'hosts: ["10.1.0.5:9200"]'

Run the playbook, Metricbeat.yml inside the /etc/ansible/roles directory, and navigate to Kibana on your web browser using the URL http:40.77.107.185//:/app/kibana ELK-Server public IP, specifying port '5601' to check that the installation worked as expected.
_TODO: Answer the following questions to fill in the blanks:_
- /Users/danielnordick/Desktop/Elk-Stack/Images/Kibana.png
- /Users/danielnordick/Desktop/Elk-Stack/Images/Filebeat.png
- /Users/danielnordick/Desktop/Elk-Stack/Images/Metricbeat.png

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
