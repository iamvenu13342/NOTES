<h1>Ansible Variables and Precedence</h1>

#### Create AWS Resources using Ansible (Collections)

**AWS Collections in Ansible:**
Ansible collections allow you to manage AWS resources efficiently. Collections bundle modules, plugins, and roles for specific cloud providers or services.

**AWS Collection Installation:**
To use AWS modules, you need to install the `amazon.aws` collection:
```sh
ansible-galaxy collection install amazon.aws
```

**Example Playbook to Create an AWS EC2 Instance:**
```yaml
---
- name: Launch an EC2 instance
  hosts: localhost
  gather_facts: false
  vars:
    ec2_key_name: my_key
    ec2_instance_type: t2.micro
    ec2_image_id: ami-0c55b159cbfafe1f0
    ec2_region: us-east-1
  tasks:
    - name: Launch EC2 instance
      amazon.aws.ec2_instance:
        key_name: "{{ ec2_key_name }}"
        instance_type: "{{ ec2_instance_type }}"
        image_id: "{{ ec2_image_id }}"
        region: "{{ ec2_region }}"
        wait: yes
      register: ec2
    - name: Display instance information
      debug:
        msg: "Instance ID: {{ ec2.instance_id }} - Public IP: {{ ec2.public_ip_address }}"
```

#### Understanding Ansible Variables and Their Scope with an Example

**Scopes of Variables:**
1. **Global:** Variables defined in `ansible.cfg` or passed via command-line.
2. **Play:** Defined in the playbook and visible to all tasks within the play.
3. **Host:** Variables specific to a host, defined in inventory or host_vars files.
4. **Role:** Defined in role defaults, vars, or tasks, and visible only within the role.

**Example:**
```yaml
---
- name: Example of variable scopes
  hosts: webservers
  vars:
    play_var: "Value from Play scope"
  tasks:
    - name: Display host_var
      debug:
        msg: "host_var: {{ host_var }}"
    - name: Display play_var
      debug:
        msg: "play_var: {{ play_var }}"
```

**Inventory File:**
```ini
[webservers]
webserver1 ansible_host=192.168.1.10 host_var="Value from Host scope"
```

#### Jinja2 Templating - Utilizing Advanced Templating Features

**Using Jinja2 Templates:**
Jinja2 templates allow you to dynamically create configuration files using variables and conditionals.

**Example Template (templates/nginx.conf.j2):**
```nginx
worker_processes {{ nginx_worker_processes }};
events {
    worker_connections {{ nginx_worker_connections }};
}
http {
    server {
        listen {{ http_port }};
        location / {
            root /var/www/html;
            index index.html index.htm;
        }
    }
}
```

**Playbook Using the Template:**
```yaml
---
- name: Configure Nginx with Template
  hosts: webservers
  vars:
    nginx_worker_processes: auto
    nginx_worker_connections: 1024
    http_port: 80
  tasks:
    - name: Deploy nginx configuration
      template:
        src: templates/nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify:
        - Restart Nginx

  handlers:
    - name: Restart Nginx
      service:
        name: nginx
        state: restarted
```

#### Variable Precedence: How Ansible Resolves Conflicts Between Different Variable Sources

Ansible follows a specific order to resolve conflicts among variables:

1. **Role defaults (`role/defaults/main.yml`)**
2. **Inventory group_vars/all**
3. **Playbook group_vars/all**
4. **Inventory group_vars/*
5. **Playbook group_vars/*
6. **Inventory host_vars/*
7. **Playbook host_vars/*
8. **Host facts**
9. **Play vars**
10. **Play vars_prompt**
11. **Play vars_files**
12. **Role vars (`role/vars/main.yml`)**
13. **Block vars**
14. **Task vars (only for the task)**
15. **Include vars**
16. **Set_facts / Registered vars**
17. **Role (and include_role) params**
18. **Include params**
19. **Extra vars (`-e` on the command line)**

**Example:**
```yaml
---
- name: Variable Precedence Example
  hosts: webservers
  vars:
    common_var: "Value from Play scope"
  tasks:
    - name: Display common_var
      debug:
        msg: "common_var: {{ common_var }}"
```

**Inventory File:**
```ini
[webservers]
webserver1 ansible_host=192.168.1.10 common_var="Value from Inventory scope"
```

**Command Line:**
```sh
ansible-playbook -i inventory.ini site.yml -e "common_var='Value from Extra Vars'"
```

#### Hands-on: Using Variables in Playbooks and Roles

**Step-by-Step Guide:**

1. **Project Structure:**
   ```plaintext
   project/
   ├── site.yml
   ├── inventory.ini
   └── roles/
       └── webserver/
           ├── defaults/
           │   └── main.yml
           ├── handlers/
           │   └── main.yml
           ├── tasks/
           │   └── main.yml
           ├── templates/
           │   └── nginx.conf.j2
           └── vars/
               └── main.yml
   ```

2. **Role Defaults (`roles/webserver/defaults/main.yml`):**
   ```yaml
   ---
   nginx_worker_processes: auto
   nginx_worker_connections: 1024
   ```

3. **Role Vars (`roles/webserver/vars/main.yml`):**
   ```yaml
   ---
   http_port: 8080
   ```

4. **Role Tasks (`roles/webserver/tasks/main.yml`):**
   ```yaml
   ---
   - name: Install nginx
     apt:
       name: nginx
       state: present

   - name: Configure nginx
     template:
       src: nginx.conf.j2
       dest: /etc/nginx/nginx.conf
     notify:
       - Restart nginx

   - name: Start nginx
     service:
       name: nginx
       state: started
       enabled: yes
   ```

5. **Playbook (`site.yml`):**
   ```yaml
   ---
   - name: Deploy Web Application
     hosts: webservers
     become: yes
     vars:
       http_port: 80
       app_version: "1.2.3"
     roles:
       - webserver
   ```

**Execution:**
```sh
ansible-playbook -i inventory.ini site.yml
```

By understanding and effectively using variables and their precedence, you can create dynamic, reusable, and maintainable automation scripts with Ansible. This approach ensures flexibility and consistency in managing your infrastructure and applications.
