
# FlexPod Converged Infrastructure setup using Ansible for FlexPod Datacenter with Red Hat OpenShift on Bare Metal

Note that the scripts in this repository have now been successfully tested with NetApp ONTAP 9.16.1, Cisco Nexus NXOS 10.4(4), and Cisco UCS 4.3(5).

This repository for FlexPod contains Ansible playbooks to configure Cisco Nexus, Cisco UCS Intersight, and NetApp ONTAP. This repository can be used for setting up Cisco devices and NetApp ONTAP Storage in preparation for Installing Red Hat OpenShift as covered in the following Cisco Validated Design (CVD): https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_rh_ocp_bm_xseries.html.

The CVD lays out the complete process for configuring the FlexPod OpenShift tenant using Ansible. As a prerequisite to this installation FlexPod Base should be installed on the FlexPod using https://www.cisco.com/c/en/us/td/docs/unified_computing/ucs/UCS_CVDs/flexpod_base_imm_m7_iac.html. Since these playbooks are intended to save time in setting up a working FlexPod, a complete FlexPod as shown below is needed to execute the playbooks. Various simulators could be used to partially test individual playbooks. The Cisco UCS X-Series Direct-based FlexPod as shown below was used to validate these Ansible scripts, but FlexPods with traditional fabric interconnects are also supported.

![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-IMM-OpenShift/blob/main/ReadmePics/Main-Topology.jpg)  

# Set up the execution environment

To execute various ansible playbooks, a linux based system will need to be setup as described in the FlexPod Base CVD with the packages listed at the following pages:

- Cisco UCS Intersight: https://galaxy.ansible.com/ui/repo/published/cisco/intersight/
- Cisco NxOS: https://galaxy.ansible.com/ui/repo/published/cisco/nxos/
- NetApp ONTAP: https://galaxy.ansible.com/ui/repo/published/netapp/ontap/

# How to execute these playbooks?

![block-diagram](https://github.com/ucs-compute-solutions/FlexPod-IMM-OpenShift/blob/main/ReadmePics/Ansible-Order.jpg)

Because a number of manual tasks need to be executed between running the Ansible playbooks, the CVD document should be used as a guide for running the playbooks. Commentary is included in the variable files to guide filling in those values.

In this version of the FlexPod setup, M.2 boot of Red Hat OpenShift will be used.
The steps for setting up a FlexPod with M.2 boot and NFS, iSCSI, and NVMe-TCP storage protocols are:

1.  Create a directory and clone the repository from Github with "git clone https://github.com/ucs-compute-solutions/FlexPod-IMM-OpenShift.git".
2.  Fill in the variable files according to the CVD.
3.  Execute the Nexus playbook with "ansible-playbook ./Setup_Nexus.yml -i inventory" to setup the Nexus switches.
4.  Execute the NetApp storage playbook with "ansible-playbook -i inventory Setup_ONTAP.yml -t ontap_config".
5.  Follow the manual steps in the CVD to create an Intersight Organization and Resource Group.
6.  Execute the IMM playbooks with "ansible-playbook ./Setup_IMM_Pools.yml", "ansible-playbook ./Setup_IMM_Server_Policies.yml", and "ansible-playbook ./Setup_IMM_Server_Profile_Templates.yml" to setup the Cisco UCS Server Profile Templates, Policies, and Pools.
7.  Follow the manual steps in the CVD to create UCS IMM server profiles for the OpenShift Cluster.
8.  Follow the steps in the CVD to install and configure the OpenShift Bare Metal Cluster.

The Ansible playbooks and CVD are structured in a way that Intel or AMD-based UCS M6, M7, or M8 servers can be setup by adjusting variables. 
