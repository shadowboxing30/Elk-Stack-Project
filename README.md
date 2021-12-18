## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Elk-Stack-Project-Diagram](Elk-Stack%20Project.drawio.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Elk-Stack-Project file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

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
- Load balancers help to direct network traffic. By filtering network traffic appropriately, load balancing ensures that the resource, DVWA in this case, will be available to a user. There are many advantages of a jumpbox. For one it allows a trusted host, say your home computer, to access a private network. In the case of this project, my jumpbox is configured to only receive the SSH protocol from my host computer. Authenticating the SSH transmission between my jumpbox and my host computer is done through the generated SSH keys. Utilizing these SSH keys insures that only my computer can access the jumpbox. Once "in" the jumpbox I can SSH to other virtual machines found within the private network. Jumpbox can be seen in this instance as the gateway into my private network. Jumpboxs can add a layer of security to the private network as well as keeping the network segmented. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data or device statistics  and system logs.
- Filebeat monitor the logfiles of, in my case two virtual web servers. Filebeats than collects these logs and exports them to elasticsearch or logstash.

- Metricbeat record metrics and statstics of in my case two virtual web servers. Metricbeats help visualize how much resources are being used by these virtual web servers in real time. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address             | Operating System |
|----------|----------|------------            |------------------|
| Jump Box | Gateway  | Public:20.119.37.175.  | Linux            |
|                       Private: 10.0.0.4
| 
| Web1     |Host Website|Private: 10.0.0.5     | Linux            |
| Web2     |Host Website|Private: 10.0.0.6     | Linux            |
| 
|Load Balancer|load balancer|public: 20.124.198.150| Linux        |  

|Elk-Stack |Elkstack    | Public: 65.52.197.89 | Linux
                          Private: 10.1.0.4    |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the elk server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 
Host computer IP: 68.255.146.27  through TCP on port 5601

Machines within the network can only be accessed by Jumpbox.
- Which machine did you allow to access your ELK VM? 

Host computer IP: 68.255.146.27  through TCP on port 5601

Jumpbox IP: 10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |     NO              | 68.225.146.27 SSH 22 |
| Web1     |     NO              | 10.0.0.4 SSH 22      |
| Web2     |     NO              | 10.0.0.4 SSH 22      |
|LoadBalancer|   NO              | 68.225.146.27 HTTP 80|
|Elk-Stack |     NO              | 68.255.146.27  through TCP on port 5601|


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
 
The main advantage to using ansible to automate tasks is that you can in simple txt file deploy many services all at once. The easy to read and modify txt file can make it easier for anyone to deploy a multiple step installation by simply running one command. Also once an ansible file is saved it can be reused to easily deploy services onto another container or machine. 

The playbook implements the following tasks:

The first three steps install the following services: docker, python, and the python module for docker. 

The fourth step expands the virtual memory of the container.

The fifth step downloads and deploys the Elk container onto the Elk virtual machine. It also instructs what ports the container will configured on. In this case the Elk container will be deployed onto ports 5601, 9200, and 5044. 

The final step is making sure the docker container is added on booted once the Elk virtual machine is turned on. 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker-Elk](Elk-Stack%20container.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

Web1 Private: 10.0.0.5
Web2 Private: 10.0.0.6

We have installed the following Beats on these machines:
 
I have installed filebeat and metricbeat to Web1 and Web2 machines. 

These Beats allow us to collect the following information from each machine:
-
"Filebeat is a light weight log shipper which is installed as an agent on your servers and monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or log stash for indexing" (https://medium.com/devops-dudes/getting-started-with-filebeat-5efe181324ae). Filebeat can be configured to monitor any location of your choosing. For example you can monitor all logs from a particular port such as port 5601.

Metricbeat is a lightweight shipper that monitors metrics on an operating system. These metrics include a real time readout of CPU and RAM usage. 

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the ansible-config.yml file to ansible container /etc/ansible.
- Update the ansible-config.yml file to include remote user- azadmin.
- Run the playbook, and ssh to elk machine to check that the installation worked as expected.

Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?
The Elk-Stack installation playbook is copied to the Ansible container in the following path /etc/ansible.

- _Which file do you update to make Ansible run the playbook on a specific machine? 

The Ansible hosts file must be updated first. Again this file is found in the Ansible container with the path /etc/ansible. In this file you add hosts with their specific IP addresses. Once the hosts file is updated than you can specify which playbooks will run on which machines. 

How do I specify which machine to install the ELK server on versus which to install Filebeat on?_

In order to run the playbook just on say the webservers, make sure that in the playbook's header under hosts it reads "hosts:webservers." This will insure that the playbook will only run on the webservers.    

- _Which URL do you navigate to in order to check that the ELK server is running?

65.52.197.89:5601/app/Kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

#Command for Running a playbook

ansible-playbook <name of playbook.yml>

#Command installing Docker

sudo apt install docker.io

#Command to update Docker

Sudo docker update

#Command for pulling a docker container from the docker hub

Sudo docker pull <name of container>

#Command for viewing containers on your machine

Sudo docker container ls -a 

#Command for starting a container

Sudo docker start <name of container>

#Command for attaching into a container

Sudo docker container exec -it <name of container> bash
