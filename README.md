## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/ELK_and_RedTeam_Network_Diagram.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml playbook file may be used to install only certain pieces of it, such as Filebeat.

- [Install DVWA Playbook](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/Playbooks/Install-DVWA.yml.txt)
- [Install Elk Playbook](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/Playbooks/Install-Elk.yml.txt)
- [Install Filebeat & Metricbeat Playbook](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/Playbooks/Install-Filebeat-and-Metricbeat.yml.txt)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

Load balancers help maintain the availability of websites because they direct traffic to the webserver with the best health status in the load balancer's backend pool.  This means users should always have access to the contents of the webservers provided at least one of the machines in the backend pool is functional.

A Jump Box with an ansible control node is used to administrate the other machines on the network.  Jump Boxes are useful because you can design network security group rules accordingly and limit the availability of access to the machine's file systems (through SSH) to require credentials which are only stored on the Jump Box.  This puts the credentials to the other machines behind a firewall for added security.  Access to the Jump Box is only permitted from selected whitelisted IP Addresses from the internet and thus the other machines on the network do not need to have port 22 exposed to the public internet, but instead only to the JumpBoxProvisioner which is on their local or peer networks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system files.
- _TODO: What does Filebeat watch for?_
- _TODO: What does Metricbeat record?_

The configuration details of each machine may be found below.

| Name               | Function  | IP Address | Operating System                   |
|--------------------|-----------|------------|------------------------------------|
| JumpBoxProvisioner | Gateway   | 10.0.0.4   | Linux UnbuntuServer 18_04-lts-gen2 |
| Web-1              | Webserver | 10.0.0.5   | Linux UnbuntuServer 18_04-lts-gen2 |
| Web-2              | Webserver | 10.0.0.6   | Linux UnbuntuServer 18_04-lts-gen2 |
| Web-3              | Webserver | 10.0.0.7   | Linux UnbuntuServer 18_04-lts-gen2 |
| Elk                | ElkStack  | 10.1.0.4   | Linux UnbuntuServer 18_04-lts-gen2 |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBoxProvisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- [my public ip]

Machines within the network can only be accessed by SSH.

Elk, Web-1, Web-2 and Web-3 can *only be accessed through the JumpBoxProvisioner machine* with the internal IP
of **10.0.0.4.**  The SSH credentials for Elk, Web-1, Web-2 and Web-3 are stored in the Ansibile Control Node on the
JumpBoxProvisioner.  As such, it is the only machine with the credentials to access the other 4 machines on the
network.

A summary of the access policies in place can be found in the table below.

| Name               | Publicly Accessible SSH (Port 22) | Allowed IP Addresses | Publicly Accessible Other Ports | Allowed IP Addresses |
|--------------------|-----------------------------------|----------------------|---------------------------------|----------------------|
| JumpBoxProvisioner | YES                               | [my public ip]       | NO                              | N/A                  |
| Web-1              | NO                                | 10.0.0.4             | YES: 80                         | [my public ip]       |
| Web-2              | NO                                | 10.0.0.4             | YES: 80                         | [my public ip]       |
| Web-3              | NO                                | 10.0.0.4             | YES: 80                         | [my public ip]       |
| Elk                | NO                                | 10.0.0.4             | YES: 5601                       | [my public ip]       |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
Ansible's playbooks allow anyone to recreate the ecosystem in a seamless and efficient way and without the specific
technical knowledge required to set up the machines individually.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.5
10.0.0.6
10.0.0.7

We have installed the following Beats on these machines:
Web-1: Filebeat & Metricbeat
Web-2: Filebeat & Metricbeat
Web-3: Filebeat & Metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
