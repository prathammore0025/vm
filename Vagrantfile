Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64" # Specify the box you want to use

  # Provisioning script to install/upgrade Guest Additions
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update
    sudo apt-get install -y wget build-essential dkms linux-headers-$(uname -r)
    
    # Remove old Guest Additions if present
    sudo apt-get remove -y virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11 || true

    # Download and install Guest Additions 7.0.22
    wget https://download.virtualbox.org/virtualbox/7.0.22/VBoxGuestAdditions_7.0.22.iso -O /tmp/VBoxGuestAdditions.iso
    sudo mkdir -p /mnt/vbox
    sudo mount /tmp/VBoxGuestAdditions.iso /mnt/vbox
    sudo /mnt/vbox/VBoxLinuxAdditions.run || true
    sudo umount /mnt/vbox
    rm -rf /tmp/VBoxGuestAdditions.iso /mnt/vbox
  SHELL

  # Ensure synced folders are properly configured
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
end
