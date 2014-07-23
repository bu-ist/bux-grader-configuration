# BUx Grader Configuration

Bootstrapping of BUx external grader environments for use with the [BUx Grader Framework](https://github.com/bu-ist/bux-grader-framework).

Provisioning is accomplished using [Ansible](http://www.ansible.com/home).

In addition a Vagrantfile is provided for quick setup of local development VMs with [Vagrant](http://www.vagrantup.com/).

## Prerequisites

In order for the grader to start it must be able to succesfully connect to a running XQueue instance. If your course will be hosted on edx.org, contact your program manager to set up your course queue and obtain credentials. See the [course author docs on external graders](http://edx.readthedocs.org/projects/edx-partner-course-staff/en/latest/exercises_tools/external_graders.html) for more details.

For development purposes you might consider standing up your own edX platform using the [edX configuration repository](https://github.com/edx/configuration). See the [Wiki page for installation options](https://github.com/edx/configuration/wiki#installation-options).

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

The ``grader`` role will look for a top-level ``requirements.txt`` file in your course repository, which it will use to populate a ``virtualenv`` if found.

The ``grader_settings_module`` variable should refer to a settings module that is importable by Python from your repository root.

For example, if your settings module is located in ``settings/production.py``, the ``grader_settings_module`` should be set to ``settings.production``.

## Setting up a development VM

### Prerequisites
1. VirtualBox (https://www.virtualbox.org/)
2. Vagrant (http://www.vagrantup.com)
3. vagrant-vbguest plugin (https://github.com/dotless-de/vagrant-vbguest) (Optional)
4. vagrant-hostupdater plugin (https://github.com/cogitatio/vagrant-hostsupdater) (Optional)

### Setup
1. Add your grader-specific configuration vars to `playbooks/group_vars/vagrant`
2. Run `vagrant up` to install and boot the VM and start provisioning with Ansible

The first run will take a while -- consider grabbing a coffee or calling your grandmother.

Once it's completed, the grader should be up and polling xqueue inside the VM.

### Shared Folders

The VM will mount two shared folders for your development convenience:

* grader - /edx/app/grader/grader
* venv - /edx/app/grader/venv

## Provisioning a Remote Server with Ansible

1. Add an inventory file to `playbooks/inventory` describing the remote system to be provisioned
2. Add your grader-specific configuration (see `playbooks/group_vars/all` for examples)
3. Provision with `ansible-playbook -i inventory/file grader.yml`

## Managing the Grader

```bash
# Check grader status
$ /edx/bin/supervisorctl status

# Start / stop grader
$ /edx/bin/supervisorctl start|stop bux-grader
```
