### <h1>Writing Your First Ansible Playbook</h1>

#### Understanding YAML Basics

YAML (YAML Ain't Markup Language) is a human-readable data serialization format often used for configuration files. Ansible playbooks are written in YAML, making them easy to read and write.

**YAML Syntax Basics:**
1. **Key-Value Pairs:** Represented as `key: value`
2. **Lists:** Represented with a dash (`-`) or inline with square brackets (`[]`)
3. **Dictionaries:** Nested key-value pairs

**Example YAML Structure:**
```yaml
simple_key: simple_value

list_example:
  - item1
  - item2
  - item3

dictionary_example:
  key1: value1
  key2: value2

nested_example:
  parent_key:
    child_key: child_value
```

#### Ansible Playbook Structure

An Ansible playbook is a YAML file that defines a series of tasks to be executed on specified hosts. It consists of multiple components: playbooks, plays, tasks, modules, and collections.

**Components:**

1. **Playbook:** The main YAML file that contains plays.
2. **Play:** Defines a set of tasks to be executed on a group of hosts.
3. **Task:** Individual actions performed within a play.
4. **Module:** Built-in tools for performing specific tasks (e.g., `apt`, `yum`, `copy`).
5. **Collection:** A distribution format for Ansible content, including roles and modules.

**Basic Playbook Structure:**
```yaml
---
- name: Example Playbook
  hosts: all
  become: yes
  tasks:
    - name: Ensure Apache is installed
      apt:
        name: apache2
        state: present
```

**Explanation:**
- `---`: Indicates the start of a YAML document.
- `- name`: Descriptive name of the playbook.
- `hosts`: Specifies the target hosts (e.g., `all` for all hosts).
- `become`: Allows escalation of privilege (e.g., using `sudo`).
- `tasks`: A list of tasks to execute.
- `name`: Descriptive name of the task.
- `apt`: The module used to install the `apache2` package.
- `state: present`: Ensures the package is installed.

#### Hands-on: Writing a Playbook to Install Apache2 and Deploy a Static App on AWS

**Step-by-Step Playbook:**

1. **Define the Playbook:**
   - Create a new YAML file, e.g., `site.yml`.

2. **Specify the Play:**
   ```yaml
   ---
   - name: Install Apache and Deploy Static Website
     hosts: webservers
     become: yes
     tasks:
       - name: Install Apache
         apt:
           name: apache2
           state: present

       - name: Start and enable Apache
         service:
           name: apache2
           state: started
           enabled: yes

       - name: Copy Static Website
         copy:
           src: /path/to/local/static/site/
           dest: /var/www/html/
   ```

3. **Detailed Breakdown:**
   - `name: Install Apache and Deploy Static Website`: Descriptive name of the play.
   - `hosts: webservers`: Target hosts specified in the inventory file.
   - `become: yes`: Use privilege escalation.
   - `tasks`: List of tasks to execute.
     - **Install Apache:**
       ```yaml
       - name: Install Apache
         apt:
           name: apache2
           state: present
       ```
       - Uses the `apt` module to ensure Apache is installed.
     - **Start and Enable Apache:**
       ```yaml
       - name: Start and enable Apache
         service:
           name: apache2
           state: started
           enabled: yes
       ```
       - Uses the `service` module to start and enable Apache.
     - **Copy Static Website:**
       ```yaml
       - name: Copy Static Website
         copy:
           src: /path/to/local/static/site/
           dest: /var/www/html/
       ```
       - Uses the `copy` module to copy the static website files from the local machine to the remote server.

4. **Execute the Playbook:**
   ```sh
   ansible-playbook -i inventory.ini site.yml
   ```
   - `-i inventory.ini`: Specifies the inventory file.
   - `site.yml`: The playbook file to execute.

By following this structure, you can create and execute a playbook to install Apache and deploy a static website on your web servers. This provides a clear understanding of how to write and structure Ansible playbooks using YAML.
