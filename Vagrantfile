Vagrant.configure("2") do |config|

  config.vm.define :ansible do |ansible|
    ansible.vm.box = "centos/7"
    ENV["LC_ALL"] = "en_US.UTF-8"
    config.ssh.insert_key = false
    ansible.vm.synced_folder "./ansible", "/home/vagrant/ansible", type: "rsync"
    ansible.vm.network "private_network", ip: "192.168.56.8", :netmask => "255.255.255.0"
    ansible.vm.provision :hosts, :sync_hosts => true
    ansible.vm.provision :host_shell do |host_shell|
    host_shell.inline = 'scp -i ~/.vagrant.d/insecure_private_key -o "StrictHostKeyChecking no" ~/.vagrant.d/insecure_private_key vagrant@192.168.56.8:/home/vagrant/.ssh/id_rsa'
    end
    ansible.vm.provision "shell", inline: <<-SHELL
      chmod 600 /home/vagrant/.ssh/id_rsa
      timedatectl set-timezone asia/ho_chi_minh
      yum install -y epel-release
      yum -y update
      yum -y install python36 python36-libs python36-devel
      python36 -m ensurepip
      /usr/local/bin/pip3 install --upgrade pip
      /usr/local/bin/pip3 install ansible
      SHELL
  end

  N=2
  (1..N).each do |i|
    config.vm.define "server#{i}" do |server|
      server.vm.box = "centos/7"
      config.ssh.insert_key = false
      server.vm.network "private_network", ip: "192.168.56.#{i}", :netmask => "255.255.255.0"
      server.vm.provision :hosts, :sync_hosts => true
    end
  end

end
