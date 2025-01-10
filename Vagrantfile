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

  # Provision the VM to set up auto-login and start XFCE
  config.vm.provision "shell", inline: <<-SHELL
    # Update the package list
    sudo apt-get update

    # Install XFCE desktop environment and LightDM
    sudo apt-get install -y xfce4 xfce4-goodies lightdm lightdm-gtk-greeter

    # Set up LightDM for automatic login
    sudo mkdir -p /etc/lightdm/lightdm.conf.d
    echo "[Seat:*]
autologin-user=vagrant
autologin-user-timeout=0
user-session=xfce
greeter-session=lightdm-gtk-greeter" | sudo tee /etc/lightdm/lightdm.conf.d/50-myconfig.conf

    # Ensure the vagrant user has an XFCE session
    echo "startxfce4" | sudo tee /home/vagrant/.xsession
    sudo chown vagrant:vagrant /home/vagrant/.xsession
    sudo chmod +x /home/vagrant/.xsession

    # Set the password for the vagrant user
    echo "vagrant:vagrant" | sudo chpasswd

    # Ensure SSH is configured
    sudo sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
    sudo systemctl restart sshd
  SHELL
end
