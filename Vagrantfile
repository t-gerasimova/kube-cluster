
hosts = [
    {
        id: "11",
        name: "master",
        mem: "2048",
        cpu: "2"
    },
    {
        id: "12",
        name: "worker1",
        mem: "2048",
        cpu: "2"
    },
    {
        id: "13",  
        name: "worker2",
        mem: "2048",
        cpu: "2"
    }
]

Vagrant.configure("2") do |config|

  config.vm.box = "hashicorp/bionic64"
  config.ssh.insert_key = false

hosts.each do |opt|
    config.vm.define opt[:name] do |node|

      node.vm.hostname = opt[:name]
      node.vm.network :private_network, ip: "192.168.56."+opt[:id]

      node.vm.provider "virtualbox" do |vb|
        vb.check_guest_additions = false
        vb.functional_vboxsf = false
        vb.name = "k8s-#{opt[:name]}"
        vb.memory = opt[:mem]
        vb.cpus = opt[:cpu]
        vb.customize ["modifyvm", vb.name, "--natnet1", "10.0.#{opt[:id]}.0/24"]
        vb.customize ["modifyvm", vb.name, "--macaddress1", "080027bb14"+opt[:id]]
      end

      node.vm.provision "shell", inline: "swapoff -a && sed -i '/ swap / s/^/#/' /etc/fstab"

    end

  end

end

