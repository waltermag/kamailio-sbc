## Project: Microsoft Teams to IP PBX Interconnection Lab

This project provides a lab environment to test the concept of using Kamailio, as Session Border Controller (SBC), to connect Microsoft Teams with other IP PBX systems. The deployment is automated using Ansible.

**Project Overview**

This lab simulates a scenario where Microsoft Teams needs to connect with other IP PBX systems. Kamailio acts as the SBC, facilitating communication between them. Ansible playbooks automate the deployment process using Docker containers.

**Features:**

* Connects Microsoft Teams to other IP PBX systems
* Automated deployment using Ansible

**Deployment**

All deployments are containerized using Docker images for Kamailio and Asterisk. Ansible playbooks manage the deployment process. The playbooks are designed for Ubuntu 20.04.6 LTS.

**Using the Lab**

The provided Ansible commands allow you to easily deploy, manage, and tear down the lab environment.

**Running the Lab Environment**

```bash
# Deploy the entire Lab
ansible-playbook lab.yaml

# Deploy only Asterisk
ansible-playbook lab.yaml --tags deploy_asterisk

# Deploy only Kamailio
ansible-playbook lab.yaml --tags deploy_kamailio

# Stop and remove the whole lab
ansible-playbook lab.yaml --tags tear_down

# Remove only Asterisk
ansible-playbook lab.yaml --tags remove_asterisk

# Remove only Kamailio
ansible-playbook lab.yaml --tags remove_kamailio
```

**Configuration Update**

You can update configurations for both Asterisk and Kamailio within the "files" directory inside their respective roles in the Ansible project structure. After making changes, rerun the playbook using `ansible-playbook lab.yaml` to apply the updates.

**Development Status**

This project is under development and might have errors which will be fixed in future iterations.

**Additional Notes**

* For more information on Kamailio, refer to the kamailio-docker: [https://github.com/kamailio/kamailio-docker](https://github.com/kamailio/kamailio-docker) project.
* The Asterisk image used is from the docker-asterisk: [https://github.com/mlan/docker-asterisk](https://github.com/mlan/docker-asterisk) project.
