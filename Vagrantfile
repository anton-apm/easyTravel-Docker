$script = <<-SCRIPT
apt-get update
apt-get install git docker docker-compose -y
wget --no-check-certificate -O Dynatrace-OneAgent-Linux-latest.sh "https://syb31902.sprint.dynatracelabs.com/api/v1/deployment/installer/agent/unix/default/latest?Api-Token=INSERT_API_TOKEN_HERE&arch=x86&flavor=default"
sudo /bin/sh Dynatrace-OneAgent-Linux-latest.sh APP_LOG_CONTENT_ACCESS=1 INFRA_ONLY=0
systemctl enable docker && systemctl start docker
git clone https://github.com/anton-apm/easyTravel-Docker
chown -R vagrant:vagrant easyTravel-Docker
cd easyTravel-Docker
sudo docker-compose up -d
SCRIPT

$script_everystartup = <<-SCRIPT
cd easyTravel-Docker
sudo docker-compose down
sudo docker-compose up -d
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 4
  end

  config.vm.box = "ubuntu/bionic64"
  config.vm.provision "shell", inline: $script
  config.vm.provision "shell", inline: $script_everystartup, run: 'always'
  config.vm.network "private_network", ip: "192.168.77.8"
end


