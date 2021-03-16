## This project is aimed at quick deployment of Easytravel and Dynatrace for demo purposes

## IMPORTANT:
- You need to add the link to the Vagrantfile to download the oneagent. You can find it inside your Dynatrace environment under Deploy Dynatrace. Relevant doc:
 https://www.dynatrace.com/support/help/technology-support/operating-systems/linux/installation/install-oneagent-on-linux/

## Other prerequisites:
- Virtualbox 
- Vargant
- Git

## After doing the step above this is what you need to do to run Easytravel:
- Start Git bash or similar console and type:
```
git clone https://github.com/anton-apm/easyTravel-Docker/
```
## IMPORTANT! You need to edit the Vagrantfile and insert your OneAgent download link/token
```
cd easytravel-Docker
vagrant up
```
## You should see your Easytravel inside your Dynatrace env after ~5-10 minutes

## This is a fork of https://github.com/Dynatrace/easyTravel-Docker with the following changes implemeted:

1. .env files was changed to contain 'ET_APM_SERVER_DEFAULT=APM'. This is required to monitor Easytravel with Dynatrace vs older  monitoring solution Appmon.

2. Vagrantfile was added to allow instant deployment of a virtual machine with Dynatrace Oneagent, Docker, Docker-compose, Git and Easytravel. 

3. Vagrantfile was set up to destroy and recreate containers on each restart to avoid the issue with MongoDB.



