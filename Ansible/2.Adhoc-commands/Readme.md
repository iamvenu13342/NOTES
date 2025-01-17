<h1> Ansible Adhoc Commands</h1>

- Ansible adhoc commands are used to perform quick, one-off tasks on remote machines.
- These commands do not require a playbook and are useful for rapid system management.

#### Passwordless Authentication
To simplify running adhoc commands, set up passwordless SSH access:

1. **Generate SSH Key Pair:**
   ```sh
   ssh-keygen -t rsa -b 4096
   ```
   Follow the prompts to save the key (default location is `~/.ssh/id_rsa`).

2. **Copy SSH Key to Target Machine:**
   ```sh
   ssh-copy-id user@hostname
   ```
   This command adds your public key to the `~/.ssh/authorized_keys` file on the remote machine.

#### Ansible Inventory
Ansible uses an inventory file to define the hosts it will manage. The default inventory file is located at `/etc/ansible/hosts`, but you can specify a different file using the `-i` option.

**Example Inventory File:**
```ini
[webservers]
server1 ansible_host=192.168.1.1
server2 ansible_host=192.168.1.2

[dbservers]
db1 ansible_host=192.168.1.3
```
The inventory file can also include variables and group variables.

#### Understanding Adhoc Commands
Adhoc commands use the `ansible` command-line tool. The basic syntax is:
```sh
ansible <host-pattern> -m <module> -a <arguments>
```
- **`<host-pattern>`:** The target hosts, as defined in the inventory file.
- **`<module>`:** The Ansible module to use (e.g., `ping`, `shell`, `copy`).
- **`<arguments>`:** Module-specific arguments.

**Basic Usage Example:**
```sh
ansible all -m ping
```
This command pings all hosts in the inventory.

#### Examples of Common Adhoc Commands
Here are some common adhoc commands used for system management tasks:

1. **Check Disk Space:**
   ```sh
   ansible all -m shell -a "df -h"
   ```
   This runs the `df -h` command on all hosts to check disk space usage.

2. **Install a Package:**
   ```sh
   ansible webservers -m apt -a "name=apache2 state=present"
   ```
   This installs the `apache2` package on all hosts in the `webservers` group using the `apt` module.

3. **Restart a Service:**
   ```sh
   ansible webservers -m service -a "name=apache2 state=restarted"
   ```
   This restarts the `apache2` service on all hosts in the `webservers` group.

4. **Copy a File:**
   ```sh
   ansible all -m copy -a "src=/local/path dest=/remote/path"
   ```
   This copies a file from the local machine to all hosts.

5. **Reboot a Host:**
   ```sh
   ansible all -m reboot
   ```
   This reboots all hosts.

#### Exploring the Power of Adhoc Commands
Adhoc commands are powerful for quickly executing tasks across multiple systems. They are particularly useful for:

- **Quick System Checks:**
  ```sh
  ansible all -a "/bin/uptime"
  ```
  This checks the uptime of all hosts.

- **Managing Users:**
  ```sh
  ansible all -m user -a "name=johndoe state=present"
  ```
  This ensures that the user `johndoe` exists on all hosts.

- **Running Custom Scripts:**
  ```sh
  ansible all -m script -a "/path/to/script.sh"
  ```
  This runs a custom script on all hosts.

By leveraging the power of adhoc commands, you can efficiently manage and automate tasks across your infrastructure without the need for writing detailed playbooks.
