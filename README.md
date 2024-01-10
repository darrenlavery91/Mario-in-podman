# Mario-in-Podman: Ansible Playbooks for Managing the Mario Brothers Container
![Uploading Screenshot 2024-01-10 at 10.57.30.png…]()


This repository provides Ansible playbooks for managing the deployment of the Mario Brothers game container using Podman.

## Usage:

### Playbook: Starting Mario Brothers Container

```yaml
- hosts: localhost
  collections:
    - containers.podman
  tasks:
    - name: Entering Mario Land
      containers.podman.podman_container:
        name: mario_game
        image: "docker.io/kaminskypavel/mario:latest"
        state: started
        ports: "4545:8080"
      register: mario

    - debug:
        msg: "Go down the tube here: http://127.0.0.1:4545"
      when: mario.container.State.ExitCode == 0

    - debug:
        msg: "Tube is blocked"
      when: mario.container.State.ExitCode == 1
```

**Usage:**

1. **Install Ansible:**
   - **Linux (Red Hat):**
     ```bash
     sudo yum install -y ansible
     ```

   - **Mac (via Brew):**
     ```bash
     brew install ansible
     ```

2. Create the directory in your user space:
   ```bash
   mkdir -p ~/ansible/
   cd ~/ansible
   ```

3. Create the `ansible.cfg` file:
   ```bash
   vim ansible.cfg
   ```
   Add the following configuration and save:
   ```ini
   [defaults]
   inventory=./inventory
   ```

4. Create the `inventory` file:
   ```bash
   vim inventory
   ```
   Add the following, replacing `your-local-ip` with your actual local IP:
   ```ini
   [localhost]
   your-local-ip ansible_connection=local
   ```
   Example:
   ```ini
   [localhost]
   192.168.101.214 ansible_connection=local
   ```

5. Run `ansible --version` to verify if the configuration file has been picked up.

6. **Install Podman:**
   - **Red Hat (RHEL):**
     ```bash
     sudo yum install -y podman
     ```

   - **Mac (via Brew):**
     ```bash
     brew install podman
     ```

7. Run the Ansible playbook to start the Mario Brothers container:
   ```bash
   ansible-playbook mario_pod.yml
   ```

8. After successful execution, access Mario Land through the provided URL.

9. Run the Ansible playbook to stop and remove the Mario Brothers container:
   ```bash
   ansible-playbook mario-pod-rm.yml
   ```

## Note:

- Customize the playbooks according to your specific requirements.
- Ensure proper network configurations and permissions for successful execution.
- A big thank you to kaminskypavel for providing the Mario Brothers container image.
