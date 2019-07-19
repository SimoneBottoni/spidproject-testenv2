# spidproject-testenv2

## Docker CE Ubuntu

- Update the apt package index
```
sudo apt-get update
```

- Install packages to allow apt to use a repository over HTTPS
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```

- Add Docker’s official GPG key
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

- Set up the stable repository
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

- Install Docker CE
```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```

- Verify that Docker CE is installed correctly
```
sudo docker run hello-world
```

- Autostart Docker
```
sudo systemctl enable docker
```

## Requisiti TestEnv2
```
sudo apt-get install libxmlsec1 libffi6
git clone https://github.com/italia/spid-testenv2.git
cd spid-testenv2
pip install -r requirements.txt
```

## Installazione
- Directory mappata in conf/ all'interno del container
```
mkdir /etc/spid-testenv2
```

- Creare nella directory il file config.yaml e la coppia chiave/certificato per l'IdP, nonché eventuali metadata SP.

- Creare la coppia chiave/certificato
```
openssl req -x509 -nodes -days 730 -sha256 -subj '/C=IT' -newkey rsa:2048 -keyout idp.key -out idp.crt
```

- Creare il container
```
docker create --name spid-testenv2 -p 8088:8088 --restart=always \
   --mount src="/etc/spid-testenv2",target="/app/conf",type=bind \
   italia/spid-testenv2
```

- Avviare il container
```
docker start spid-testenv2
```

- Il log si può visualizzare con
```
docker logs -f spid-testenv2
```
