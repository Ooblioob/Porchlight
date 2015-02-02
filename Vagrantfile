# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.hostname = "unitybox.cfpb.local"

  config.vm.box = "CentOS64"
  config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/centos-64-x64-vbox4210.box"

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.auto_detect = true
    # If you are using VirtualBox, you might want to enable NFS for shared folders
    # config.cache.enable_nfs  = true
  end

  # Forward a port from the guest to the host, which allows for outside
  # computers to access the VM, whereas host only networking does not.
  config.vm.network "forwarded_port", guest: 80, host: 8080, auto_correct: true
  config.vm.network "forwarded_port", guest: 8000, host: 8000, auto_correct: true

  # Map the last argument here to the folder where you have Wordpress checked out.
  config.vm.synced_folder "./", "/var/www/porchlight", create: true, owner: "vagrant", group: "vagrant"
  
  config.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/deploy_porchlight.yml"
      # Creates proper inventory file with correct ssh port
      # The generated inventory file is located at:
      # .vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory
      ansible.groups = {
          "local" => ["default"]
      }
  end

end
