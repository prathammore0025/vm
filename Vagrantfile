Vagrant.configure("2") do |config|
  # Use Ubuntu as the base box
  config.vm.box = "ubuntu/bionic64"  # Or a newer Ubuntu version if preferred

  # Configure VirtualBox provider
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"   # Assign 2GB of RAM
    vb.cpus = 2          # Assign 2 CPUs
    vb.gui = true        # Enable GUI

    # Set video memory to 128MB
    vb.customize ["modifyvm", :id, "--vram", "128"]
  end

  # Install a GUI and configure the VM for auto-login
  config.vm.provision "shell", inline: <<-SHELL
    # Update the package list
    sudo apt-get update

    # Install a lightweight desktop environment (XFCE) and additional utilities
    sudo apt-get install -y xfce4 xfce4-goodies lightdm

    # Configure LightDM as the default display manager
    sudo systemctl enable lightdm

    # Enable auto-login for the vagrant user
    sudo bash -c 'echo "[Seat:*]" >> /etc/lightdm/lightdm.conf'
    sudo bash -c 'echo "autologin-user=vagrant" >> /etc/lightdm/lightdm.conf'

    # Allow password-based login for the vagrant user
    echo "vagrant:vagrant" | sudo chpasswd  # Set the password for 'vagrant' user
    sudo sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    sudo systemctl restart sshd
  SHELL
end
