# Arichains-Guide
`apt update && apt upgrade -y`
`apt install curl iptables build-essential git wget jq make gcc nano tmux htop nvme-cli pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev -y`

GO Kurulumu

`ver="1.21.3" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version`

İkili dosyaların yüklenmesi

`mkdir -p $HOME/go/bin`
`wget https://github.com/airchains-network/junction/releases/download/v0.1.0/junctiond`
`chmod +x junctiond`
`mv junctiond $HOME/go/bin/`
`junctiond version --long | grep -e version -e commit`

`junctiond init YOUR NAME --chain-id junction`

Genesis'i İndirin

`wget -O $HOME/.junction/config/genesis.json "https://raw.githubusercontent.com/111STAVR111/props/main/Airchains/genesis.json"`

Addr kitabını indirin

`wget -O $HOME/.junction/config/addrbook.json "https://share102.utsa.tech/warden/addrbook.json"`,

Servis dosyası oluşturma

`tee /etc/systemd/system/junctiond.service > /dev/null <<EOF
[Unit]
Description=junctiond
After=network-online.target
[Service]
User=$USER
ExecStart=$(which junctiond) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF`

`systemctl daemon-reload`
`systemctl enable junctiond`
`systemctl restart junctiond && journalctl -u junctiond -f -o cat`

`junctiond keys add <name_wallet>`

`junctiond comet show-validator`

`nano $HOME/.junction/validator.json`

`{
  "pubkey": {#pubkey},
  "amount": "1000000amf",
  "moniker": "moniker",
  "identity": "",
  "website": "",
  "security": "",
  "details": "",
  "commission-rate": "0.05",
  "commission-max-rate": "0.5",
  "commission-max-change-rate": "0.5",
  "min-self-delegation": "1"
}`

`junctiond tx staking create-validator $HOME/.junction/validator.json \
    --from=<key-name> \
    --chain-id=junction \
    --fees=500amf`






