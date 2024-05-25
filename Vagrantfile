VAGRANTFILE_API_VERSION = "2"

$script = <<ENDSCRIPT
  sudo apt update -y
  sudo apt install openjdk-17-jdk wget -y
  sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
  echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
  sudo apt upgrade -y
  sudo apt-get update -y
  # Download and install init-system-helpers version 1.60 from Ubuntu 20.04
  wget http://archive.ubuntu.com/ubuntu/pool/main/i/init-system-helpers/init-system-helpers_1.60_all.deb
  sudo dpkg -i init-system-helpers_1.60_all.deb
  sudo apt-get install -y jenkins
  sudo systemctl enable jenkins
  sudo systemctl start jenkins
ENDSCRIPT

Vagrant.configure("2") do |config|
  config.vm.define "jenkinsserver" do |jenkinsserver|
    jenkinsserver.vm.box_download_insecure = true
    jenkinsserver.vm.box = "hashicorp/bionic64"
    jenkinsserver.vm.network "forwarded_port", guest: 8080, host: 8080
    jenkinsserver.vm.network "forwarded_port", guest: 8081, host: 8081
    jenkinsserver.vm.network "private_network", ip: "100.0.0.2"
    jenkinsserver.vm.hostname = "jenkinsserver"
    jenkinsserver.vm.provider "virtualbox" do |v|
      v.name = "jenkinsserver"
      v.memory = 2048
      v.cpus = 2
    end
    jenkinsserver.vm.provision "shell", inline: $script
  end
end
