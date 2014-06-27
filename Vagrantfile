# -*- mode: ruby -*-
# vi: set ft=ruby :

# This script will install hrpsys-base from ppa
$script = <<SCRIPT
apt-get update
apt-get install -y python-software-properties
add-apt-repository ppa:cassou/emacs
add-apt-repository ppa:openrave/release
add-apt-repository ppa:hrg/stable
echo "deb http://packages.ros.org/ros/ubuntu precise main" > /etc/apt/sources.list.d/ros-latest.list
wget http://packages.ros.org/ros.key -O - | apt-key add -
apt-get update
apt-get install -y xorg fluxbox hrpsys-base python-pip virtualbox-guest-x11 git pkg-config g++ libomniorb4-dev uuid-dev libopencv-dev python-dev libyaml-dev supervisor
pip install watchdog
cd /tmp
git clone https://github.com/gbiggs/rtctree.git
cd rtctree
python setup.py install
cd /tmp
git clone https://github.com/gbiggs/rtshell.git
cd rtshell
git checkout 626376622e75ebcd57113741596715b124a7aea1
echo "n" | python setup.py install
ln -s /usr/local/share/rtshell/shell_support /etc/profile.d/rtshell.sh
echo "xhost +" > /etc/X11/Xsession.d/99localhost
echo "if [ -z `pgrep xinit` ]
then
  sudo startx 2> /dev/null &
fi
export DISPLAY=:0.0" > /etc/profile.d/display.sh
echo "[program:openhrp-model-loader]
command=/usr/bin/openhrp-model-loader -ORBInitRef NameService=corbaloc:iiop:localhost:2809/NameService -ORBgiopMaxMsgSize 20971520
user=vagrant
autostart=true
autorestart=true
" > /etc/supervisor/conf.d/openhrp-model-loader.conf
supervisorctl update
startx 2> /dev/null &
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.hostname = "rtm"
  config.vm.box = "precise32"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"
  #config.vm.network :private_network, ip: "192.168.50.4"
  #config.vm.network :public_network

  config.vm.provider :virtualbox do |vb|
    vb.gui = true
    
    vb.customize ["modifyvm", :id, "--memory", "2048"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
    #vb.customize ["modifyvm", :id, "--graphicscontroller", "vboxvga"]
    vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
  end
  
  config.vm.provision "shell", inline: $script
end
