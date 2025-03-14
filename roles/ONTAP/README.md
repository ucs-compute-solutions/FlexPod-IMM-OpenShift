### Automated ONTAP storage deployment using Ansible for FlexPod Datacenter with Red Hat OpenShift on Bare Metal
 
This repository contains Ansible roles and playbooks for an end-to-end ONTAP storage deployment for FlexPod OpenShift tenant.

The ONTAP deployment automation is based on the following roles:

	ontap_network
	ontap_svm
	ontap_volumes
	ontap_lifs
	ontap_finalize_setup

These roles are developed as per the best practices prescribed in the Cisco Validated Design (CVD) "[FlexPod Datacenter with Red Hat OpenShift Bare Metal Manual Configuration with Cisco UCS X-Series Direct](https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_rh_ocp_bm_xseries.html)".

### Environment Validated

As the automation solution is specifically build for the above mentioned CVD, the current roles and playbooks support the following components:

	Storage Operating System: ONTAP 9.16.1
	Storage Protocols: NFS, iSCSI, NVMe/TCP, and S3

### Prerequisites

1. FlexPod Base setup should be in place using the FlexPod Base CVD "[FlexPod Datacenter Base Configuration using IaC with Cisco IMM and NetApp ONTAP Deployment Guide](https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_base_imm_m7_iac.html)"

2. Refer to the FlexPod Base GitHub repository for automated deployment of FlexPod Base via Ansible:
   https://github.com/ucs-compute-solutions/FlexPod-Base-IMM

### Getting Started

1. From the Ansible Control machine Download a ZIP version of this repository or clone it using the below command:
	
```
git clone https://github.com/ucs-compute-solutions/FlexPod-IMM-OpenShift.git
```

2. There is one variable file under the vars folder 'ontap_main.yml' for setup of ONTAP that need to be filled out with environment specific parameters prior to executing the playbook.

3. This playbook to setup ONTAP will also use some common parameters defined in the 'all.yml' variable file under group_vars, please ensure that the all.yml file is up to date. 

NOTE: The format of the variable file needs to be maintained as it is, any changes to the structure of the file may lead to failure in execution of the playbook.

NOTE: Sample values are pre-populated against some variables in order to provide the user additional clarity on how the variable needs to be filled out. Please replace the sample values with your environment specific information.

4. Update the credentials for the ONTAP Cluster

Navigate to the 'ontap' file within the 'group_vars' directory and update it with the username info. Open the 'secrets.yml' file under 'group_vars' directory and update the password for the ONTAP cluster.

Example -

	# Credentials for connecting to the ONTAP Cluster
	username: admin
	password: "{{ password_1 }}"

5. Update the Inventory file

Open the 'inventory' file and update it with a record for the ONTAP Cluster Management IP

Example -

	[ontap]
	# ONTAP Cluster Management IP. List only one ONTAP Cluster IP
	192.168.10.5

6. Executing the Playbook

A playbook by name 'Setup_ONTAP.yml' is available at the root of this repository. It calls all the required roles to complete the setup of the ONTAP storage system for the FlexPod OpenShift tenant.

Execute the playbook from the Ansible Control machine as an admin/ root user using the following command:


	After setup of Cisco Nexus switches	-	ansible-playbook -i inventory Setup_ONTAP.yml -t ontap_config
 

If you would like to run a part of the deployment, you may use the appropriate tag that accompanies each task in the role and run the playbook in the below fashion -

	ansible-playbook -i inventory Setup_ONTAP.yml -t <tag_name>	

### Authors

 * Kamini Singh (kamini.singh@netapp.com)
