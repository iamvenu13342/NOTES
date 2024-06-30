<h1>Network Automation Using Ansible</h1>

#### Introduction to Network Automation

**Network automation** refers to using software to automate the management, configuration, testing, deployment, and operation of network devices. Ansible provides a powerful platform for automating network tasks across a variety of devices and vendors.

**Benefits of Network Automation:**
- **Consistency:** Ensures uniform configuration across devices.
- **Efficiency:** Reduces manual configuration time.
- **Error Reduction:** Minimizes human errors in configuration.
- **Scalability:** Easily scales operations across large network environments.
- **Compliance:** Ensures configurations adhere to policies and standards.

#### Key Concepts in Ansible for Network Automation

**1. Network Modules:**
Ansible provides specific modules designed for interacting with network devices. These modules are categorized by vendor and functionality, such as Cisco, Juniper, and Arista.

**2. Network Roles:**
Predefined roles simplify the setup and configuration of network devices. These roles can be found on Ansible Galaxy.

**3. Connection Plugins:**
Ansible uses connection plugins like `network_cli`, `httpapi`, `netconf`, and `local` to manage network devices.

#### Setting Up Ansible for Network Automation

**Installing Ansible:**
Ensure you have Ansible installed on your control machine. For network automation, you may also need to install additional collections.

```sh
pip install ansible
ansible-galaxy collection install cisco.ios
ansible-galaxy collection install juniper.junos
```

**Configuring Inventory:**
Define your network devices in the inventory file with appropriate connection parameters.

**Example `hosts` file:**
```ini
[routers]
router1 ansible_host=192.168.1.1 ansible_user=admin ansible_password=pass ansible_network_os=ios
router2 ansible_host=192.168.1.2 ansible_user=admin ansible_password=pass ansible_network_os=ios

[switches]
switch1 ansible_host=192.168.2.1 ansible_user=admin ansible_password=pass ansible_network_os=eos
switch2 ansible_host=192.168.2.2 ansible_user=admin ansible_password=pass ansible_network_os=eos
```

**Configuring Ansible:**
Adjust the `ansible.cfg` file to specify the connection type and timeout settings.

```ini
[defaults]
inventory = ./hosts
host_key_checking = False

[ssh_connection]
ssh_args = -o StrictHostKeyChecking=no
```

#### Writing Playbooks for Network Automation

**Basic Playbook Structure:**
A playbook for network automation follows the same structure as other Ansible playbooks but uses network-specific modules.

**Example Playbook: Configure an Interface:**
```yaml
---
- name: Configure network devices
  hosts: routers
  gather_facts: no
  connection: network_cli
  tasks:
    - name: Configure interface on Cisco IOS
      cisco.ios.ios_interface:
        name: GigabitEthernet0/1
        description: Uplink to switch
        enabled: yes
        state: present
```

**Example Playbook: Backup Configurations:**
```yaml
---
- name: Backup device configurations
  hosts: routers
  gather_facts: no
  connection: network_cli
  tasks:
    - name: Backup Cisco IOS configuration
      cisco.ios.ios_config:
        backup: yes
        backup_options:
          dir_path: /backups
```

#### Advanced Network Automation Tasks

**1. Configuring VLANs:**
Automate the creation and assignment of VLANs on switches.

```yaml
---
- name: Configure VLANs
  hosts: switches
  gather_facts: no
  connection: network_cli
  tasks:
    - name: Create VLAN 10
      arista.eos.eos_vlan:
        vlan_id: 10
        name: Sales
        state: present

    - name: Assign VLAN 10 to interface
      arista.eos.eos_vlan_interface:
        vlan_id: 10
        name: Ethernet2
        state: present
```

**2. Deploying Configuration Templates:**
Use Jinja2 templates to deploy configurations dynamically.

**Template (`router.j2`):**
```jinja2
hostname {{ inventory_hostname }}
interface {{ interface_name }}
  description {{ interface_description }}
  ip address {{ ip_address }} {{ subnet_mask }}
```

**Playbook:**
```yaml
---
- name: Deploy configuration template
  hosts: routers
  gather_facts: no
  connection: network_cli
  vars:
    interface_name: GigabitEthernet0/1
    interface_description: Uplink to switch
    ip_address: 192.168.1.1
    subnet_mask: 255.255.255.0
  tasks:
    - name: Apply configuration template
      cisco.ios.ios_config:
        src: router.j2
```

**3. Running Commands and Gathering Facts:**
Execute arbitrary commands on network devices and gather operational data.

**Example:**
```yaml
---
- name: Gather interface status
  hosts: routers
  gather_facts: no
  connection: network_cli
  tasks:
    - name: Show interface status
      cisco.ios.ios_command:
        commands:
          - show interfaces status
      register: result

    - name: Display interface status
      debug:
        var: result.stdout_lines
```

#### Best Practices for Network Automation with Ansible

**1. Use Version Control:**
Store your playbooks, roles, and templates in a version control system like Git to track changes and collaborate with team members.

**2. Test in a Staging Environment:**
Always test your automation scripts in a staging or lab environment before deploying to production.

**3. Idempotency:**
Ensure your playbooks are idempotent, meaning they can be run multiple times without causing unintended changes.

**4. Documentation:**
Document your playbooks, roles, and templates thoroughly to provide context and usage instructions for other team members.

**5. Modularize Playbooks:**
Break down complex configurations into smaller, reusable playbooks or roles to improve maintainability and readability.

**6. Use Variables and Templates:**
Leverage variables and Jinja2 templates to create flexible and reusable configurations.

**7. Secure Sensitive Data:**
Use Ansible Vault to encrypt sensitive information such as passwords and API keys.

**8. Monitor and Log Automation Activities:**
Implement logging and monitoring to track the execution and outcome of automation tasks.

By leveraging Ansible for network automation, you can streamline your network management processes, reduce errors, and improve the overall efficiency and reliability of your network operations.
