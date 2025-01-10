sudo apt-get update
sudo apt-get install -y xfce4 xfce4-goodies

startxfce4


icacls "C:\Program Files\Oracle\VirtualBox" /grant "%username%:F" /T

https://download.virtualbox.org/virtualbox/7.0.22/VirtualBox-7.0.22-165102-Win.exe

https://releases.hashicorp.com/vagrant/2.4.3/vagrant_2.4.3_windows_amd64.msi

Solution: Allow Non-Console Users to Run X Server

sudo sed -i 's/^allowed_users=console/allowed_users=anybody/' /etc/X11/Xwrapper.config
