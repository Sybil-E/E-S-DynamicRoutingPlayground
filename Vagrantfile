# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.network "private_network", virtualbox__intnet: true

  [
    { hostname: "isp-bgprt01", playbook: "isp_bgprt01.yml" },
    { hostname: "own-bgprt01", playbook: "own_bgprt01.yml" },
    { hostname: "igp-rt01", playbook: "igp_rt01.yml" },
    { hostname: "testvm01", playbook: "testvm01.yml" },
    { hostname: "testvm02", playbook: "testvm02.yml" }
  ].each do |node|
    config.vm.define node[:hostname] do |item|
      item.vm.hostname = node[:hostname] 

      item.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "playbook/#{node[:playbook]}"
        ansible.inventory_path = "playbook/hosts/local"
        ansible.limit = "target"
      end

      item.vm.provision :shell, inline: "ip r del default dev eth0"
    end
  end
end
