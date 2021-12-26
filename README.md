# Elastic-Stack-Project
Azure cloud network set up and deployment, with ansible and multiple VMs set up to run a Web Application. Elastic Stack set up and configuration to monitor the cloud network.
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

~/Elastic-Stack-Project/Diagrams/ELK-Stack-Project.drawio.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Ansible folder may be used to install only certain pieces of it, such as Filebeat.

  
~/Elastic-Stack-Project/Ansible/My-playbook1.yml
~/Elastic-Stack-Project/Ansible/Elk-playbook.yml
~/Elastic-Stack-Project/Ansible/Filebeat-playbook.yml
~/Elastic-Stack-Project/Ansible/Metricbeat-config.yml


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant and available, in addition to restricting access to the network.
- The load balancer prevents network failure due to high traffic, it increases defenses against a DDoS attack (distributed denial of service) – The advantage of the jumpbox is that it creates a one path gateway to the network (access control). That decreases the amount of monitoring needed since we only have to look at one vm, for intrusion prevention (segmentation).

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
-What does Filebeat watch for? Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
-What does Metricbeat record? Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function  | IP Address     | Operating System |
|----------|---------- |----------------|------------------|
| Jump Box | Gateway   | 10.0.0.4       | Git Bash         |
| Web1     | Web Server| 10.0.0.5       | Git Bash         |
| Web2     | Web Server| 10.0.0.6       | Git Bash         |
| ELK      | Elk Server| 10.1.0.4       | Git Bash         |
| Load Bal.| Load Bal. | Static Ext. IP | Git Bash         |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the ELK Server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Workstation Public IP through TCP:5601

Machines within the network can only be accessed by Workstation and Jumpbox Provisioner.
- Which machine did you allow to access your ELK VM? What was its IP address? Jumpbox Provisioner IP: 10.0.0.4 via SSH port 22 and Workstation Public IP via TCP port 5601

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses       |
|------------|---------------------|----------------------------|
| Jump Box   | No                  | Workstation Public IP      |
|            |                     | on SSH 22 with Private Key |
| Web1       | NO                  | 10.0.0.4 on SSH 22 with    |
|            |                     | Private Key                |
| Web2       | No                  | 10.0.0.4 on SSH 22 with    |
|            |                     | Private Key                |
| ElK Server | No                  | Workstation Public IP on   |
|            |                     | TCP 5601                   |
| Load Bal.  | No                  | Workstation Public IP on   |
|            |                     | HTTP 80                    |
                     

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because Ansible lets you quickly and easily deploy multi-tier apps. There is no need to write custom code to automate the systems, by listing the required tasks to be done on a playbook, Ansible will figure out how to get your system running to the state and specs you want them in. 

The playbook implements the following tasks:
-- name: Configure Elk VM with Docker
   hosts: elk
   remote_user: azadmin
   become: true
   tasks:

- Increase System Memory:
   - name: Use more memory
     sysctl:
	 name: vm.max_map_count
	 value: "262144"
         state: present
	 reload: yes  

- Install Python3
   - name: Install python3-pip
        apt:
	  force_apt_get: yes
	  name: python3-pip
	  state: present

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1: 10.0.0.5
- Web 2: 10.0.0.6

We have installed the following Beats on these machines:
-	ELK Server, Web 1, and Web 2.
-	The ELK Stack Installed are: FileBeat and MetricBeat.

These Beats allow us to collect the following information from each machine:
- FileBeat ships log files to logstash. Log events such as http Headers
- MetricBeat is a lightweight agent that can be installed on target servers to periodically collect metric data from a target server. Mainly operating system metrics such as CPU or memory data related to services running on the server.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the elk-playbook.yml file to ansible container.
- Update the FileBeat-playbook.yml file to include installer
- Run the playbook, and navigate to Kibana > Logs : Add log data > System logs > 5:Module Status > Check data to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
