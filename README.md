## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![alt text](https://github.com/rpaley/ELK-Stack-Project/blob/main/Images/Network%20Diagram%20-%20RedTeam.png "Network Diagram")

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

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

Load balancing ensures that the application will be highly stable, in addition to restricting disruptions to the network.
-  Load balancers help protect availability by ensuring that machines don't get overwhelmed by traffic and therefore unavailable. A load balancer allows the traffic to be distributed evenly to ensure that access is preserved.
-  A jump box provides a single point through which traffic can be directed and from which other devices can be controlled. This makes it much easier to monitor and identify any security breaches or flaws.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the machine state and system files.
- Filebeat monitors what files have been changed on a system and when the changes where made.
- Metricbeat monitors machine statistics rather than specific file information.

The configuration details of each machine may be found below.

| Name                 | Function       | IP Address                              | Operating System   |
|----------------------|----------------|-----------------------------------------|--------------------|
| Jump-Box-Provisioner | Gateway        | Public: 52.147.22.43 Private: 10.0.04   | Linux Ubuntu 18.04 |
| Web-1                | DVWA Container | Private: 10.0.0.5                       | Linux Ubuntu 18.04 |
| Web-2                | DVWA Container | Private: 10.0.0.7                       | Linux Ubuntu 18.04 |
| Web-3                | DVWA Container | Private: 10.0.0.8                       | Linux Ubuntu 18.04 |
| ELK-SERVER           | Monitoring     | Public: 52.163.84.216 Private: 10.1.0.4 | Linux Ubuntu 18.04 |

### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Jump-Box-Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 180.150.48.193

Machines within the network can only be accessed by the Jump-Box-Provisioner.
- The Jump-Box-Provisioner, with IP 10.0.0.4, can access the ELK-SERVER

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses                         |
|----------------------|---------------------|----------------------------------------------|
| Jump-Box-Provisioner | Yes                 | 180.150.48.193                               |
| Web-1                | No                  | 10.0.0.4                                     |
| Web-2                | No                  | 10.0.0.4                                     |
| Web-3                | No                  | 10.0.0.4                                     |
| ELK-SERVER           | No                  | 10.0.0.4 180.150.48.193 (Kibana Access only) |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it is not practical to have to manually configure a potentially large number of machines.

The playbook implements the following tasks:

It installs the apt packages docker.io (for running containers) and python3-pip (to install Python software), and the pip docker package (allows Ansible to control the state of the Docker containers). 
It then enables the VM system to use more memory, which is required for ELK to run properly. 
Finally, it downloads and launches the docker elk container with the correct port mappings and enables the docker service.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
![alt text](https://github.com/rpaley/ELK-Stack-Project/blob/main/Images/docker_ps_output.png "Docker PS output")

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5, 10.0.0.7, 10.0.0.8

We have installed the following Beats on these machines:
- Filebeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects information about changes to system files and when those changes were made.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible/files.
- Update the Filebeat configuration file to include the correct IP address for the ELK machine (in lines 1106 and 1806). Then, create the the playbook to install filebeat in /etc/ansible/files/filebeat-install.yml 
- Run the playbook, and navigate to the ELK server GUI (via the Kibana site) to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:
- _Which file is the playbook? Where do you copy it? install-elk.yml is the playbook, and it is copied to the Web-x VMs.
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? You update the filebeat-config.yml file to specify which machine to install the filebeat service on. This is different from specifying where to install the ELK server because that used Ansible's hosts option to specify that the playbook should only be run on machines in the ELK-SERVER group.
- _Which URL do you navigate to in order to check that the ELK server is running? http://[your.VM.IP]:5601/app/kibana. For mine, this is http://52.163.84.216:5601/app/kibana
