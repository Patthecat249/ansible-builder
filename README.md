# Documentation - How to Build an Execution Environment

## Create only the environment

```bash
ansible-builder create -v 3
```

## Create the Image - run ansible-builder

```bash
# Export Red Hat Automation Hub TOKEN
export ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_TOKEN="$(cat token-automationhub)"

# Test galaxy hub configuration
ANSIBLE_CONFIG=./files/ansible.cfg ansible-galaxy collection download   ansible.controller:4.5.36 --server automation_hub -vvv

# Run Ansible-Builder with Automation Hub Token
ansible-builder build --container-runtime podman --tag ansible-ee-ubi10:1.0.0 --build-arg ANSIBLE_GALAXY_SERVER_AUTOMATION_HUB_TOKEN --verbosity 3

# Run Ansible-Builder
ansible-builder build --container-runtime podman --tag ansible-ee-ubi10:1.0.0 --verbosity 3
```

## Execute Image

```bash
podman run -it --name ee localhost/ansible-ee-ubi10:1.0.0 /bin/bash
```

## Trivy scan

```bash
# Podman Image Save und an Trivy übergeben
podman save -o ansible-ee-ubi10-1.0.1.tar localhost/ansible-ee-ubi10:1.0.1 && \
trivy image --input ansible-ee-ubi10-1.0.1.tar --scanners vuln
```
