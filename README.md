## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/Calanis77/Cybersecurity/tree/main/Diagrams#:~:text=Cloud%20Security.drawio.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - https://github.com/Calanis77/Cybersecurity/tree/main/Ansible#:~:text=filebeat%2Dplaybook.yml
  - https://github.com/Calanis77/Cybersecurity/tree/main/Ansible#:~:text=metricbeat%2Dplaybook.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly responsive, in addition to restricting analyzes to the network. What aspect of security do load balancers protect? What is the advantage of a jump box?
- Load Balancers protect against denial of service attacks (DDoS) because the load balancer analyzes the traffic coming in and determines what server to send the traffic to. This prevents one server from getting overloaded with traffic because the load balancer allows traffic to be distributed evenly among the servers that are connected to it. It is also common for load balancers to have a health probe that periodically checks that a machine is working properly before sending traffic. If it isn't, the load balancer will divert traffic from the malfunctioning server until the issue is resolved. A jump box limits the access that the public has to your virtual network because in order to access the other virtual machines, an individual needs the private IPs of the machines. A jumpbox allows greater control over access to a virtual network and its contents

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- What does Filebeat watch for? Filebeat looks for changes in files and when they occurred by looking at log files because Filebeat generates and organizes log files.
- What does Metricbeat record? Metricbeat records metrics from the Operating system and services running on the server. By using Elasticsearch or Logstash, you can visualize the metrics and statistics that Metricbeat generates from the OS and running services.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux Ubuntu     |
| Web- 1   |Web Server used to run DVWA| 10.0.0.5 |Linus Ubuntu|
| Web- 2   |Web Server used to run DVWA| 10.0.0.6 |Linux Ubuntu|
| Elk      |Run Elk Container & Cabana | 10.1.0.4 |Linux Ubuntu|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 23.127.169.112,67.11.207.101

Machines within the network can only be accessed by Jump Box.
-Which machine did you allow to access your ELK VM? Jump Box 
-What was its IP address? 40.86.0.126

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes.                | 40.86.0.126.         |
| Web-1    | No                  | 10.0.0.5             |
| Web-2    | No                  | 10.0.0.6             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- It prevented having to configure ELK manually resulting in a more streamlined process. It also allowed more control over what was being installed and/or performed on the machine

The playbook implements the following tasks:
- The first task of the elk playbook installs docker.io on the Elk virtual machine
-Python is then installed on the Elk VM
-Elk requires more virtual memory so this task increases the memory to 8GB
-This task tells the Elk machine use the 8GB as the amount of memory defined in the previous task
-Downloads, installs and executes the docker elk container on the Elk vm on restart so the elk container doesn't need to be manually started
-This enables docker on boot so you don't have to manually start docker when you turn your VM back on.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/Calanis77/Cybersecurity/blob/main/Diagrams/Screen%20Shot.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1: 10.0.0.5 Web-2: 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc. Filebeat collects log information about the file system and specifies which files have been changed and when a file was changed to either Elasticsearch or Logstash. If you wanted to see the output from Filebeat, you would connect to Kibana and check the logs for any changes that have been made to the file system over a specific or more general time interval. For example, you would specify the container, the beat and the argument you are looking for. You can further specify by date and time range. Metricbeat shows statistics for the every process running on your system including memory, CPU usage, file system, Network IO and disk IO statics. To view the data collected, you would navigate to Kibana (assuming that your VM is already running), select the system you'd like to review and from there can view the metrics of the system.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible/files/filebeat-config.yml
- Update the filebeat-config.yml file to include host "10.1.0.4:9200" with username "elastic" and password "changeme" and setup.Kibana host to "10.1.0.4:5601".
- Run the playbook, and navigate to Kibana (Elk GUI interface) and click "Check Data" to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- /etc/ansible/roles/filebeat.playbook.yml
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- Filebeat-config.yml How do I specify which machine to install the ELK server on versus which to install Filebeat on? First, all private IP addresses that need to be accessed need to be added to the hosts file in order to allow connection. For the purposes of this project, we added 3 IP addresses to this file: 2 were web servers (10.0.0.5 & 10.0.0.6) and an elk server was added with IP 10.1.0.4. From there, in the playbook file, navigate to "hosts" at the top of the file to specify whether you want the playbook installed on Elk or the Web servers. In addition to this, we also have to specify the host to the ELK server for Filebeat so IP "10.1.0.4" is added to the config file to specify the location for installation.- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
