# Project-1
Cybersecurity Project 1
## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.

     Diagrams/AzureELKStackDeployment


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.

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
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_

A load balance is configured on this network to distribute incoming traffic across two servers (Web-1 and Web-2), providing security and resiliency. It provides resiliency by re-routing traffic to only the healthy server if one server is attacked (DDoS)or is otherwise unavailable. The load balancer knows if a server is available and healthy by monitoring each with a health probe. The health probe declares a server unhealthy if two consecutive probes sent five seconds apart fail. Should that occur, all traffic will be sent to the remaining operational server.

The load balancer also controls access to the machines "behind it" by forwarding all allowed HTTP traffic on port 80 from its public IP address to the virtual NIC addresses of the VMs it protects. It has a public IP address (13.89.219.43), and a back-end pool of two servers (Web-1 and Web-2) with virtual NIC addresses 10.0.0.5 and 10.0.0.6. 
 
A jumpbox is configured on the perimeter network to protect and add security between the private and untrusted public networks. The jumpbox increases network security by controlling administrative access to the other servers (e.g., Web-1, Web-2). With this network configuration, administrative access to the servers can only be accomplished by logging into and passing the security checks on the jumbox. Direct administrative access to other servers is not possible. All inbound traffic from the Internet to the jumpbox is denied, except SSH traffic on port 22 coming from my workstation IP(64.83.233.214). The jumpbox will accept inbound traffic from the load balancer, Web-1 and Web-2 (virtual network). The jumpbox sends outgoing SSH traffic on port 22 to the virtual network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the metrics and statistics_ and system logs.

The ELK server provides centralized logging that identifies problems with servers and applications. It consists of Elasticsearch (stores logs), Logstash (ships, processes, and stores logs), and Kibana (visualization tool). Filebeat and Metricbeat are components of the ELK stack. They are agents installed on the machine(s) monitored (Web-1 and Web-2). They forward logs to Logstash for processing or into Elasticsearch for indexing. Filebeat handles audit logs, depreciation logs, gc logs, server logs, and slow logs. Metricbeat collects metrics from the operating system and services running on the monitored servers.

In this configuration, Filebeat and Metricbeat are installed on Web-1 and Web2 using YAML configuration files. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name               | Function    | IP Address    | Operating System    |
|--------------------|-------------|---------------|---------------------|
| JumpboxProvisioner | Gateway     | 40.122.247.46 | Ubuntu Server 18.04 |
| Web-1              | DVWA        | 10.0.0.5      | Ubuntu Server 18.04 |
| Web-2              | DVWA        | 10.0.0.6      | Ubuntu Server 18.04 |
| ELK                | monitor     | 13.77.209.54  | Ubuntu Server 18.04 |
| Loadbalancer       | distributor | 13.89.219.43  | Ubuntu Server 18.04 |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the __Loadbalancer__ machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
	64.83.233.214 – my workstation
	10.0.0.4 – JumpboxProvisioner


Machines within the network can only be accessed by _JumpboxPorvisioner at external IP address 40.122.247.48 and internal IP address 10.0.0.4
A summary of the access policies in place can be found in the table below.

| Name               | Publicly Accessible   | Allowed IP Addresses  |
|--------------------|-----------------------|-----------------------|
| JumpboxProvisioner | Yes, only me via SSH  | 64.83.233.214         |
| Loadbalancer       | Yes, only me via HTTP | 64.83.233.214         |
| Web-1              | No                    | 10.0.0.5              |
| Web-2              | No                    | 10.0.0.6              |
| ELK Server         | Yes, only me via HTTP | 64.83.233.214         |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_

Ansible provides an automated method for deploying software, updates, or applications to many machines, improving efficiencies by saving staff time. To use Ansible, the administrator must create an Ansible playbook of tasks and provide a list of IP addresses upon which the tasks should be performed. Ansible then determines how to get the tasks done for each machine. The playbook is human-readable and contains infrastructure data descriptions. 
As an example, Ansible can be used to distribute sitewide security policies. All nodes associated with the same security policies can be updated by writing one playbook containing the policy details and using SSH to communicate with the machines. 

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

- Install Docker
- Install Docker python module
- Increase the memory available for use by the application
- Download and install a docker ELK container
- Enable Docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Diagrams/docker_ps_output.png 



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
	10.0.0.5
	10.0.0.6

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

	Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

Filebeat looks at audit logs, depreciation logs, gc logs, server logs, and slow logs. Metricbeat collects metrics from the operating system and 
services running on the monitored servers (e.g., Apache, MySQL. Filebeat uses a harvester to monitor log files and forwards new content to Logstash
 for aggregating or into Elasticsearch for indexing. Metricbeat inserts collected metrics into Elasticsearch or sends them to Logstash, Redis, or Kafk.a 


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible _____.
- Update the Ansible’s host__ file to include the ELK stack as a group
1. - Run the playbook, and navigate to ELK Server (10.1.0.0) to check that the installation worked as expected.


_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

Download the playbook
Update the ansible host file
Run the playbook
Verify it worked

SSH to the control node (JumpboxProviser)
       ssh azuresuer@40.122.247.46

Start and attach to the docker container:
       sudo docker container list -a   <- this shows container name
       sudo docker start <your container name>
       sudo docker attach <your container name>  <- gets a shell in your container


Copy the Ansible playbook (install-elk.yml). From the container:

	touch /etc/ansible/install-elk.yml
       nano /etc/ansible/install-elk.yml

Copy and paste install-elk.yml from github repository and save install-elk.yml
       

Add the ELK stack to the Ansible’s hosts file as a group. From the container:

	nano /etc/ansible/hosts

Add:
[elk]
 10.1.0.4 ansible_python_interpreter=/usr/bin/python3

Save the file

Run the playbook from the container:
	
ansible-playbook /etc/ansible/install-elk.yml


To verify the playbook was successful,  SSH into the ELK server and ensure that the sebp/elk:761 container is running:

     ssh azureuser@10.1.0.0  
     sudo docker ps


 

