# AWS-Ansible-Elk-and-Filebeat
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Alt text](https://raw.githubusercontent.com/Iancritchlow/AWS-Ansible-Elk-and-Filebaet/main/Diagrams/vpc%20diagram%20(2).jpg)


These files have been tested and used to generate a live ELK deployment on AWS. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml file may be used to install only certain pieces of it, such as Filebeat.

  - install-elk.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly efficient, in addition to restricting incoming traffic to the network.
Load balancers will help protect against the aspects of SQL injection and cross site scripting. This Jump Box will provide a high level of security to the databases and webservers by making them inaccessible to the public internet. It will accomplish by using a complicated/generated key or .pem file and using the SSH protocols. The Jump Box will also house and run the needed containers and images to allow the webservers and databases to comminute.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- When Filebeat is deployed it will watch for log files/locations and simplify the data to be reviewed.
- When metricbeat is added/deployed it will record/statistical data from the operating system it will also record services running on the server, it will
also collect metrics and running services from the CPU to memory. Redis to NGINX and much more. it is an efficient light weight way to send system and services and statistics. Although not deployed in the network this is a highly recommended practice.   

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.10.0.27 | Linux ec2        |
| DVWA     |Webserver | 10.10.2.94 | Ubuntu           |
| DVWA2    |Webserver |10.10.2.124 | Ubuntu           |
| ELK      |Logger    |10.10.2.31  | Ubuntu           |
| RDP      |Viewing   |10.10.0.124 | Windows          |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box and RDP machines can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: The two webservers would be considered whitelisted
ip 10.10.2.94
ip 10.10.2.124

Machines within the network can only be accessed by SSH protocol for Linux and RDP secure for windows.
- The Jump Box is the only machine I allowed to access your ELK VM? With the following IP address 10.10.0.27

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | 10.0.0.1 10.0.0.2    |
| DVWA     | NO                  | 10.10.2.94           |
| DVWA 2   | NO                  | 10.10.2.124          |
| ELK      | NO                  | 10.10.2.31           |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantages of automating configuration with Ansible, is this practice saves time, it is easier on the installation process and there will be no trouble shooting during deployment because the scripts and commands work.

The playbook implements the following tasks:
- _Here is a simple rundown or the ELK installation process TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ... Download corrected install-elk.yml and then copy to Jump Box using the command 
```scp -i " Ohio Key.pem" install-elk.yml ec2-user@ec2-3-142-43-135.us-east-2.compute.amazonaws.com:/ec2-user```

- ... SSH in to Jump Box 
```ssh -i "Ohio_Key.pem" ec2-user@ec2-3-142-43-135.us-east-2.compute.amazonaws.com```

- ... Check Docker status use command 
```sudo service docker status``` if status in not running use 
```sudo service docker start``` to start docker
- ... ```Sudo docker run -t -I cyberxsecurity/ansible bash (this will place you in the root```
- ... open new git or command window and do the following 
SSH in to Jump box, LS to confirm keys and install-elk.yml and run rolowing command 
```sudo docker ps``` copy the container id 
```sudo docker cp install-elk.yml <container id>:/root```
```sudo docker cp Ohio Key.pem <container id>:/root```

- ... go back to the Jump Box in Root and do the following 
ls
install-elk.yml and Ohio Key.pem should appear
cd /etc/ansible
nano hosts
make following changes 
under [webserver]
       10.10.2.94
	  10.10.2.124
	ADD -[ELK]
	   10.10.2.31 Save and quit 
- ... cd ~
ssh in to ELK run 
```sudo apt-get update``` 
```sudo apt-get upgrade``` 
exit 
When back in root run following command 
```ansible-playbook install-elk.yml --key-file Ohio Key.pem```
-... Use you RDP machine via AWS and the private ip address from ELK 
in you running instances, this will allow you to view a working Kibana 





This is the path so you can see an picture of what Kibana look like 
![Alt text](https://raw.githubusercontent.com/Iancritchlow/AWS-Ansible-Elk-and-Filebaet/main/Images%20final%20output/Imagesdocker_RDP_ELK_Kibana_output.png.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- DWVA 10.10.2.94
  DVWA2 10.10.2.124

We have installed the following Beats on these machines:
- For this network I have installed Kibana and Filebeat 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the file to ansible:/root.
- Update the filebeat.yml file to include the ELK ip 
- Run the playbook, and navigate to Kibana in your windows RDP to check that the installation worked as expected.

The files that were used and include are - filebeat-playbook.yml this file was acquired from a secure credible website written by a trusted source 
- You have to edit and reconfigure the configuration file. 
The way I specified which machine to install the ELK server 
on versus the way in which to install Filebeat on . by navigating to the ansible director while the Is the elk server was more automated by  was done in hosts file and the file beat was configured prior to by changing lines 1106 and making it my 10.10.0.27 and the doing the same to 1806 in the filebeat.yml file prior to importingin to docker.
- _The url you want to navigate is http://10.10.2.31:5601/ to make sure that filebeat is working 
- This is an image of what filebeat loks like when properly deployed 
- ![Alt text](https://raw.githubusercontent.com/Iancritchlow/AWS-Ansible-Elk-and-Filebeat/main/Images%20final%20output/filebeat.PNG)

The bonus is completed as linux readme there you will find all the need commands and the needed instances/ specifications..
