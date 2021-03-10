# jtprogru's Vagrant Box Packer Builds

![Validate Packer JSON files](https://github.com/jtprogru/vag-boxes/workflows/Validate%20Packer%20JSON%20files/badge.svg)

This project contains the Packer build configurations for all of Jeff Geerling's (jtprogru's) Vagrant Boxes. Each box builds a minimal base box for use with VirtualBox. Available boxes include:

  - [jtprogru/ubuntu2004](https://app.vagrantup.com/jtprogru/boxes/ubuntu2004) - [`ubuntu2004` directory](ubuntu2004/)
  - [jtprogru/centos7](https://app.vagrantup.com/jtprogru/boxes/centos7) - [`centos7` directory](centos7/)

All of these boxes are available as public, free Vagrant boxes and can be used with the command:
```bash
vagrant init jtprogru/[box name here]
```


## Requirements

The following software must be installed/present on your local machine before you can use Packer to build any of these Vagrant boxes:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/)
  - [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

## Usage

Make sure all the required software (listed above) is installed, then cd into one of the box directories and run:
```bash
$ packer build -var 'version=1.2.0' box-config.json
```
After a few minutes, Packer should tell you the box was generated successfully, and the box was uploaded to Vagrant Cloud.

> **Note**: This configuration includes a post-processor that pushes the built box to Vagrant Cloud (which requires a `VAGRANT_CLOUD_TOKEN` environment variable to be set); remove the `vagrant-cloud` post-processor from the Packer template to build the box locally and not push it to Vagrant Cloud. You don't need to specify a `version` variable either, if not using the `vagrant-cloud` post-processor.

### Building _all_ the boxes

Whenever VirtualBox is updated, it's best to re-build _all_ the base boxes so they have the latest guest additions.

Assuming you have Ansible and Packer installed already, and you have a `VAGRANT_CLOUD_TOKEN` available in your environment, you can run the playbook to build and push updated versions for all the boxes:
```bash
ansible-playbook build-boxes.yml
```
You can also build and push just one box:
```bash
ansible-playbook build-boxes.yml -e "{'boxes':['centos7']}"
```
## Testing built boxes

There's an included Vagrantfile that allows quick testing of the built Vagrant boxes. From the same box directory, run the following command after building the box:
```bash
$ vagrant up
```
Test that the box works correctly, then tear it down with:
```bash
$ vagrant destroy -f
```
## License

[WTFPL](LICENSE.md)

## Author

These configurations are maintained by [Michael Savin](https://jtprog.ru).
