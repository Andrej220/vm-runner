
# QEMU VM Management Script

This repository contains a Bash script to manage QEMU virtual machines. The script allows you to start, stop, check the status, reset, and retrieve network information of a virtual machine (VM) using QEMU commands.

## Features

- Customizable VM resources (RAM, CPU, disk, and network).
- YAML configuration support for easy setup.
- Command-line argument support to override default settings.
- VM status monitoring and network info retrieval.
- Bridge creation for VM networking.

## Prerequisites

Ensure the following are installed on your system:

- **QEMU**: Required to run virtual machines.
- **`socat`**: Used to communicate with the QEMU monitor via a socket.
- **Bridge utilities**: For managing network bridges.
- **YAML Parsing Utility**: This script uses the `parse_yaml.sh` module from another repository.

## Setup

1. **Clone the repository**:

    ```bash
    git clone git@github.com:Andrej220/vm-runner.git
    cd vm-runner
    ```

2. **Clone the YAML parsing utility**:

    ```bash
    git clone git@github.com:Andrej220/bash_utils.git
    ```

    The `parse_yaml.sh` file should be placed in the `utils/` directory or update the path in the script to match its location.

## Usage

### Script Parameters

The script uses a combination of **default values**, **command-line arguments**, and **YAML configuration files** to set VM parameters.

#### Default Values

- **Project name**: `k3`
- **RAM**: `4G`
- **CPUs**: `2`
- **Host Port**: `8022`
- **VM Port**: `22`
- **VM Directory**: `./vm/`
- **Bridge Name**: `br0`

### YAML Configuration File

To override default values, provide a YAML configuration file. The script will look for a config file named `<script_name>_config.yml` by default, but you can specify your own using the `--config` or `-c` flag.

Example (`my_vm_config.yml`):

```yaml
project_name: "my_vm"
ram: "8G"
cpus: "4"
hostport: "2222"
vmport: "22"
vm_dir: "./my_vm/"
bridge_name: "br1"
```

### Command-Line Arguments

You can override the default values or YAML settings using these arguments:

- `--project-name`: Set the project name.
- `--ram`: Set the RAM for the VM.
- `--cpus`: Set the number of CPUs.
- `--host-port`: Set the host port for port forwarding.
- `--vm-port`: Set the VM's SSH port.
- `--vm-dir`: Set the directory where VM files are stored.
- `--bridge-name`: Set the network bridge name.

Example:

```bash
./vm_manager.sh --project-name "test_vm" --ram "8G" --cpus "4" start
```

### Script Commands

- `start`: Start the VM.
- `stop`: Stop the VM.
- `status`: Get the VM's status.
- `reset`: Reset the VM.
- `check-bridge`: Ensure the network bridge is created.
- `netinfo`: Get network information for the VM.

Example usage:

```bash
./vm_manager.sh start           # Start the VM
./vm_manager.sh stop            # Stop the VM
./vm_manager.sh status          # Check VM status
./vm_manager.sh reset           # Reset the VM
./vm_manager.sh check-bridge    # Create/check the network bridge
./vm_manager.sh netinfo         # Get network information
```

### Disk Management

The script automatically detects additional `.qcow2` files in the VM directory and attaches them as additional drives.

## License

This project is licensed under the MIT License.
