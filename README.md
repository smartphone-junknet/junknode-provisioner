# Provisioning of JunkNet nodes

Ansible playbook collection to install Podman and Nomad on a cluster of nodes running on PostMarketOS

## Requirements

- Already installed PostMarketOS on the node to be provisioned (from now on, called the *managed host*)
- At least 250 MB of free space inside the root partition of the managed host
- The user `junker` must exist, with password login enabled and sudoers privileges (this is all done by choosing the user name and password from the `pmbootstrap init` prompts)
- Set the user password inside the vault file at `host_vars/pmos_usb/vault` as the value for the key `vault_junker_password`
- Set the gossip encryption key (see [this documentation](https://developer.hashicorp.com/nomad/tutorials/transport-security/security-gossip-encryption)) inside the vault file at `host_vars/pmos_usb/vault` as the value for the key `vault_nomad_gossip_key`
- A working Internet connection is required on the managed host
- The MAC address on the LAN network interface must not be randomized
- The Python 3 interpreter should be available from all users PATH variable on the managed host

### Creating an Ansible vault file

Create the file for the first time with the following command:

```bash
ansible-vault create host_vars/pmos_usb/vault
```

To edit that file, launch the same command as above but replace `create` with `edit`

## Provision a node connected via USB

To Install Podman, Nomad and their dependencies, and make Podman API and Nomad services start on boot, run the following playbook:

```bash
ansible-playbook provision_node.yaml --ask-vault-pass
```

### Skipping free space assertion

If you are running the playbook again after the first run, and the available space is lower than the required 250 MB, you can skip the check that will fail the playbook adding the argument `--skip-tags check-free-space` to the `ansible-playbook` command shown above.
