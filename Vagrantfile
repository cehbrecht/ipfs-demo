Vagrant.configure("2") do |config|
  config.vm.box = "rocky/9"

  # Configure VM resources
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "6144"  # 6 GB of RAM
  end

  # Set up a private network with a static IP
  config.vm.network "private_network", type: "dhcp"

  # Install necessary tools: Firewalld, Vim, Git, Byobu, IPFS, and tcpdump
  config.vm.provision "shell", inline: <<-SHELL
    # Update system
    sudo dnf update -y

    # Install Firewalld
    sudo dnf install -y firewalld

    # Enable and start firewalld service
    sudo systemctl enable firewalld
    sudo systemctl start firewalld

    # Allow IPFS swarm port (default 4001) through the firewall
    sudo firewall-cmd --zone=public --add-port=4001/tcp --permanent
    sudo firewall-cmd --zone=public --add-port=4001/udp --permanent

    # Reload firewalld to apply the changes
    sudo firewall-cmd --reload

    # Install Vim, Git, Byobu, and tcpdump
    sudo dnf install -y vim git byobu tcpdump

    # Verify installation of tools (optional)
    vim --version
    git --version
    byobu --version
    tcpdump --version

    # Install IPFS (example with a precompiled version)
    curl -o go-ipfs.tar.gz https://dist.ipfs.io/go-ipfs/v0.32.1/go-ipfs_v0.32.1_linux-amd64.tar.gz
    tar -xvzf go-ipfs.tar.gz
    cd go-ipfs
    sudo bash install.sh

    # Start IPFS daemon (optional)
    ipfs daemon &
  SHELL
end
