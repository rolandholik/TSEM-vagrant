# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "deCervantes"
  config.vm.box_check_update = false
  config.vm.provider "virtualbox"
  config.vm.provider "virtualbox" do |vb|
    vb.name = "TSEM-6.10_vagrant"
    vb.memory = "4096"
    max_cpus = `nproc`.to_i - 2  # make vagrant use most of the system cpus (for kernel compilation)
    vb.cpus = max_cpus > 0 ? max_cpus : 1  # ensure at least 1 CPU is assigned
  end

   config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update -y && sudo apt-get upgrade -y
    sudo apt-get install -y libncurses5-dev gcc make git exuberant-ctags bc libssl-dev git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison libcap-dev libxen-dev pkg-config elfutils
    cd ~
    git clone --depth=1 --branch TSEM-6.10 https://github.com/rolandholik/TSEM.git
    git clone --recurse-submodules https://github.com/rolandholik/Quixote.git
    cd Quixote
    sed -i 's#url = git@github.com:Quixote-Project/HurdLib.git#url = https://github.com/Quixote-Project/HurdLib.git#g' .git/config
    git pull --recurse-submodules
    
    cd ~/TSEM
    git switch TSEM-6.10
    sudo make -j$(nproc)
    sudo make modules_install
    sudo make install
    
    cd ~/Quixote
    make
    sudo make install
    echo "export PATH=\"/opt/Quixote/sbin:/opt/Quixote/bin:$PATH\"" | sudo tee -a /etc/environment

    sudo sed -i 's/^GRUB_HIDDEN_TIMEOUT=0/GRUB_HIDDEN_TIMEOUT=0/' /etc/default/grub
    sudo sed -i 's/^GRUB_TIMEOUT=0/GRUB_TIMEOUT=10/' /etc/default/grub
    sudo sed -i 's/^GRUB_TIMEOUT_STYLE=hidden/#GRUB_TIMEOUT_STYLE=menu/' /etc/default/grub
    sudo sed -i 's/^GRUB_CMDLINE_LINUX_DEFAULT=".*"/GRUB_CMDLINE_LINUX_DEFAULT=""/' /etc/default/grub
    sudo sed -i 's/^GRUB_CMDLINE_LINUX=".*"/GRUB_CMDLINE_LINUX="tsem_mode=no_root_modeling"/' /etc/default/grub

    sudo update-grub
   SHELL

 # post-provisioning hook: Run `vagrant reload` after provisioning completes
  config.vm.post_up_message = "Provisioning complete. Rebooting VM..."
 
  config.vm.provision "shell", inline: <<-SHELL
    sudo shutdown -r now
  SHELL
end
