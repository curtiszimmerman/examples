# Getting Started with Fli using Docker Toolbox.

## Pre-requisites

Install Docker Toolbox 
 - https://www.docker.com/products/docker-toolbox

## Start Fli

Run the install

```
git clone https://github.com/ClusterHQ/examples
cd examples/fli-docker-toolbox
sh startfli.sh
```

## Configure Fli

```
docker-machine ssh fli-vm
source ~/.bashrc
fli setup -z chq
sudo vi /root/token.txt
fli config -t /root/token.txt
```

## Use Fli

```
fli init MyVolumeSet
fli create MyVolumeSet MyVolume
fli sync MyVolumeSet
fli fetch <EXISTING-VOLUMESET>
```

## Cleanup

```
sh cleanup.sh
Docker Machine Ready...
About to remove fli-vm
Are you sure? (y/n): y
```