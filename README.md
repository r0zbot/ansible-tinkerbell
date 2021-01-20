This repo contains some ansible playbooks to help setting up a tinkerbell testing environment. By default, it uses the hosts contained locally in the `hosts` file.

## Playbooks

- ### boostrap.yml
  This playbook verifies the machine has python installed, and if it doesn't, installs it.

- ### template.yml
  Initializes the specified template (for now hardcoded to hello-world) to all the hosts under the `worker` group.

- ### generate_workflow.yml
  Helper for `template.yml` that generates the hardware configuration from its template and pushes it, creating a new workflow for it. It should not be run directly.

- ### setup.yml
  Sets up the environment and runs `setup.sh`, which pulls all the images and downloads OSIE. If `copy_osie: true`, then instead of downloading OSIE it copies it from the ansible host. It also starts all services included in the `docker-compose.yml` file.

- ### all.yml
  All the above :)

## Inventory variables
Some inventory variables are required to correctly set up tinkerbell:

### `[provisioner]`  
- `network_interface`: The name of the network interface (as shown in `ip a`) tinkerbell should use on the provisioner.
- `gateway_ip`: The IP the provisioner should have on the internal network, that workers will use to reach it. Doesn't need to match the value in `ansible_host`, as it will be added if needed.

### `[worker]`  
- `mac_address`: The mac address of the hardware to be added.
- `uuid`: A random UUID to identify this worker.
- `template`: Which template should be assigned to this worker when creating a new workflow.
- `gateway`: IP address of the provisioner. Should probably match `gateway_ip` from above
- `netmask`: Netmask for the worker's network.

## TODO

- [] Generic templates
- [] Allow a list of templates to be passed
- [] Make template generation idempotent
- [] Make workflow generation idempotent
- [] Dynamic inventory