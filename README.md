
# edX External Grader Configuration

Bootstrapping of external "pull" graders for your [edX](https://www.edx.org/) course.

Provisioning is accomplished using [Ansible](http://www.ansible.com/home).

In addition a Vagrantfile is provided for quick setup of local development VMs with [Vagrant](http://www.vagrantup.com/).

## Getting Started

Clone this repository to your build machine and install the requirements.
Use of `virtualenv` is highly recommended.

```bash
$ git clone git@github.com:bu-ist/bux-grader-configuration
$ cd bux-grader-configuration
$ pip install -r requirements.txt
```

## Configuration

The playbook uses roles which depend on the existence of a few variables.

```yml
grader_repos: https://github.com/your-grader-repository
grader_version: develop
grader_settings_module: settings
```

The ``grader`` role assumes there is a top-level ``requirements.txt`` file in your course repository, which it uses to create a ``virtualenv``.

The ``grader_settings_module`` variable should refer to a settings module that is importable by Python from your repository root.

For example, if your settings module is located in ``settings/production.py``, the ``grader_settings_module`` should be set to ``settings.production``.

## Setting up a development VM

### Prerequisites
1. VirtualBox (https://www.virtualbox.org/)
2. Vagrant (http://www.vagrantup.com)

### Setup
1. Add your grader-specific configuration vars to `playbooks/group_vars/vagrant`
2. Run `vagrant up` to install and boot the VM and start provisioning with Ansible

The first run will take a while -- consider grabbing a coffee or calling your grandmother.

Once it's completed, the grader should be up and polling xqueue inside the VM.

### Shared Folders

The VM will mount two shared folders for your development convenienence:

grader - /edx/app/grader/grader
venv - /edx/app/grader/venv

## Provisioning a Remote Server with Ansible

1. Add an inventory file to `playbooks/inventory` describing the remote system to be provisioned
2. Add your grader-specific configuration (see `playbooks/group_vars/all` for examples)
3. Provision with `ansible-playbook -i inventory/file site.yml

## Managing the Grader

```bash
# Check grader status
$ /edx/bin/supervisorctl status

# Start / stop grader
$ /edx/bin/supervisorctl start|stop bux-grader
```
