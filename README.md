# Celestia-light-node
Celestia is a modular blockchain network that separates consensus from data availability, enabling more scalable and flexible decentralized applications.

![image](https://github.com/user-attachments/assets/76c5e287-255b-4e63-86fe-232598e4d6c8)

A Celestia Light Node allows you to validate and verify data without the need to store the entire blockchain, making the process lighter and more accessible.

## Install dependencies
```console
# Update & Install Packages
sudo apt update & sudo apt upgrade -y

sudo apt install curl tar wget aria2 clang pkg-config libssl-dev jq build-essential \
git make ncdu -y
```
```console
# Install Go if needed..
ver="1.22.0"
cd $HOME
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
## Install Celestia CLI
```console
cd $HOME
rm -rf celestia-node
git clone https://github.com/celestiaorg/celestia-node.git
cd celestia-node/
```
## Set version
```console
git checkout tags/v0.14.1
```
![image](https://github.com/user-attachments/assets/1b7ef984-67fb-468f-b5dc-2e970656d604)

## Start installation
```console
make build && make install && make cel-key
```
![image](https://github.com/user-attachments/assets/6c529ffa-6e50-483a-b5c7-e300a68e749a)

## Node initialization
Create your wallet that will be linked to your Celestia Light Node and replace <name> with the name you want to give your wallet:
```console
./cel-key add <name> --keyring-backend test \
    --node.type light --p2p.network celestia
```

```console
# Initialize your node
celestia light init
```
## Starting your node
You can now start your node with your wallet. Replace <name> with the name you previously gave your wallet:
```console
sudo tee /etc/systemd/system/celestia.service > /dev/null <<EOF
[Unit]
Description=Celestia Node
After=network.target

[Service]
User=root
ExecStart=/root/celestia-node/build/celestia light start --keyring.accname <name> \
--core.ip consensus.lunaroasis.net
Restart=always
RestartSec=3
LimitNOFILE=infinity

[Install]
WantedBy=multi-user.target
EOF
```
## Start node
```console
systemctl daemon-reload
systemctl enable celestia
systemctl start celestia
```
## Check logs
```console
journalctl -fu celestia
```
## Check node status
```console
systemctl status celestia
```
