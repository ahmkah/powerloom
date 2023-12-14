
POWERLOOM TESTNET FAZ-2

```console
# ADIM-1, Sunucumuzu güncelleyelim:
sudo apt-get update
sudo apt-get upgrade -y
```

```console
# ADIM-2, Docker ve Docker Compose Kuruyoruz (tek tek giriniz):

sudo apt install apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io

sudo systemctl start docker
sudo systemctl enable docker

docker --version

sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

docker-compose --version

```

```console
# ADIM-3, powerloom_testnet_pretask klonlayalım ve .env değişkenlerini değiştirelim:

// Bunun için WL almış bir cüzdan o cüzdanın privatekey bilgileri ve 1 adet Polygon zkEVM Mainnet RPC gerekli Alchemy, Ankr, Quicknode deneyebilirsiniz.


git clone https://github.com/PowerLoom/deploy.git --single-branch powerloom_testnet_phase2 --branch testnet_phase2 

cd powerloom_testnet_phase2

cp env.example .env

nano .env
```

```console
# ADIM-4 yeni bir screen yaratıp içinde aşağıdaki komudu çalıştırıyoruz.. Screen çıkış için CTRL+A ve D

screen -S powerloom

./build.sh
```

```console
# ADIM-5 şimdi dosker servisler çalışıyormu bakalım.. Aşağıdaki gibi olmalı çıktınız

docker ps
/////////////////////////////////////////////////////////////////////////////////////////////////////
# docker ps

CONTAINER ID   IMAGE                                     COMMAND                  CREATED          STATUS                    PORTS                                                                                                                                                 NAMES
08c24f53d600   ghcr.io/powerloom/pooler:zkevm_quests     "bash -c 'sh snapsho…"   10 minutes ago   Up 10 minutes (healthy)   0.0.0.0:8002->8002/tcp, :::8002->8002/tcp, 0.0.0.0:8555->8555/tcp, :::8555->8555/tcp                                                                  powerloom_testnet_phase2-pooler-1
434195492ce1   ghcr.io/powerloom/audit-protocol:phase2   "bash -c 'sh snapsho…"   10 minutes ago   Up 10 minutes (healthy)   0.0.0.0:9000->9000/tcp, :::9000->9000/tcp, 0.0.0.0:9002->9002/tcp, :::9002->9002/tcp, 0.0.0.0:9030->9030/tcp, :::9030->9030/tcp                       powerloom_testnet_phase2-audit-protocol-1
77df5d96bdd3   rabbitmq:3-management                     "docker-entrypoint.s…"   10 minutes ago   Up 10 minutes (healthy)   4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp, :::5672->5672/tcp, 15671/tcp, 15691-15692/tcp, 25672/tcp, 0.0.0.0:15672->15672/tcp, :::15672->15672/tcp   powerloom_testnet_phase2-rabbitmq-1
4c757c2dc9d9   ipfs/kubo:release                         "/bin/sh -c ' echo '…"   10 minutes ago   Up 10 minutes (healthy)   4001/tcp, 8080-8081/tcp, 4001/udp, 0.0.0.0:5001->5001/tcp, :::5001->5001/tcp                                                                          powerloom_testnet_phase2-ipfs-1
c951d0f2ee58   redis:alpine                              "docker-entrypoint.s…"   10 minutes ago   Up 10 minutes (healthy)   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp                                                                                                             powerloom_testnet_phase2-redis-1    

////////////////////////////////////////////////////////////////////////////////////////////////////
```

```console
# ADIM-6 Yerel makinenizden core_apı bağlantı noktasındaki uzak dağıtım örneğine tünel açın.. 8002 numaralı bağlantı noktasını açar.
<remote-instance-IP-address>  sunucu IP adresinizi yazın..


ssh -nNTv -L 8002:localhost:8002 root@<remote-instance-IP-address>
```

```console
# ADIM-7 , Tarayıcınıza aşağıdaki linki yapıştırın ve çıktıları kontrol edin .. localhost yerine VPS IP yazın..

http://localhost:8002/internal/snapshotter/epochProcessingStatus?page=1&size=1

http://localhost:8002/current_epoch

```
```console
# Silmek container durdurmak için ./bash olan screen içinde CTRL+C yapın ve aşağıdaki komudu girin

docker-compose --profile ipfs down --volumes
```



                                                                                                       

