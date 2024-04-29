**Project Overview:**
This project serves as a lab environment for testing the proof of concept of using Kamailio as a Session Border Controller (SBC) to connect Microsoft Teams to other IP PBX systems. The deployment is managed via Ansible.

**Deployment Method:**
All deployments are containerized using Docker.

**Ansible Compatibility:**
The Ansible playbook was prepared for Ubuntu 20.04.6 LTS.

**Docker Images Used:**
- Kamailio Image: `ghcr.io/kamailio/kamailio:5.8.1-bookworm` from the project [kamailio-docker](https://github.com/kamailio/kamailio-docker).
- Asterisk Image: `mlan/asterisk:full-1.1.8` from the project [docker-asterisk](https://github.com/mlan/docker-asterisk).

**Deployment Commands:**
```bash
# Deploy the complete lab
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

**Configuration Update:**
The configurations for both Asterisk and Kamailio can be updated inside the roles under the files directory. After updating configurations, rerun the playbook.

**Development Status:**
This project is still under development and has not been fully tested yet. Errors are expected and will be fixed in subsequent iterations
