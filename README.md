# PackerBuilds
Packer build files for automated operating system template builds in a Proxmox environment, using the newer .HCL format.

## Requirements
- Packer installed
- Proxmox API token created
- Proxmox Packer plugin installed `packer plugins install github.com/hashicorp/proxmox`
- if using local ISO, download ISO from [here](https://ubuntu.com/download/server) to Proxmox
- Debian package `whois` installed

## Instructions

Replace the variables in the example code with values from your own environment, including Proxmox address, SSO user, etc.

- Save a copy of variables.pkr.hcl.example as variables.pkr.hcl, edit as necessary with your own information
- Save a copy of http/user-data.example as user-data, and edit as necessary with your own personalization ( I like to add ssh keys )
- to update password in http/user-data, run command `mkpasswd --method=SHA-512 --rounds=4096` type your password and copy/paste the output to user-data line 26 (see below for step by step)
- cd into proxmox-ubuntu2404 (or wherever you stored it)
- run command `packer init .`
- run command `packer build .`

## Options

`ubuntu-24.04.pkr.hcl`

### local ISO or download ISO (as of 8/26/2024)

If using Option 2, please update the checksum with this command `curl https://releases.ubuntu.com/22.04/ubuntu-22.04-live-server-amd64.iso | sha256sum` in ubuntu-24.04.pkr.hcl line 38, or else it will not work


```yaml
    # VM OS Settings
    # (Option 1) Local ISO File
    iso_file = "local:iso/ubuntu-24.04-live-server-amd64.iso"
    # - or -
    # (Option 2) Download ISO
    # iso_url = "https://releases.ubuntu.com/22.04/ubuntu-22.04-live-server-amd64.iso"
    # iso_checksum = "84aeaf7823c8c61baa0ae862d0a06b03409394800000b3235854a6b38eb4856f"
    # command to update checksum is curl https://releases.ubuntu.com/22.04/ubuntu-22.04-live-server-amd64.iso | sha256sum
```

`variables.pkr.hcl` 

### Minimum variable updates


```yaml
variable "proxmox_api_url" {
  type = string
  default = "https://192.168.1.100:8006/api2/json"
}

variable "proxmox_api_token_id" {
  type = string
  default = "root@pve!name"
}

variable "proxmox_api_token_secret" {
  type = string
  default = "pve token"
  sensitive = true
}
```

## To use custom user in Packer template:

1. Update variables.pkr.hcl with correct user info into `ubuntu_user` and `ubuntu_user_secret` variables, plain text
2. run command `mkpasswd --method=SHA-512 --rounds=4096` to get hash, paste into user-data, line 26, replacing hashed password info
3. make sure to update the user info in user-data as well
4. if using ssh key instead of password, look ast line 95 in ubuntu-24.04.pkr.hcl and follow commments


## References
- https://developer.hashicorp.com/packer/integrations/hashicorp/proxmox