# MacOS local installation

## Hardware requirements

* Apple Silicon (M1/M2/M3/M4)
* 6 free cores
* 12 GB of free memory
* 150 GB of free disk space
* MacOS Sequoia or later

## Install

Hypeman's installer will do the following:

* Install [Homebrew](https://brew.sh) (if not already installed)
* Install git
* Create `$HOME/dev` and clone source from [https://github.com/lot49-cybernetics/hypeman](https://github.com/lot49-cybernetics/hypeman)
* Install Ansible
* Run the Ansible playbook

You can use `install.sh` for this:

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/lot49-cybernetics/hypeman/refs/heads/main/deploy/local/macos/install.sh)"
```

If you find any gaps in the installer, please [report an issue](https://github.com/lot49-cybernetics/hypeman/issues/new/choose).

## Security considerations

We strongly recommend that you review the installer script before running it: some steps require `sudo` and it modifies your developer machine. You may also want to consider setting up the development environment in a clean VM, whether cloud-hosted (e.g. in [MacStadium](https://www.macstadium.com/)) or locally using [Parallels](https://www.parallels.com/), [VMWare Fusion](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion) or [UTM](https://mac.getutm.app/).

The installer is tested with a clean MacOS Sequoia image running on a UTM ARM64 VM.

## Testing the installer

To do an end-to-end test of the installer, you will need to install UTM:

```shell
brew install --cask utm
```

This will also install the `utmctl` CLI tool. Due to [this issue](https://github.com/utmapp/UTM/issues/6982) it's not yet possible to script VM creation, but these instructions will be updated once it's resolved.

You should go through the regular MacOS setup and create a user called `inception`, then set a hostname and allow remote login:

```shell
sudo scutil --set HostName inception-vm
sudo systemsetup -setremotelogin on
```

You can test this out; it will look something like this

```shell
ssh -o "StrictHostKeyChecking accept-new" inception@inception-vm.local
(inception@inception-vm.local) Password:
Last login: Sun Aug  3 15:30:41 2025 from xxx
inception@inception-vm ~ %
```

Now install Ansible roles & collections (one-time):

```shell
ansible-galaxy install -r requirements.yml
```

and run the playbook, providing the password you set for `inception`:

```shell
ansible-playbook main.yml --ask-become-pass --ask-pass
```

The playbook will configure the test VM.
