# Ansible for WordPress Docker Compose Deployment

This repository contains Ansible code to deploy a WordPress site with Nginx using Docker Compose. This simple setup doesn't cover SSL certification. By default, this playbook will clone your specified WordPress application's GitHub repository onto a target server.

### What You Need
You'll need Ansible installed on your computer, along with the community.docker and geerlingguy.docker Ansible collections.

## How to Set Up and Deploy
This repository contains all the necessary Ansible files like hosts.ini, playbook.yml, vault.yml, and the roles/ folder.

###  Populate Ansible Vault (ansible/vault.yml)
This file securely stores sensitive information like database passwords.

Go to the ansible/ folder within this repository (your local copy). For example: cd ansible-for-nginx-wordpress-deployment/ansible/
Create or edit the vault file: ansible-vault edit vault.yml
Set a password for the vault and remember it!

Add these secret variables inside the file, making sure names match exactly:
wp_db_name
wp_db_user
wp_db_password
mysql_root_password

Save and close the editor.
 
## What Else to Configure
ansible/hosts.ini: Tell Ansible where your server is. For a remote server, specify its IP address, SSH username, and the path to your SSH private key. 
ansible/playbook.yml: This is your main deployment script.

Set wp_repo_url to your WordPress application project's GitHub URL (this should be a separate repository containing your docker-compose.yml and WordPress files, not this Ansible repo).

Define remote_app_dir (e.g., /opt/my_wordpress_app) as the path where your WordPress application will be cloned on the server.
Ensure vars_files: - vault.yml is present to load your secrets.

ansible/roles/wordpress_deploy/templates/.env.j2: This template creates the .env file for Docker Compose on the server. Here, ensure all variables like WORDPRESS_LOCAL_HOME, NGINX_LOGS, and all database variables (e.g., WORDPRESS_DB_NAME, MYSQL_ROOT_PASSWORD) correctly use Ansible variables (like {{ wp_db_name }}).

Your docker-compose.yml file: This file, located in your WordPress application repository, defines your Docker services. Verify it uses environment variables (e.g., ${WORDPRESS_LOCAL_HOME}) from your .env file.

## How to Run
Go to the root of this Ansible repository on your local machine.
Run the Ansible playbook: ansible-playbook -i ansible/hosts.ini ansible/playbook.yml --ask-vault-pass
Enter your Vault password when prompted.

## After Deployment
Open your web browser and go to http://your_server_ip:8080 (adjust the port if needed) to follow the WordPress setup wizard.
