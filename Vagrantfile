# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
:router1 => {
        :box_name => "generic/debian11",
        :net => [
                   {ip: '10.0.10.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "r1r2"},
                   {ip: '10.0.12.1', adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "r1r3"},
                   {ip: '192.168.10.1', adapter: 4, netmask: "255.255.255.0", virtualbox__intnet: "net1"}
                ],
	:config => "router.yml"
  },
:router2 => {
        :box_name => "generic/debian11",
        :net => [
                   {ip: '10.0.10.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "r1r2"},
                   {ip: '10.0.11.2', adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "r2r3"},
                   {ip: '192.168.20.1', adapter: 4, netmask: "255.255.255.0", virtualbox__intnet: "net2"}
                ],
	:config => "router.yml"
  },
:router3 => {
        :box_name => "generic/debian11",
        :net => [
                   {ip: '10.0.12.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "r1r3"},
                   {ip: '10.0.11.1', adapter: 3, netmask: "255.255.255.252", virtualbox__intnet: "r2r3"},
                   {ip: '192.168.30.1', adapter: 4, netmask: "255.255.255.0", virtualbox__intnet: "net3"}
                ],
	:config => "router.yml"
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

    config.vm.define boxname do |box|

        box.vm.box = boxconfig[:box_name]
        box.vm.host_name = boxname.to_s
	
	box.vm.provider "virtualbox" do |v|
        	# Set VM RAM size and CPU count
        	v.memory = "512"
        	v.cpus = "1"
     	 end

        boxconfig[:net].each do |ipconf|
          box.vm.network "private_network", ipconf
        end

	if boxconfig[:config].length > 0
      		box.vm.provision "ansible" do |ansible|
	          ansible.playbook = boxconfig[:config]
       		  ansible.verbose = "v"
		  #if boxname.to_s.include? "Server"
		  #  	  ansible.extra_vars = { work_ip: boxconfig[:net][0][:ip]}
		  #end
		end
	end
      end
  end
end

