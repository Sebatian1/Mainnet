# Pactus
### Update system and install build tools
```
sudo apt update && sudo apt list --upgradable && sudo apt upgrade -y
```
### Additional package
```
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential git make ncdu net-tools -y
```
### Install go
```
sudo rm -rf /usr/local/go
curl -Ls https://go.dev/dl/go1.21.9.linux-amd64.tar.gz | sudo tar -xzf - -C /usr/local
eval $(echo 'export PATH=$PATH:/usr/local/go/bin' | sudo tee /etc/profile.d/golang.sh)
eval $(echo 'export PATH=$PATH:$HOME/go/bin' | tee -a $HOME/.profile)
```
```
go version
```
### Install binary & build binary
```
cd $Home
git clone https://github.com/pactus-project/pactus.git .pactus
cd .pactus
make build
cd build
```
### Add wallet
_Creat new wallet_
```
./pactus-daemon init
```
_Recover old wallet_
```
./pactus-daemon init --restore "Your seed phrase"
```
### Create & start service
_(Replace yourpass)_
```
sudo tee /etc/systemd/system/pactusd.service > /dev/null << EOF
[Unit]
Description=pactus Node
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=root
ExecStart= /root/.pactus/build/pactus-daemon start --password "Yourpass"
Restart=always
RestartSec=120
[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable pactusd
```
```
sudo systemctl restart pactusd && journalctl -f -u pactusd
```
