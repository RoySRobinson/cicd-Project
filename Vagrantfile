Vagrant.configure("2") do |config|
     
   config.vm.box = "bento/centos-7.6"
   
   #$script = <<-SCRIPT
   #  sudo yum install epel-release -y
   #  sudo yum install ansible -y
   #SCRIPT

   $scriptGit = <<-SCRIPT
     sudo yum install curl -y
     sudo yum install policycoreutils-python -y
     sudo yum install openssh-server -y
     sudo systemctl enable sshd
     sudo systemctl start sshd
     sudo yum install postfix -y
     sudo systemctl enable postfix
     sudo systemctl start postfix
   SCRIPT

   $scriptJen = <<-SCRIPT
     sudo yum install java-1.8.0-openjdk-devel -y
     sudo yum install curl -y
     sudo yum localinstall package-1.2.3.rpm 
     sudo yum install git -y 
   SCRIPT

   $scriptVault = <<-SCRIPT
     sudo yum install wget -y
     sudo yum install unzip -y
     sudo yum install libcap2-bin 
   SCRIPT

   $scriptRocket = <<-SCRIPT
     sudo echo -e "[mongodb-org-3.6]\nname=MongoDB Repository\nbaseurl=https://repo.mongodb.org/yum/redhat/7/mongodb-org/3.6/x86_64/\ngpgcheck=1\nenabled=1\ngpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc" | sudo tee /etc/yum.repos.d/mongodb-org-3.6.repo
     sudo yum install -y curl && curl -sL https://rpm.nodesource.com/setup_8.x | sudo bash -
     sudo yum install -y gcc-c++ make mongodb-org nodejs
     sudo yum install -y epel-release && sudo yum install -y GraphicsMagick
     sudo npm install -g inherits n && sudo n 8.11.3 
   SCRIPT


   $scriptProd = <<-SCRIPT
   sudo yum install httpd -y
   sudo yum enable httpd -y
   SCRIPT


   $scriptPreprod = <<-SCRIPT
   sudo yum install httpd -y
   sudo yum enable httpd -y
   SCRIPT
   
   config.vm.define "gitlab" do |gitlab|
     gitlab.vm.hostname = "gitlab-server.com"
     config.vm.network "public_network", ip: "10.2.3.50"
     #gitlab.vm.network "public_network", bridge: 'en0: Wi-Fi(AirPort)'
     gitlab.vm.provider :virtualbox do |v|
       v.customize ["modifyvm", :id, "--memory", "2048"]
       v.customize ["modifyvm", :id, "--cpus", "2"]
     end
     gitlab.vm.provision "shell", inline: $scriptGit
   end

   config.vm.define "jenkins" do |jenkins|
     jenkins.vm.hostname = "jenkins-server.com"
     #jenkins.vm.network "public_network", bridge: 'eth0: Wi-Fi(AirPort)'
     config.vm.network "public_network", ip: "10.2.3.60"
     jenkins.vm.provision "shell", inline: $scriptJen
     #jenkins.vm.provision "ansible" do |ans| 
     #	ans.playbook = "jenkins.yml"
     #  ans.tags = "execute"
     #end
   end

   config.vm.define "vault" do |vault|
     vault.vm.hostname = "vault-server.com" 
     #vault.vm.network "public_network", bridge: 'eth0: Wi-Fi(AirPort)'
     config.vm.network "public_network", ip: "10.2.3.70"
     vault.vm.provision "shell", inline: $scriptVault
   end
 
   config.vm.define "rocket" do |rocket|
     rocket.vm.hostname = "rocketchat-server.com"
     #rocket.vm.network "public_network", bridge: 'eth0: Wi-Fi(AirPort)'
     config.vm.network "public_network", ip: "10.2.3.80"
     rocket.vm.provision "shell", inline: $scriptRocket
   end

   config.vm.define "production" do |production|
     production.vm.hostname = "production-server.com"
     #production.vm.network "public_network", bridge: 'eth0: Wi-Fi(AirPort)'
     config.vm.network "public_network", ip: "10.2.3.90"
     production.vm.provision "shell", inline: $scriptProd
   end

   config.vm.define "preprod" do |preprod|
     preprod.vm.hostname = "preprod-server.com"
     #preprod.vm.network "public_network", bridge: 'eth0: Wi-Fi(AirPort)'
     config.vm.network "public_network", ip: "10.2.3.100"
     preprod.vm.provision "shell", inline: $scriptPreprod
   end 


end
