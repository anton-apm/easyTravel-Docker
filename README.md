## This project is aimed at quick deployment of Easytravel and Dynatrace for demo purposes

## IMPORTANT:
- You need to add the link to the Vagrantfile to download the oneagent. You can find it inside your Dynatrace environment under Deploy Dynatrace. Relevant doc:
 https://www.dynatrace.com/support/help/technology-support/operating-systems/linux/installation/install-oneagent-on-linux/

## Other prerequisites:
- Virtualbox 
- Vargant
- Git

## After doing the step above this is what you need to do to run Easytravel:
- Start Git bash or similar console
- git clone https://github.com/anton-apm/easyTravel-Docker/
## IMPORTANT! You need to edit the Vagrantfile and insert your OneAgent download link/token
- cd easytravel-Docker
- vagrant up

## This is a fork of https://github.com/Dynatrace/easyTravel-Docker with the following changes implemeted:

1. .env files was changed to contain 'ET_APM_SERVER_DEFAULT=APM'. This is required to monitor Easytravel with Dynatrace vs older  monitoring solution Appmon.

2. Vagrantfile was added to allow instant deployment of a virtual machine with Dynatrace Oneagent, Docker, Docker-compose, Git and Easytravel. 

3. Vagrantfile was set up to destroy and recreate containers on each restart to avoid the issue with MongoDB.



Here are the contents of the Vagrantfile:
```
$script = <<-SCRIPT
apt-get update
apt-get install git docker docker-compose -y
wget  -O Dynatrace-OneAgent-latest.sh "INSERT_YOUR_ONEAGENT_LINK_HERE https://www.dynatrace.com/support/help/technology-support/operating-systems/linux/installation/install-oneagent-on-linux/"
sudo /bin/sh Dynatrace-OneAgent-latest.sh APP_LOG_CONTENT_ACCESS=1 INFRA_ONLY=0
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
  config.vm.network "private_network", ip: "192.168.7.8"
end
```
