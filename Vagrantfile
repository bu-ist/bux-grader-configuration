# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "trusty64"
  config.vm.box_url = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.network :private_network, ip: "192.168.33.50"
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.hostname = "bux-grader.local"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder "grader", "/edx/app/grader/grader", :create => true,
                          :owner=> "vagrant", :group=>"vagrant", :mount_options => ["dmode=2775"]
  config.vm.synced_folder "venv", "/edx/app/grader/venv", :create => true,
                          :owner=> "vagrant", :group=>"vagrant", :mount_options => ["dmode=2775"]

  # Bump instance memory (pip installs started failing due to memory allocation errors with trusty)
  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
  end

  #
  # See http://docs.vagrantup.com/v2/provisioning/ansible.html
  #
  config.vm.provision :ansible do |ansible|

    ansible.playbook = "playbooks/grader_vagrant.yml"
    ansible.inventory_path = "playbooks/inventory/vagrant"

    # A little extra info during playbook runs
    ansible.verbose = "v"

    # Limit plays to specific tags
    # ansible.tags = ["foo"]

    # Consider using `ansible vault` to encrypt sensitive config vars files
    # ansible.raw_arguments = ["--ask-vault-pass"]

    # Dry run
    # ansible.raw_arguments = ["--check"]

  end

end
