### Automated ONTAP storage deployment using Ansible for FlexPod OpenShift 4.7 Bare-Metal
 
This repository contains Ansible roles and playbooks for an end-to-end ONTAP storage deployment for OpenShift 4.7 in a FlexPod Datacenter.
https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_redhat-OCP.html

The ONTAP deployment automation is based on the following roles:

	ontap_primary_setup
	ontap_network
	ontap_svm
	ontap_volumes
	ontap_lifs

These roles are developed as per the best practices prescribed in the Cisco Validated Design (CVD) "FlexPod Infrastrucutre as Code (IaC) for RedHat OpenShift Container Platform 4.7 Bare-Metal".

### Environment Validated

As the automation solution is specifically build for the above mentioned CVD, the current roles and playbooks support the following components:

	Storage Operating System: ONTAP 9.8
	OpenShift Container Platform: 4.7
	Storage Protocols: iSCSI, NFS

### Prerequisite

1. It is assumed that the physical rack and stack, power-on, initialisation of ONTAP OS, setup of Node Management IPs and initial ONTAP Cluster with IP is completed.
NOTE: Aggregate creation is part of the automation.

2. The user should have an Ansible Control machine that has network reachability to the ONTAP storage system and internet access to pull this repository from GitHub.
Refer https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html for guidance on setting up an Ansible Control machine.

3. The Ansible control machine should have the NetApp library and collections installed

	```
	pip3 install netapp-lib
	ansible-galaxy collection install netapp.ontap

	```

### Getting Started

1. From the Ansible Control machine Download a ZIP version of this repository or clone it using the below command:
	
	```
	git clone https://github.com/NetApp-Automation/FlexPod-OpenShift-4.7-Bare-Metal

	```
2. There is one variable file under the vars folder 'ontap_main.yml' that needs to be filled out with environment specific parameters prior to executing the playbook.

NOTE: The format of the variable file needs to be maintained as it is, any changes to the structure of the file may lead to failure in execution of the playbook.

NOTE: Sample values are pre-populated against some variables in order to provide the user additional clarity on how the variable needs to be filled out. Please replace the sample values with your environment specific information.

3. Update the credentials for the ONTAP Cluster

Navigate to the 'ontap' file within the 'group_vars' directory and update it with the admin credentials for the ONTAP cluster and the Node Management IPs 

Example -

	# Credentials for connecting to the ONTAP Cluster
	username: admin
	password: password

	# Management IP Addresses of the two nodes in the ONTAP Cluster
	ontap_a: 192.168.10.10
	ontap_b: 192.168.10.20
	

4. Update the Inventory file

Open the 'inventory' file and update it with a record for the ONTAP Cluster Management IP

Example -


	[ontap]
	# ONTAP Cluster Management IP, list only one ONTAP Cluster IP
	192.168.10.5


5. Executing the Playbook

A playbook by name 'Setup_ONTAP.yml' is available at the root of this repository. It calls all the required roles to complete the setup of the ONTAP storage system.

Execute the playbook from the Ansible Control machine as an admin/ root user using the following command:


	ansible-playbook -i inventory Setup_ONTAP.yml
	

If you would like to run a part of the deployment, you may use the appropriate tag that accompanies each task in the role and run the playbook in the below fashion -

	ansible-playbook -i inventory Setup_ONTAP.yml -t <tag_name>
	

### Authors

 * Arvind Ramakrishnan (arvind.ramakrishnan@netapp.com)
