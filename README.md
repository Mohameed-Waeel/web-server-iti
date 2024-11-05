# web-server-iti

This project utilizes Ansible to automate the deployment and configuration of Apache web servers. It includes setup for virtual hosts with SSL, basic authentication, and SELinux configuration to enhance security.

# How to configure selinux to allow httpd to listen on port 88 (command)
![10](https://github.com/user-attachments/assets/03b8bfee-ea80-47ba-908b-75fa22470818)
![20](https://github.com/user-attachments/assets/7abe4a47-c30c-4690-a37b-766828e9c1bf)

# In directory directive how to permit only 1 IP to access the private area (Configure )
![30](https://github.com/user-attachments/assets/9ab20f95-517b-450b-887f-b4d3ddf0819e)
![40](https://github.com/user-attachments/assets/7bfdd47c-931f-4403-9c63-bc4f7f033775)

## Project Structure

```plaintext
.
├── README.md                   # This file
├── inventory                   # Inventory files for hosts
│   └── production_hosts        # Contains the list of target hosts
├── variable_files              # Directory for variable files
│   └── global_variables.yml    # Global variables used across playbooks
├── app_roles                   # Contains Ansible roles
│   └── web_server              # Role for configuring the web server
│       ├── tasks               # Directory for task definitions
│       │   └── setup.yml       # Main tasks for setting up the web server
│       ├── templates           # Contains Jinja2 templates for configuration files
│       │   ├── site_configuration.conf.j2 # Virtual host configuration template
│       │   └── access_control.j2          # .htaccess configuration template
│       └── settings            # Directory for variable settings specific to the role
│           └── defaults.yml    # Default variables for the web server role
└── main_playbook.yml           # Main playbook that ties everything together
```

## Prerequisites

Before running this playbook, ensure you have the following:

- **Ansible**: Install Ansible on your control machine.
- **Python**: Ensure Python is installed on your managed nodes.
- **Access**: SSH access to the target servers with appropriate permissions to install packages and modify configuration files.
- **SELinux**: The managed nodes should have SELinux enabled if using custom ports.

## Configuration

### Inventory File

The `inventory/production_hosts` file contains the target hosts for the Ansible playbook. Each host must be listed under the appropriate group. 

Example:
```ini
[web_servers]
web_server_1 ansible_host=your_server_ip
```

### Global Variables

The `variable_files/global_variables.yml` file defines various variables that can be used throughout the playbooks, including:

- `httpd_port`: The HTTP port for Apache.
- `https_port`: The HTTPS port for Apache.
- `custom_port`: The custom SELinux port.
- `trusted_ip`: The IP address allowed access.
- `ssl_cert_path`: Path to the SSL certificate file.
- `ssl_key_path`: Path to the SSL certificate key file.

### User Credentials

User credentials for basic authentication should be defined in the `global_variables.yml` file. Example:

```yaml
user_credentials:
  - user: user1
    pass: password1
  - user: user2
    pass: password2
```

## Role Overview

### Web Server Role

The `app_roles/web_server` directory contains all the necessary files to configure the Apache web server. 

- **Tasks**: The `tasks/setup.yml` file contains a series of tasks to install Apache, configure SELinux, set up directories, create user authentication files, and configure virtual hosts.
- **Templates**: The `templates` directory contains Jinja2 templates used to generate configuration files for Apache based on the provided variables.

### Templates

- `site_configuration.conf.j2`: Template for configuring virtual hosts with SSL and logging.
- `access_control.j2`: Template for `.htaccess` file to manage access controls.

### Defaults

The `settings/defaults.yml` file defines default variables for the web server role, including user and group names.

## Running the Playbook

To run the playbook, navigate to the project directory and execute the following command:

```bash
ansible-playbook main_playbook.yml -i inventory/production_hosts
```

You will be prompted to enter the hostname for the virtual host during the execution.

## Notifications

The playbook includes a handler to restart the Apache service whenever a virtual host configuration is changed. This ensures that all configuration changes are applied immediately.

## Conclusion

This Ansible project simplifies the process of deploying and configuring an Apache web server with SSL and basic authentication. By utilizing roles and templates, the project promotes reusability and maintainability of the Ansible code.

For further customization, you can modify the templates and variables according to your specific requirements.

---
