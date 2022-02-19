## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Network Diagram](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/diagrams%20and%20images/ELK_and_RedTeam_Network_Diagram.jpg)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the .yml playbook file may be used to install only certain pieces of it, such as Filebeat.

- [Install DVWA Playbook](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/ansible/pentest.yml.txt)
- [Install Elk Playbook](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/ansible/install-elk.yml.txt)
- [Install Filebeat & Metricbeat Playbook](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/ansible/roles/install-filebeat-and-metricbeat.yml.txt)

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

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Load balancers help maintain the availability of websites because they direct traffic to the webserver with the best health status in the load balancer's backend pool.  This means users should always have access to the contents of the webservers provided at least one of the machines in the backend pool is functional.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A Jump Box with an ansible control node is used to administrate the other machines on the network.  Jump Boxes are useful because you can design network security group rules accordingly and limit the availability of access to the machines' file systems (through SSH) to require credentials which are only stored on the Jump Box.  This puts the credentials to the other machines behind a firewall (network security group) for added security.  Access to the Jump Box is only permitted from select/whitelisted IP Addresses from the internet and thus the other machines on the network do not need to have port 22 exposed to the public internet, but instead only to the JumpBoxProvisioner which is on their local or peer networks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files **(filebeat)** and system metrics **(metricbeat)**.

- **Filebeat**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Filebeat is a lightweight shipper for forwarding and centralizing log data. Installed as an agent on your servers, Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.

- **Metricbeat**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash. Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server, such as: Apache.

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
- Installs docker.io, forces update of docker to the most recent version.
- Installs python3-pip and forces updated to the most recent version, uses pip to install the Docker module.
- Increases the virtual memory.  Tells the system to use the additional memory to run the ELK container (requires 3.5gb of RAM to run correctly).
- Uses the docker_container module to download and launch an elk container.  Identifies and maps the ports required for elk to run, 5601, 9200 and 5044.
- Sets the system to enable the docker service and elk container on boot up so the container is also restarted when the machine is restarted.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker PS Output](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/diagrams%20and%20images/elkvm_docker_ps_output.jpg)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5 (Web-1)
- 10.0.0.6 (Web-2)
- 10.0.0.7 (Web-3)

We have installed the following Beats on these machines:
- Web-1: Filebeat & Metricbeat
- Web-2: Filebeat & Metricbeat
- Web-3: Filebeat & Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat monitors specified log files for changes or events.  Metricbeat monitors the systems for system statistics like CPU usage and memory usage.  When setup properly these beats will display data to their respective Kibana dashboards and will allow easy administration of the Web VMs from both security and performance standpoints.

### Using the install-elk.yml Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible/*.
- Update the hosts file (in /etc/ansible/) to include the internal IP address of the elk machine under the header [elk].  It should look like this:
```
[elk]
10.1.0.4 ansible_python_interpreter=/usr/bin/python3
```
NOTE: your Elk-VM's internal IP may be different than in the above example.  Please adjust your hosts file text accordingly.
- Run the playbook, 
```
ansible-playbook install-elk.yml
```
and navigate to the public IP of the Elk-VM in a web browser.  Be sure to include the port (:5601) and the following text: "/app/kibana".  It should look like this:  http://[Elk-VM-Public-IP]:5601/app/kibana, or in the context of the [network](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/diagrams%20and%20images/ELK_and_RedTeam_Network_Diagram.jpg) pictured above: http://20.122.91.6:5601/app/kibana.

### Using the install-filebeat-and-metricbeat.yml Playbook

SSH into the control node and follow the steps below:
- Copy the [filebeat-config.yml](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/ansible/files/filebeat-config.yml) and [metricbeat-config.yml](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/ansible/files/metricbeat-config.yml) files to the **/etc/ansible/files/** directory.
- Edit the filebeat-config.yml and metricbeat-config.yml files to point to the internal IP address of your Elk-VM.
- filebeat-config.yml
```
1105  hosts: ["10.1.0.4:9200"]
1106  username: "elastic"
1107  password: "changeme"
```
```
1804  setup.kibana:
1805    host: "10.1.0.4:5601" # TODO: Change this to the IP address of your ELK server
```
- metricbeat-config.yml
```
61  setup.kibana:
62    host: "10.1.0.4:5601"
```
```
96  hosts: ["10.1.0.4:9200"]
97  username: "elastic"
98  password: "changeme"
```  
- If you prefer to use non-default credentials for your installation update these files accordingly to ensure the credentials match those of your own installation.
- Copy the [install-filebeat-and-metricbeat.yml](https://github.com/mchovde/GWU_Bootcamp_Project_1_Elk_Stack/blob/main/ansible/roles/install-filebeat-and-metricbeat.yml.txt) to the **/etc/ansible/roles/** directory.
- Make sure the hosts file has been updated to include the internal IP of your Web-VMs (should already have been done to install DVWA) under the heading [webservers]:
```
[webservers]
10.0.0.5 ansible_python_interpreter=/usr/bin/python3
10.0.0.6 ansible_python_interpreter=/usr/bin/python3
10.0.0.7 ansible_python_interpreter=/usr/bin/python3
```
- Run the install-filebeat-and-metricbeat.yml file from the /etc/ansible/roles/ directory with the following command:
```
ansible-playbook install-filebeat-and-metricbeat.yml
```
