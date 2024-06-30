<h1>Advanced Ansible Project - Terraform + Ansible Integration</h1>

#### Introduction to Terraform and Ansible

**Terraform** and **Ansible** are powerful tools for infrastructure as code (IaC) and configuration management, respectively. Combining these tools allows you to provision infrastructure using Terraform and configure it using Ansible.

**Terraform:**
- **Purpose:** Automates the provisioning of infrastructure across various cloud providers and services.
- **Key Features:** Infrastructure as code, resource management, state management, and provider plugins.

**Ansible:**
- **Purpose:** Automates configuration management, application deployment, and task automation.
- **Key Features:** Agentless, uses SSH for communication, YAML-based playbooks, idempotent operations, and extensive module library.

#### Project Overview: Combining Terraform and Ansible

**Objective:**
To provision infrastructure using Terraform and configure it using Ansible, demonstrating a comprehensive infrastructure automation workflow.

**Project Components:**
1. **Terraform Configuration:**
   - Define the infrastructure resources (e.g., EC2 instances, security groups) in Terraform.
2. **Ansible Playbooks:**
   - Define the configuration tasks (e.g., installing software, setting up services) in Ansible.
3. **Integration:**
   - Use Terraform to provision infrastructure and pass the necessary information (e.g., IP addresses) to Ansible for configuration.

#### Step-by-Step Implementation

**Step 1: Terraform Configuration**

1. **Install Terraform:**
   Download and install Terraform from the official website.

2. **Define Variables:**
   Create a `variables.tf` file to define input variables.

   ```hcl
   variable "aws_access_key" {
     description = "AWS access key"
     type        = string
   }

   variable "aws_secret_key" {
     description = "AWS secret key"
     type        = string
   }

   variable "region" {
     description = "AWS region"
     type        = string
     default     = "us-west-2"
   }

   variable "instance_type" {
     description = "EC2 instance type"
     type        = string
     default     = "t2.micro"
   }
   ```

3. **Provider Configuration:**
   Create a `provider.tf` file to configure the AWS provider.

   ```hcl
   provider "aws" {
     access_key = var.aws_access_key
     secret_key = var.aws_secret_key
     region     = var.region
   }
   ```

4. **Resource Definition:**
   Create a `main.tf` file to define the infrastructure resources.

   ```hcl
   resource "aws_instance" "web" {
     ami           = "ami-0c55b159cbfafe1f0"
     instance_type = var.instance_type

     tags = {
       Name = "AnsibleDemoInstance"
     }

     provisioner "local-exec" {
       command = "echo ${aws_instance.web.public_ip} > ip_address.txt"
     }
   }

   output "instance_ip" {
     value = aws_instance.web.public_ip
   }
   ```

5. **Initialize and Apply Terraform:**
   Run the following commands to initialize and apply the Terraform configuration.

   ```sh
   terraform init
   terraform apply
   ```

**Step 2: Ansible Configuration**

1. **Install Ansible:**
   Install Ansible on your control machine.

   ```sh
   sudo apt-get update
   sudo apt-get install ansible
   ```

2. **Inventory File:**
   Create a `hosts` file to define the inventory.

   ```ini
   [web]
   $(cat ip_address.txt) ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/id_rsa
   ```

3. **Ansible Playbook:**
   Create a `playbook.yml` file to define the configuration tasks.

   ```yaml
   ---
   - name: Configure Web Server
     hosts: web
     become: yes
     tasks:
       - name: Update and upgrade apt packages
         apt:
           update_cache: yes
           upgrade: dist

       - name: Install Nginx
         apt:
           name: nginx
           state: present

       - name: Start Nginx
         service:
           name: nginx
           state: started
           enabled: yes

       - name: Copy index.html
         copy:
           src: ./index.html
           dest: /var/www/html/index.html
           mode: '0644'
   ```

4. **Custom HTML File:**
   Create an `index.html` file to serve as the web server's home page.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
     <title>Welcome to Ansible Web Server</title>
   </head>
   <body>
     <h1>Success! The Ansible Web Server is Up and Running!</h1>
   </body>
   </html>
   ```

**Step 3: Integration and Execution**

1. **Terraform Output:**
   Ensure that the public IP address of the EC2 instance is saved to `ip_address.txt`.

2. **Run Ansible Playbook:**
   Execute the Ansible playbook using the inventory file.

   ```sh
   ansible-playbook -i hosts playbook.yml
   ```

3. **Verify Setup:**
   Access the public IP address in a web browser to verify that the Nginx web server is running and serving the custom HTML page.

**Step 4: Clean Up**

1. **Destroy Infrastructure:**
   Use Terraform to destroy the infrastructure when it's no longer needed.

   ```sh
   terraform destroy
   ```

#### Best Practices

**1. Modularize Code:**
   - Separate Terraform and Ansible configurations into logical modules for better organization and reusability.

**2. Use Variables and Templates:**
   - Leverage variables in Terraform and templates in Ansible to create flexible and reusable configurations.

**3. Secure Credentials:**
   - Use environment variables, Terraform Cloud, or Ansible Vault to manage and secure sensitive information.

**4. Version Control:**
   - Store all configurations in a version control system like Git to track changes and collaborate with team members.

**5. Documentation:**
   - Document the setup, configurations, and any custom scripts to provide context and usage instructions for others.

By combining Terraform and Ansible, you can achieve a robust and automated workflow for provisioning and configuring infrastructure, ensuring consistency, scalability, and efficiency in your infrastructure management processes.
