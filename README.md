# k3skubevirt

## Steps for testing KubeVirt with K3S

| Type       | Description                                    | Info |
| ---------- | ---------------------------------------------- | ---------------------------------------------------------------------------------------- |
| OS         | Install Minimal Ubuntu 20.04 LTS               | NB. For test I have installed minimal Desktop and SSH server<br>`sudo apt install openssh-server` |
| Test only  | SUDO change                                    | visudo and added NOPASSWD to sudo'ers group for ease of use in testing |
| OS         | Basic patching                                 | `sudo apt update && sudo apt upgrade -y` |
| Security   | Generate new SSH keys for host                 | `ssh-keygen -q -t rsa -N '' -f ~/.ssh/id_rsa <<<y 2>&1 >/dev/null` |
| Security   | Add self SSH key into `authorized_keys`        | `cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys` |
| Hardening  | Install fail2ban and protect SSH daemon        | `sudo apt install fail2ban -y` |
| Clean      | Remove packages no longer required             | `sudo apt autoremove -y` |
| Clean      | Clean up cache                                 | `sudo apt clean` |
| Pre-req    | Install any pre-requisities                    | `sudo apt install -y curl git qemu-kvm libvirt-daemon-system libvirt-clients` |
| Pre-req    | Install any optional pre-requisities           | `sudo apt install -y bridge-utils virtinst virt-manager` |
| Pre-req    | Clone test pack from Git                       | `git clone https://github.com/dcgsteve/k3skubevirt.git`
| Pre-req    | Reboot                                         | |
| PaaS       | Install K3S                                    | curl -sfL https://get.k3s.io | sh - |
| Test Pack  | Update config after booting up K3S             | `booted` |
| PaaS       | Wait for K3S to be ready                       | Wait until `k3s kubectl get node` returns Status of Ready |
| Test Pack  | CDI                                            | `cdiadd` |
| Test Pack  | KubeVirt                                       | `kvadd` |
| Test Pack  | Virtctl                                        | `vcadd` |
| Test Pack  | Wait for KV deployment                         | `kubectl get all -n kubevirt` - wait for main kubevirt container to have a PHASE of Deployed|
| Test Pack  | Start CDI proxy                                | Run `cdistartproxy` in a seperate SSH session or in the background |
| Test Pack  | Upload image(s)                                | Upload any qcow2 images you need using `image-upload xxx` where `xxx` is the qcow2 image file name *without .qcow2 at the end*, e.g. `image-upload win10g1` |
| Test Pack  | VM | Use example YAML files to create a VM with `kubectl apply -f xxxx.yaml` |
| Test Pack  | VM | If your config doesn't auto start VM then start it manually | `virtctl start xxx` |
| Test pack  | VM | Check if VM is up and running `virtctl vnc xxx` | 
