## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

Diagrams/michael-liang-network-diagram.jpg

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook files may be used to install only certain pieces of it, such as Filebeat.

  Ansible/install-elk.yml

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
Load balancers protect the accessibility aspect of security. A jump box acts as a trusted machine that is then utilized to gain connection to more sensitive systems. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the jump box and system network.
Filebeat watches for log data and collects log events. 
Metricbeat collects metric data from the OS and other services that are being used on the server. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function  | IP Address   | Operating System |   |
|----------|-----------|--------------|------------------|---|
| Jump Box | Gateway   | 13.90.43.191 | Linux            |   |
| ELK-VM   | Webserver | 20.83.202.36 | Linux            |   |
| Web-1    | Webserver | 10.0.0.5     | Linux            |   |
| Web-2    | Webserver | 10.0.0.6     | Linux            |   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jump box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
98.116.179.131

Machines within the network can only be accessed by SSH.
We allowed the jump box (IP 13.90.43.191) to access the ELK VM by way of SSH and we allowed the host PC to access the ELK VM by way of port 5601 (IP 98.116.179.131).

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses     |
|----------|---------------------|--------------------------|
| Jump Box | Yes                 | 98.116.179.131           |
| ELK-VM   | No                  | 10.0.0.4, 98.116.179.131 |
| Web-1    | No                  | 10.0.0.4, 98.116.179.131 |
| Web-2    | No                  | 10.0.0.4, 98.116.179.131 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
Ansible is open source and is a very efficient tool that can carry out tasks in a streamlined manner. Needing to put together a playbook allows step-by-step review of tasks to be carried out, making it a more organized way of configuring virtual machines.

The playbook implements the following tasks:
-increase virtual memory usage to max
-install docker.io
-install python3-pip
-install python module docker
-download and install docker container
-enable docker container at start-up

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Diagrams/michael-liang-elk-docker-ps.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
Web-1: 10.0.0.5
Web-2: 10.0.0.6

We have installed the following Beats on these machines:
FileBeat and MetricBeat

These Beats allow us to collect the following information from each machine:
Filebeat watches for and collects log data. Metricbeat collects metric data from the operating system and other services currently in use by the system.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- First, copy install-elk.yml into /etc/ansible
- Update the hosts file located in /etc/ansible to include IPs 10.0.0.5 and 10.0.0.6 under [webservers] and 10.1.0.5 under [elk]. Be sure to include ansible_python_interpreter=/usr/bin/python3 after each IP entry
- Run the playbook with "ansible-playbook install-elk.yml"
- SSH into the ELK-VM and run "sudo docker ps" to ensure the elk container was properly installed and is running
- After ensuring the elk container is up and running, navigate to http://20.83.202.36:5601/app/kibana on your local PC to access Kibana

- The filebeat playbook is named filebeat-playbook.yml and should be copied to /etc/ansible/roles and the metricbeat playbook is named metricbeat-playbook.yml, also to be copied to /etc/ansible/roles
- /etc/ansible/hosts within the Ansible control node needs to be updated with the correct IPs of the VMs as stated above. Within the playbook files, you specify the machines you want the playbook to run on after "hosts:". For Web-1 and Web-2, it would be "hosts: webservers" and for the ELK-VM, it would be "hosts: elk"
- Navigate to http://20.83.202.36:5601/app/kibana to confirm that the ELK server is running.

To run the playbooks, within your Ansible control node, run:
ansible-playbook filebeat-playbook.yml
ansible-playbook metricbeat-playbook.yml