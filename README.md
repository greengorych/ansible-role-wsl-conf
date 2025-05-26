# Ansible Role: wsl-conf

Ansible role to manage per-distribution WSL 1 and WSL 2 configuration file [`wsl.conf`](https://github.com/greengorych/wsl-configurations/tree/main/defaults/wsl.conf).

> [!TIP]
> You can use [`cloud-init`](https://github.com/greengorych/wsl-configurations/tree/main/defaults/.cloud-init) for generating `wsl.conf` for new WSL 2 virtual machines with cloud-init support.

By default, this role generates and manages a `wsl.conf` file that includes:
- Variable descriptions
- Variable dependencies
- Default values
- Possible values
- Variables with default or overridden values

## Requirements

- Windows Subsystem for Linux (WSL) must already be installed and configured on the target system.
- Supported Linux distributions running inside WSL (e.g., Ubuntu, Debian, Fedora, AlmaLinux, etc.).
- Ansible version 2.12 or higher is recommended.

## Role Variables

The following variables allow customization of the `wsl.conf` file and its management by this role.

>[!IMPORTANT]
> - You can set or override the default role variables in your playbook or in [`vars/main.yml`](https://github.com/greengorych/ansible-role-wsl-conf/blob/main/vars/main.yml).
> - By default, all variable values match those of a standard WSL installation. The virtual machine will behave exactly as a typical WSL distribution with default `wsl.conf` settings.
> - By default, `wsl_user_default` is set to `{{ ansible_user_id }}`, which matches the user running the playbook. This requires `gather_facts: true` to populate `ansible_user_id`. If facts are not gathered, set `wsl_user_default` manually.

### Mapping Role Variables to wsl.conf Settings

The table below maps role variables to their corresponding [section] and key in the resulting `wsl.conf` file:

| Role variable                         | Configuration section | Configuration variable |
| ------------------------------------- |-----------------------|------------------------|
| wsl_conf_boot_systemd                 | [boot]                | systemd                |
| wsl_conf_boot_protect_binfmt          | [boot]                | protectBinfmt          |
| wsl_conf_boot_command                 | [boot]                | command                |
| wsl_conf_automount_enabled            | [automount]           | enabled                |
| wsl_conf_automount_mount_fstab        | [automount]           | mountFsTab             |
| wsl_conf_automount_root               | [automount]           | root                   |
| wsl_conf_automount_options            | [automount]           | options                |
| wsl_conf_network_hostname             | [network]             | hostname               |
| wsl_conf_network_generate_resolv_conf | [network]             | generateResolvConf     |
| wsl_conf_network_generate_hosts       | [network]             | generateHosts          |
| wsl_conf_gpu_enabled                  | [gpu]                 | enabled                |
| wsl_conf_time_use_windows_timezone    | [time]                | useWindowsTimezone     |
| wsl_conf_interop_enabled              | [interop]             | enabled                |
| wsl_conf_interop_append_windows_path  | [interop]             | appendWindowsPath      |
| wsl_conf_user_default                 | [user]                | default                |

## Configuration Reference

### [boot] section

| Variable      | Values      | Default | Description                      |
| ------------- | ----------- | ------- | -------------------------------- |
| systemd       | true, false | true    | Enables systemd support          |
| protectBinfmt | true, false | true    | Blocks systemd unit generation   |
| command       | string      | Not set | Command to run at boot (as root) |

### [automount] section

| Variable   | Values               | Default | Description                                          |
| ---------- | -------------------- | ------- | ---------------------------------------------------- |
| enabled    | true, false          | true    | Auto-mount Windows drives                            |
| mountFsTab | true, false          | true    | Auto-mount /etc/fstab entries                        |
| root       | path (string)        | /mnt/   | Mount point for Windows drives                       |
| options    | comma-separated list | Not set | Additional mount options (see table "Mount Options") |

### Mount Options

| Option     | Description                                                    | Default  |
|------------|----------------------------------------------------------------|----------|
| metadata   | Enable Linux file permissions on mounted drives                | Disabled |
| uid=UID    | User ID used as owner of all files                             | 1000     |
| gid=GID    | Group ID used as owner of all files                            | 1000     |
| umask=MODE | Octal permission mask for files and directories                | 022      |
| fmask=MODE | Octal permission mask for files only                           | 000      |
| dmask=MODE | Octal permission mask for directories only                     | 000      |
| case=MODE  | Case sensitivity behavior (see table "Case Sensitivity Modes") | off      |

### Case Sensitivity Modes

| Modes  | Description                                       |
|--------|---------------------------------------------------|
| off    | Case insensitivity (default)                      |
| dir    | Enables case sensitivity for specific directories |
| force  | Forces all new directories to be case-sensitive   |

### [network] section

| Variable           | Values      | Default          | Description                    |
| ------------------ | ----------- | ---------------- | ------------------------------ |
| hostname           | string      | Windows hostname | Custom hostname                |
| generateResolvConf | true, false | true             | Auto-generate /etc/resolv.conf |
| generateHosts      | true, false | true             | Auto-generate /etc/hosts       |

### [gpu] section

| Variable | Values      | Default | Description                        |
| -------- | ----------- | ------- | ---------------------------------- |
| enabled  | true, false | true    | Enable Linux access to Windows GPU |

### [time] section

| Variable           | Values      | Default | Description                      |
| ------------------ | ----------- | ------- | -------------------------------- |
| useWindowsTimezone | true, false | true    | Sync Linux timezone with Windows |

### [interop] section

| Variable          | Values      | Default | Description                       |
| ----------------- | ----------- | ------- | --------------------------------- |
| enabled           | true, false | true    | Enable Windows/Linux interop      |
| appendWindowsPath | true, false | true    | Append Windows PATH to Linux PATH |

### [user] section

| Variable | Values   | Default            | Description        |
| -------- | -------- | ------------------ | ------------------ |
| default  | username | First created user | Default Linux user |


## Dependencies

This role has no external Ansible dependencies.

## Example Playbook

```yaml
- name: Managing wsl.conf
  hosts: localhost
  gather_facts: true
  roles:
    - ansible-role-wsl-conf
```

## License

MIT - free to use, modify, and share.

## Author Information

This role was created by [greengorych](https://github.com/greengorych).

## Related Links

- [Advanced settings configuration in WSL](https://learn.microsoft.com/en-us/windows/wsl/wsl-config)
- [default.user-data – Default cloud-init Configuration File](https://github.com/greengorych/wsl-configurations/tree/main/defaults/.cloud-init)