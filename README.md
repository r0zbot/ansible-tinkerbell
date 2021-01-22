This repo contains some ansible playbooks to help setting up a tinkerbell testing environment. By default, it uses the hosts contained locally in the `hosts` file.

## Playbooks

- ### boostrap.yml
  This playbook verifies the machine has python installed, and if it doesn't, installs it.

- ### update_templates.yml
  Creates tinkerbell templates for the templates inside `templates/`. **Make sure the files don't contain any spaces!"

- ### update_workers.yml
  Attributes the template specified in the `template` variable to each host under the `worker` group by creating a workflow for it.

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

- [ ] docker-compose up as a handler
- [x] Generic templates
- [ ] Allow a list of templates to be passed
- [ ] Make template generation idempotent
- [ ] Make workflow generation idempotent
- [ ] Dynamic inventory
- [ ] Maybe organize into roles?