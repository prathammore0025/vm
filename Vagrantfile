Vagrant.configure("2") do |config|
  # Use Ubuntu as the base box
  config.vm.box = "ubuntu/bionic64"

  # Configure VirtualBox provider
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048" # Assign 2GB of RAM
    vb.cpus = 2        # Assign 2 CPUs
    vb.gui = true      # Enable GUI

    # Set video memory to 128MB
    vb.customize ["modifyvm", :id, "--vram", "128"]
  end

  # Provision the VM to set up the GUI and auto-login
  config.vm.provision "shell", inline: <<-SHELL
    # Update the package list
    sudo apt-get update

    # Install XFCE desktop environment and LightDM
    sudo apt-get install -y xfce4 xfce4-goodies lightdm

    # Configure LightDM as the default display manager
    sudo systemctl enable lightdm

    # Enable auto-login for the vagrant user
    sudo bash -c 'echo "[Seat:*]" >> /etc/lightdm/lightdm.conf'
    sudo bash -c 'echo "autologin-user=vagrant" >> /etc/lightdm/lightdm.conf'

    # Ensure password-based SSH login is enabled
    echo "vagrant:vagrant" | sudo chpasswd  # Set the password for 'vagrant' user
    sudo sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    sudo systemctl restart sshd

    # Ensure VirtualBox Guest Additions dependencies are installed
    sudo apt-get install -y build-essential dkms linux-headers-$(uname -r)

    # Install VirtualBox Guest Additions
    wget https://download.virtualbox.org/virtualbox/7.0.22/VBoxGuestAdditions_7.0.22.iso -O /tmp/VBoxGuestAdditions.iso
    sudo mkdir -p /mnt/vbox
    sudo mount /tmp/VBoxGuestAdditions.iso /mnt/vbox
    sudo /mnt/vbox/VBoxLinuxAdditions.run || true
    sudo umount /mnt/vbox
    rm -rf /tmp/VBoxGuestAdditions.iso /mnt/vbox
  SHELL

  # Configure synced folders (optional)
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
end
