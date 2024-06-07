Vagrant.configure("2") do |config|

  ###################################################################################
  config.vm.define "vault1" do |node|

    # which image to use
    node.vm.box = "opensuse/Leap-15.6.x86_64"

    # sizing of the VMs
    node.vm.provider "libvirt" do |lv|
      lv.random_hostname = false
      lv.memory = 8196
      lv.cpus = 2
    end

    # set the hostname
    node.vm.hostname = "vault1"

    # disable shared folders
    node.vm.synced_folder ".", "/vagrant", disabled: true

    node.trigger.after :destroy do |trigger|
      trigger.warn = "Removing ansible/vault_root_token--vault1"
      trigger.run = {inline: "rm -vf ansible/vault_root_token--vault1"}
    end # node.trigger.after

  end # config.vm.define

  ###################################################################################
  config.vm.define "vault2" do |node|

    # which image to use
    node.vm.box = "opensuse/Leap-15.6.x86_64"

    # sizing of the VMs
    node.vm.provider "libvirt" do |lv|
      lv.random_hostname = false
      lv.memory = 8196
      lv.cpus = 2
    end

    # set the hostname
    node.vm.hostname = "vault2"

    # disable shared folders
    node.vm.synced_folder ".", "/vagrant", disabled: true

    # Ansible
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.limit = "all"
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision

    node.trigger.after :destroy do |trigger|
      trigger.warn = "Removing ansible/vault_root_token--vault2"
      trigger.run = {inline: "rm -vf ansible/vault_root_token--vault2"}
    end # node.trigger.after

  end # config.vm.define

end # Vagrant.configure
