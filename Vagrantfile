Vagrant.configure("2") do |config|

  config.vm.provider "virtualbox" do |vb|
      vb.memory = "1536"
  end

  config.vm.define "server" do |server|
    server.vm.box = "ubuntu/jammy64"
    server.vm.hostname = "server"
    server.vm.network "forwarded_port", guest: 6443, host: 6443
    server.vm.network :private_network, ip: '172.20.20.10',  virtualbox__intnet: true
    server.vm.provision "shell", 
      inline: "curl -sfL https://get.k3s.io | sh -s - server --write-kubeconfig /vagrant/config  --write-kubeconfig-mode 666 --token=SECRET --tls-san 172.20.20.10 --flannel-iface enp0s8 --flannel-backend=wireguard-native --kube-apiserver-arg advertise-address=172.20.20.10 --flannel-external-ip --node-external-ip=172.20.20.10 --node-ip=172.20.20.10"
  end

  config.vm.define "agent1" do |agent|
    agent.vm.box = "ubuntu/jammy64"
    agent.vm.hostname = "agent1"
    agent.vm.network :private_network, ip: '172.20.20.11',  virtualbox__intnet: true
    agent.vm.provision "shell", 
      inline: "curl -sfL https://get.k3s.io | K3S_URL=https://172.20.20.10:6443 K3S_TOKEN=SECRET sh -s -  --node-ip=172.20.20.11 --node-external-ip=172.20.20.11"
  end

  config.vm.define "agent2" do |agent|
    agent.vm.box = "ubuntu/jammy64"
    agent.vm.hostname = "agent2"
    agent.vm.network :private_network, ip: '172.20.20.12',  virtualbox__intnet: true
    agent.vm.provision "shell", 
      inline: "curl -sfL https://get.k3s.io | K3S_URL=https://172.20.20.10:6443 K3S_TOKEN=SECRET sh -s -  --node-ip=172.20.20.12 --node-external-ip=172.20.20.12"
  end

end
