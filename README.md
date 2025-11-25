# CloudShop Deployment

This project contains the source code and infrastructure automation for CloudShop, a simple e-commerce application.

## Architecture

*   **Application**: Node.js/Express app with EJS templates.
*   **Infrastructure**: AWS (VPC, Public Subnets, EC2) provisioned via Terraform.
*   **Configuration**: Ansible playbooks to configure Web Server (Nginx + Node.js) and Monitoring Server (Nagios).
*   **Monitoring**: Nagios Core monitoring the Web Server.

## Prerequisites

*   AWS CLI configured with credentials.
*   Terraform installed.
*   Ansible installed.
*   SSH Key Pair generated (default path: `~/.ssh/cloudshop-key`).

## Directory Structure

*   `app/`: Application source code.
*   `terraform/`: Infrastructure code.
*   `ansible/`: Configuration management code.

## Deployment Steps

### 1. Provision Infrastructure

Navigate to the `terraform` directory:

```bash
cd terraform
terraform init
terraform apply -var="public_key=$(cat ~/.ssh/cloudshop-key.pub)"
```

Type `yes` to confirm. Note the `web_public_ip` and `nagios_public_ip` outputs.

### 2. Configure Instances

Navigate to the `ansible` directory:

```bash
cd ../ansible
```

Generate the inventory file (ensure you are on Linux/Mac or Git Bash):

```bash
chmod +x generate_inventory.sh
./generate_inventory.sh
```

Run the playbook:

```bash
ansible-playbook site.yml
```

### 3. Access Application

*   **CloudShop**: `http://<WEB_PUBLIC_IP>`
*   **Nagios**: `http://<NAGIOS_PUBLIC_IP>/nagios`
    *   **Username**: `nagiosadmin`
    *   **Password**: `nagiosadmin` (default set in playbook)

## Monitoring

Nagios is configured to monitor:
*   **HTTP**: Checks if CloudShop is reachable.
*   **SSH**: Checks if SSH is reachable.
*   **Load**: Checks Nagios server load (as a demo).

## Development

To run the app locally:

```bash
cd app
npm install
npm start
```

Visit `http://localhost:3000`.
