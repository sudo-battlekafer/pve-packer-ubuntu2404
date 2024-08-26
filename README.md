# PackerBuilds
Packer build files for automated operating system template builds in a Proxmox environment, using both the older .JSON code as well as the newer .HCL format.

## Requirements
- Packer installed
- Proxmox API token created
- Proxmox Packer plugin installed `packer plugins install github.com/hashicorp/proxmox`
- if using local ISO, download ISO from [here](https://ubuntu.com/download/server)

## Instructions

In each example build, replace the variables in the example code with values from your own environment, including vCenter Server address, SSO user, etc.

- Save a copy of variables.pkr.hcl.example as variables.pkr.hcl, edit as necessary with your own information
- Save a copy of http/user-data.example as user-data, and edit as necessary with your own personalization ( I like to add ssh keys )
- cd into proxmox-ubuntu2404 (or wherever you stored it)
- run command `packer init .`
- run command `packer build .`