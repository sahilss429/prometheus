servers=[
  {
    :hostname => "dc1",
    :ip => "192.168.10.10",
    :box => "mynewbox",
    :ram => 2048,
    :cpu => 2
  },
  {
    :hostname => "dc2",
    :ip => "192.168.10.11",
    :box => "mynewbox",
    :ram => 2048,
    :cpu => 2
  },
  {
    :hostname => "dc4",
    :ip => "192.168.10.13",
    :box => "mynewbox",
    :ram => 2048,
    :cpu => 2
  },
  {
    :hostname => "dc3",
    :ip => "192.168.10.12",
    :box => "mynewbox",
    :ram => 2048,
    :cpu => 2
  }
]
ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
Vagrant.configure("2") do |config|
    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.hostname = machine[:hostname]
            node.vm.provision 'shell', inline: "sudo apt-get update -y && sudo apt-get install -y unzip python"
            node.vm.provision 'shell', inline: 'sudo -H -u ubuntu bash -c "mkdir -p /home/ubuntu/.ssh"'
            node.vm.provision 'shell', inline: "sudo -H -u ubuntu bash -c 'echo #{ssh_pub_key} >> /home/ubuntu/.ssh/authorized_keys'", privileged: false
            node.vm.network "private_network", ip: machine[:ip]
            node.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
            end
        end
    end
end
