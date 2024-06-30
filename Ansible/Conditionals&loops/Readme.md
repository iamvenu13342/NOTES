<h1> 7: Ansible Conditionals and Loops</h1>

#### Using Conditionals in Ansible to Control Task Execution

Conditionals in Ansible allow you to control whether a task should run based on the state of the system or the value of variables. You can use conditionals to make your playbooks more dynamic and responsive to the environment they are running in.

**Basic Syntax for Conditionals:**
```yaml
- name: Task Name
  module:
    parameter: value
  when: condition
```

**Examples:**

1. **Check OS Distribution:**
   ```yaml
   - name: Install nginx on Ubuntu
     apt:
       name: nginx
       state: present
     when: ansible_facts['os_family'] == 'Debian'
   ```

2. **Check Variable Value:**
   ```yaml
   - name: Restart nginx if necessary
     service:
       name: nginx
       state: restarted
     when: nginx_needs_restart
   ```

3. **Using `or`, `and`, and `not` Operators:**
   ```yaml
   - name: Install nginx if on Debian or RedHat
     package:
       name: nginx
       state: present
     when: ansible_facts['os_family'] == 'Debian' or ansible_facts['os_family'] == 'RedHat'
   ```

#### Implementing Loops for Repetitive Tasks

Loops in Ansible allow you to perform the same task multiple times with different items or values. This is useful for tasks like creating multiple users, installing multiple packages, or performing operations on multiple hosts.

**Basic Syntax for Loops:**
```yaml
- name: Task Name
  module:
    parameter: value
  loop:
    - item1
    - item2
    - item3
```

**Examples:**

1. **Installing Multiple Packages:**
   ```yaml
   - name: Install multiple packages
     apt:
       name: "{{ item }}"
       state: present
     loop:
       - nginx
       - git
       - curl
   ```

2. **Creating Multiple Users:**
   ```yaml
   - name: Create multiple users
     user:
       name: "{{ item.name }}"
       state: present
       groups: "{{ item.groups }}"
     loop:
       - { name: 'alice', groups: 'admin' }
       - { name: 'bob', groups: 'users' }
       - { name: 'charlie', groups: 'users' }
   ```

3. **Using `with_items`:**
   ```yaml
   - name: Add multiple SSH keys
     authorized_key:
       user: "{{ item.user }}"
       key: "{{ item.key }}"
     with_items:
       - { user: 'alice', key: 'ssh-rsa AAA...' }
       - { user: 'bob', key: 'ssh-rsa BBB...' }
   ```

#### Practical Examples of Conditionals and Loops in Playbooks

**Example Playbook:**

1. **Check OS and Install Packages:**
   ```yaml
   ---
   - name: Ensure Nginx is installed
     hosts: webservers
     become: yes
     tasks:
       - name: Install nginx on Debian-based systems
         apt:
           name: nginx
           state: present
         when: ansible_facts['os_family'] == 'Debian'

       - name: Install nginx on RedHat-based systems
         yum:
           name: nginx
           state: present
         when: ansible_facts['os_family'] == 'RedHat'
   ```

2. **Create Multiple Users:**
   ```yaml
   ---
   - name: Manage users
     hosts: all
     become: yes
     vars:
       users:
         - name: 'alice'
           groups: 'admin'
         - name: 'bob'
           groups: 'users'
         - name: 'charlie'
           groups: 'users'
     tasks:
       - name: Create users
         user:
           name: "{{ item.name }}"
           state: present
           groups: "{{ item.groups }}"
         loop: "{{ users }}"
   ```

3. **Add SSH Keys Conditionally:**
   ```yaml
   ---
   - name: Setup SSH keys
     hosts: all
     become: yes
     vars:
       ssh_keys:
         - user: 'alice'
           key: 'ssh-rsa AAA...'
         - user: 'bob'
           key: 'ssh-rsa BBB...'
     tasks:
       - name: Add SSH keys if user exists
         authorized_key:
           user: "{{ item.user }}"
           key: "{{ item.key }}"
         when: "'{{ item.user }}' in lookup('env', 'USER_LIST')"
         loop: "{{ ssh_keys }}"
   ```

**Advanced Examples:**

1. **Conditionally Include Tasks Based on Variable:**
   ```yaml
   ---
   - name: Conditional tasks
     hosts: all
     tasks:
       - name: Include database setup
         include_tasks: setup-database.yml
         when: db_setup_required
   ```

2. **Dynamic Looping with `with_dict`:**
   ```yaml
   ---
   - name: Install software from a dictionary
     hosts: all
     become: yes
     vars:
       software:
         nginx:
           version: '1.18.0'
           state: present
         git:
           version: '2.28.0'
           state: latest
     tasks:
       - name: Install software
         package:
           name: "{{ item.key }}"
           version: "{{ item.value.version }}"
           state: "{{ item.value.state }}"
         with_dict: "{{ software }}"
   ```

By leveraging conditionals and loops, you can create powerful, flexible playbooks that adapt to different environments and perform complex automation tasks efficiently. This approach enhances the maintainability and scalability of your Ansible automation scripts.
