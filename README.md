# ansible_relearn

This repo was used for relearning Ansible and updating my notes from *2.8* to latest version (*2.21*). Was run against ansible_relearn [Containerlab](https://containerlab.dev) lab (Cisco IOS + Arista EOS).

## Setup

```bash
uv sync
uv run ansible-galaxy collection install -r collections/requirements.yml
```

Lab (`basic_lab`, inventory in *ansible-inventory.yml*):

```bash
clab deploy -t basic_lab.clab.yml
clab destroy -t basic_lab.clab.yml -c
```

Ad-hoc examples:

```bash
uv run ansible cisco -l WAN1 -m ios_command -a "commands='show version'"
uv run ansible arista -l ISP1 -m eos_command -a "commands='show version'"
```

## Playbooks (`playbooks/`)

### Top level
- `backup_cfgs.yml` - Back up running configs using several different methods
- `updated_known_hosts.yml` - Collect SSH host keys and add them to known_hosts
- `scrap.yml` - Scratch playbook for creating/cleaning a config directory

### `cfg/` - Configuration
- `apply_ntp_cfg_from_tmpl.yml` - Push NTP config rendered from a Jinja2 template
- `cfg_from_roles.yml` - Build per-device config files from the roles
- `multi_vendor_ntp_syslog.yml` - Configure NTP and syslog on Cisco and Arista with resource modules
- `multi_vendor_resource_mod.yml` - List and print the resource modules available per OS
- `napalm_merge.yml` - Merge config onto devices using NAPALM
- `resource_modules.yml` - Get and configure VLANs with resource modules
- `xx_command.yml` - (Commented out) show commands via `cli_command`/platform modules

### `facts/` - Facts
- `gather_facts.yml` - Facts via `gather_facts` and the `setup` module
- `napalm_facts.yml` - Facts, interfaces, LLDP and config via NAPALM

### `files/` - Files and directories
- `create_dir.yml` - Create a directory on Arista devices
- `create_dir_structure.yml` - Create a directory tree on Arista devices
- `create_directory.yml` - Create a dated local directory structure (and delete it)
- `create_file.yml` - Create a file on Arista devices

### `jinja/` - Templating
- `jinja_intf.yml` - Render interface config to screen and file
- `jinja_lp.yml` - Render loopback config and save to file
- `jinja_test.yml` - Print a rendered template

### `print/` - Output and variables
- `print_cmd_output.yml` - Run show commands on Cisco and Arista and print the output
- `print_cmd_string.yml` - Print a command's output as a string
- `print_facts.yml` - Print `ansible_facts` for all hosts
- `print_hostvar.yml` - Print all or specific host variables

### `show_parse_val/` - Parse and validate
- `napalm.yml` - Run NAPALM getters and CLI commands
- `napalm_val.yml` - NAPALM state validation
- `parsing.yml` - Parse show output into structured data with ntc-templates (TextFSM)
- `val_file.yml` - NAPALM validation file (expected state for WAN1)

## Other directories

- `packet_coders_lab/` - Separate Packet Coders spine-leaf lab with its own inventory and `ansible.cfg`
  - `pb_push_config.yml` - Push spine-leaf config to Arista switches from `data.yml`
  - `pb_test_ospf.yml` - Verify all OSPF neighbors are FULL
- `roles/` - `base`, `ip`, `router`, `brownfield` roles used by `cfg_from_roles.yml`
- `templates/` - Jinja2 templates for interfaces and NTP
- `configs/` - Sample device configs
- `inventory/` - Hosts, group_vars and host_vars
