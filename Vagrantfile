# Vagrantfile for Rocky Linux 9 with IPFS v0.32.1, 6GB Memory, and tcpdump
Vagrant.configure("2") do |config|
  # Specify the box to use
  config.vm.box = "generic/rocky9"

  # Set up VM properties
  config.vm.hostname = "rocky9-ipfs"
  config.vm.network "private_network", ip: "192.168.33.10" # Assign static IP

  # Port forwarding for IPFS (optional)
  config.vm.network "forwarded_port", guest: 5001, host: 5001 # IPFS API port
  config.vm.network "forwarded_port", guest: 4001, host: 4001 # IPFS swarm port
  config.vm.network "forwarded_port", guest: 8080, host: 8080 # IPFS gateway port

  # Provisioning script for IPFS and tcpdump installation
  config.vm.provision "shell", inline: <<-SHELL
    # Update system and install required packages
    sudo dnf update -y
    sudo dnf install -y wget tar tcpdump byobu git vim

    # Download and install IPFS
    IPFS_VERSION="v0.32.1"
    wget https://dist.ipfs.tech/kubo/${IPFS_VERSION}/kubo_${IPFS_VERSION}_linux-amd64.tar.gz -O /tmp/kubo.tar.gz
    tar -xvzf /tmp/kubo.tar.gz -C /tmp
    sudo mv /tmp/kubo/ipfs /usr/local/bin/ipfs

    # Initialize IPFS node
    ipfs init

    # Start IPFS daemon in background (systemd setup optional)
    nohup ipfs daemon &

    # Verify installations
    ipfs --version
    tcpdump --version
  SHELL

  # Resources: Increased memory to 6GB
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "6144" # Set VM memory to 6GB
    vb.cpus = 2        # Set CPU cores
  end
end
