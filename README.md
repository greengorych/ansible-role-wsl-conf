# Ansible Role: wsl-conf

Ansible role for generating or changing per-distribution WSL configuration file [`wsl.conf`](https://github.com/greengorych/wsl-configurations/tree/main/defaults/wsl.conf).

>[!TIP]
>You can use [`cloud-init`](https://github.com/greengorych/wsl-configurations/tree/main/defaults/.cloud-init) for generating or changing [`wsl.conf`](https://github.com/greengorych/wsl-configurations/tree/main/defaults/wsl.conf) for new instances.

## Requirements

- Windows Subsystem for Linux (WSL) must be installed and configured on the target system.
- Supported Linux distributions running inside WSL (e.g., Ubuntu, Debian, Fedora, AlmaLinux, etc.).
- Ansible version 2.12 or higher is recommended.

## Role Variables

Below are the variables you can use to customize `wsl.conf`.

>[!IMPORTANT]
>- You can set or override the role variables in [`vars/main.yml`](https://github.com/greengorych/ansible-role-wsl-conf/blob/main/vars/main.yml).
>- By default, all parameter values match those of a standard WSL installation. The instance will behave exactly as a typical WSL distribution with default `wsl.conf` settings.
>- By default, the `wsl_user_default` variable is set to `{{ ansible_user_id }}`, which resolves to the user running the playbook. This ensures that the default user inside WSL matches the Ansible connection user. Please make sure that `gather_facts: true` is enabled in your playbook, or manually set the `wsl_user_default` variable.

### Main Parameters

| Section   | Parameter          | Variable                         | Values                    | Default         |
|-----------|--------------------|----------------------------------|---------------------------|-----------------|
| boot      | systemd            | wsl_boot_systemd                 | true, false               | true            |
| boot      | command            | wsl_boot_command                 |                           | Not set         |
| automount | enabled            | wsl_automount_enabled            | true, false               | true            |
| automount | mountFsTab         | wsl_automount_mountfstab         | true, false               | true            |
| automount | root               | wsl_automount_root               |                           | /mnt/           |
| automount | options            | wsl_automount_options            | See table "Mount Options" | Not set         |
| network   | hostname           | wsl_network_hostname             | Any valid hostname        | Not set         |
| network   | generateResolvConf | wsl_network_generate_resolv_conf | true, false               | true            |
| network   | generateHosts      | wsl_network_generate_hosts       | true, false               | true            |
| time      | useWindowsTimezone | wsl_time_use_windows_timezone    | true, false               | true            |
| interop   | enabled            | wsl_interop_enabled              | true, false               | true            |
| interop   | appendWindowsPath  | wsl_interop_append_windows_path  | true, false               | true            |
| user      | default            | wsl_user_default                 | Existing user             | ansible_user_id |

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

## Dependencies

This role has no external dependencies.

## Example Playbook

```yaml
- name: Customizing wsl.conf
  hosts: localhost
  gather_facts: true
  roles:
    - { role: ansible-role-wsl-conf }
```

## License

MIT - free to use, modify, and share.

## Author Information

This role was created by [greengorych](https://github.com/greengorych).

## Related Links

- [Advanced settings configuration in WSL](https://learn.microsoft.com/en-us/windows/wsl/wsl-config)
- [default.user-data – Default cloud-init Configuration File](https://github.com/greengorych/wsl-configurations/tree/main/defaults/.cloud-init)