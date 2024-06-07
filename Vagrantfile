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

  end # config.vm.define

end # Vagrant.configure
