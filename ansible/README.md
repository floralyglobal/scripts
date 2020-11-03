## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram]

Answer: (Images/Network_Diagram.png)

- _TODO: Enter the playbook file._ 
  
  Answer: filebeat-playbook.yml FILE
  ---
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system

    # Use command module
  - name: Setup filebeat
    command: filebeat setup

    # Use command module
  - name: Start filebeat service
    command: service filebeat start



This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
Answer: Load balancing ensures that the work to process incoming traffic will be shared with both vulnerable servers. The advantage of using a jumobox is that the access controls are under authorized users.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file systems of the VMs on the network and system metrics.
- _TODO: What does Filebeat watch for?
Answer: Filebeat collects data about the file system.

- _TODO: What does Metricbeat record?_
Answer: Metricbeat records the machine metrics, like uptime.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
|RedTeam-Web1|WEBSERVER| 10.0.0.5  | Linux            |
|RedTeam-Web2|WEBSERVER| 10.0.0.6  | Linux            |
|ELK-SERVER|Monitoring| 10.1.0.4   | Linux            |

Availability Zone 1: RedTeam-VM1 + RedTeam-VM2
Availability Zone 2: ELK-SERVER

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses_
10.0.0.5
10.0.0.6
10.1.0.4

Machines within the network can only be accessed by each other.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?
Answer: I allowed the Jumpbox VM to access my ELK VM. The access to this machine 23.100.36.109.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |     23.100.36.109    |
|   ELK    | No                  |       10.0.0.1-254   |
|RedTeam-Web1| No                |       10.0.0.1-254   |
|RedTeam-Web2| No                |       10.0.0.1-254   |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
Allows administrators to deliver more value to the business by spending time on more important tasks.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
Answer: Create an Ansible playbook that installs Docker and configures an ELK container. Run the playbook to launch the container. Restrict access to the ELK VM.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
 The ELK server is configured to monitor Web1 and Web2 VM's at 10.0.0.5 and 10.0.0.6.

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
Answer: Filebeat and metricbeat on Web1 and Web2 VM's. 

These Beats allow us to collect the following information from each machine:
Filebeat: Filebeat detects changes to the filesystem. Specifically, we use it to collect Apache logs. 

Metricbeat: Metricbeat detects changes in system metrics, such as CPU usage. We use it to detect SSH login attempts, failed sudo escalations, and CPU/RAM statistics.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbooks file to Ansible control node.
- Update the /etc/ansible file to include playbook and host
- Run the playbook, and navigate to kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?
The playbook is under the /etc/ansible folder. You copy it into the files folder using the cp command. 


- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
You update the host file under etc to specify the webservers and the ELK server. In the playbook, we specify the host name. 
The command would look like such: 
  ansible-playbook install_filebeat.yml webservers

- _Which URL do you navigate to in order to check that the ELK server is running?
curl http://[ELKSERVER_IP_ADDRESS]
