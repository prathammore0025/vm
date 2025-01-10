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
    sudo apt-get install -y xfce4 xfce4-goodies

    # Ensure password-based SSH login is enabled
    echo "vagrant:vagrant" | sudo chpasswd  # Set the password for 'vagrant' user
    sudo sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    sudo systemctl restart sshd

  SHELL
end
