# Install a VM Bridge Network

## Install the latest Python and setup a new Python virtualenv

```bash
sudo yum install -y git python3 python3-pip python3-virtualenv python3-libselinux python3-libsemanage python3-policycoreutils
virtualenv-3 ~/python
source ~/python/bin/activate
echo "source ~/python/bin/activate" | tee -a ~/.bashrc
```

## Install the latest Ansible

```bash
pip install setuptools_rust wheel
pip install --upgrade pip
pip install ansible selinux setools
```

## Install the vm_network ansible role

### Create a directory for the ansible role. 

```bash
install -d ~/.ansible/roles/computate.computate_vm_network
```

### Clone the vm_network ansible role. 

```bash
git clone git@github.com:computate-org/computate_vm_network.git ~/.ansible/roles/computate.computate_vm_network
```

## Run the vm_network ansible playbook to install the application locally (requires sudo privileges with -K). 

- Run `nmcli c` and look for one with TYPE=ethernet. 
NETWORK_ETHERNET_CONNECTION: "enp2s0"
- Run `ip addr` and look for the mac address of the ethernet connection above. 
NETWORK_BRIDGE_MAC: "40:40:40:40:40:40"
- Run `nmcli d` and look for the DEVICE of the ethernet CONNECTION above. 
NETWORK_ETHERNET_DEVICE: "enp2s0"
- Run `ip addr` and look for the IP address of the ethernet connection above. 
NETWORK_IP_ADDRESS: "192.168.0.2"
- Set the gateway to be the NETWORK_IP_ADDRESS above with a 1 at the end instead of another number. 
NETWORK_GATEWAY: "192.168.0.1"
- You can set the DNS to the same as the NETWORK_GATEWAY above. 
NETWORK_DNS: "192.168.0.1"
- You can set this to be the name of the NETWORK_ETHERNET_CONNECTION before. 
NETWORK_BRIDGE_SLAVE_CONNECTION: "{{ NETWORK_ETHERNET_CONNECTION }}"
- You can name these uniquely to other computers on the network with bridge networks: br0, br1, br2, etc. 
NETWORK_BRIDGE_CONNECTION: "br0"


```bash
cd ~/.ansible/roles/computate.computate_vm_network
ansible-playbook -K install.yml -e NETWORK_ETHERNET_CONNECTION="enp2s0" -e "NETWORK_BRIDGE_MAC=40:40:40:40:40:40" -e "NETWORK_ETHERNET_DEVICE=enp2s0" -e "NETWORK_IP_ADDRESS=192.168.0.2" -e "NETWORK_GATEWAY=192.168.0.1" -e "NETWORK_DNS=192.168.0.1" -e "NETWORK_BRIDGE_SLAVE_CONNECTION={{ NETWORK_ETHERNET_CONNECTION }}" -e "NETWORK_BRIDGE_CONNECTION=br0"
```

