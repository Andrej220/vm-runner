
# VM Management Script

This script provides a lightweight command-line interface to configure, start, stop, and monitor a virtual machine (VM) environment. It uses QEMU for virtualization and relies on `yq` to parse YAML configuration files, allowing flexible configuration.

## Prerequisites
Ensure you have the following installed:

- **yq**: For parsing YAML configuration files.
- **qemu-system-x86_64**: QEMU for VM creation and management.
- **socat**: For interacting with the VM's monitor interface.

### Dependencies Installation

#### Ubuntu:
```bash
sudo apt update
sudo apt install yq qemu-system-x86 socat
```

#### Fedora:
```bash
sudo dnf install yq qemu-system-x86 socat
```

## Configuration

Configuration options are available via a YAML file (default `<script_name>_config.yml`) located in the root directory. Customize this file with necessary variables for VM setup.

### Key Configuration Options in `config.yml`

- **project_name**: The name of the project, used for naming the VM and the PID file. It refers to another project, e.g., k3s-cluster-lab https://github.com/Andrej220/k3s-cluster-lab.git, and determines the names of hard disk images. The script will look for primary disk images as `${VM_DIR}${PROJECT_NAME}_snapshot.qcow2`; any other `.qcow2` images in this directory will be added as additional disks.
- **ram**: RAM allocated to the VM (e.g., `4G`).
- **cpus**: Number of CPUs assigned to the VM.
- **vm_dir**: Directory where VM images are stored.
- **bridge_name**: Bridge name for network bridging.
- **interface**: Network interface for the bridge.
- **tapinterface**: TAP device to use for the bridge.
- **hostports**: Array of port mappings in `host:vm` format for port forwarding.

Example `config.yml`:
```yaml
project_name: "my_project"
ram: "8G"
cpus: 4
vm_dir: "./vm/"
bridge_name: "br0"
interface: "eth1"
tapinterface: "tap0"
hostports:
  - host: "8022"
    vm: "22"
  - host: "9090"
    vm: "9090"
```

### Command-Line Arguments

The script allows command-line arguments to override `config.yml` values, with priority from command-line arguments > config file > defaults.

- `--project-name`: Overrides the `project_name` in the config.
- `--ram`: Overrides the RAM setting.
- `--cpus`: Overrides the CPU setting.
- `--vm-dir`: Overrides the VM directory.
- `--bridge-name`: Overrides the bridge name.
- `--interface`: Overrides the network interface.
- `--tap`: Overrides the TAP device.

## Usage

Ensure the script has execute permissions:
```bash
chmod +x <script_name>.sh
```

### Commands

- `start`: Starts the VM. If an existing VM instance is running, it will skip reinitializing it.
- `stop`: Stops the VM gracefully, using QEMU monitor commands.
- `status`: Checks and displays the current VM status.
- `reset`: Performs a soft reset on the VM.
- `check-bridge`: Sets up the specified network bridge if it doesn't exist.
- `netinfo`: Retrieves network information from the VM.

Example:
```bash
./<script_name>.sh start --project-name "my_project" --ram "8G" --cpus 4
./<script_name>.sh stop      # Stop the VM
./<script_name>.sh status    # Display the VM's status
./<script_name>.sh reset     # Reset the VM
./<script_name>.sh check-bridge # Set up or verify the network bridge
./<script_name>.sh netinfo   # Show VM network info
```

### Port Forwarding Example

For example, to access the VM via SSH using port forwarding:
```bash
ssh -p 8022 user@localhost  # Access the VM via SSH on forwarded port 8022
```

## Troubleshooting

1. **Permissions for TAP Devices**: If you encounter permissions issues when creating a TAP device, try running the script with `sudo`.
2. **QEMU Startup Issues**: If QEMU fails to start, verify the paths to disk images and ensure necessary files are in the `vm_dir` directory.

---
