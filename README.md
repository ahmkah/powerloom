
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

// Bunun için WL almış bir cüzdan o cüzdanın privatekey bilgileri ve 1 adet ETH RPC gerekli INFURA deneyebilirsiniz.


git clone https://github.com/PowerLoom/deploy.git --single-branch powerloom_testnet_pretask --branch testnet_pretask 

cd powerloom_testnet_pretask

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

CONTAINER ID   IMAGE                                  COMMAND                  CREATED       STATUS                 PORTS                                                                                                                                                 NAMES
bfa1abe2b8aa   powerloom-pooler-frontend              "sh -c 'sh snapshott…"   2 hours ago   Up 2 hours (healthy)   0.0.0.0:3000->3000/tcp, :::3000->3000/tcp                                                                                                             deploy-pooler-frontend-1
852f3445f11c   powerloom-pooler                       "bash -c 'sh init_pr…"   2 hours ago   Up 2 hours (healthy)   0.0.0.0:8002->8002/tcp, :::8002->8002/tcp, 0.0.0.0:8555->8555/tcp, :::8555->8555/tcp                                                                  deploy-pooler-1
ee652fda8513   powerloom-audit-protocol               "bash -c 'sh init_pr…"   2 hours ago   Up 2 hours (healthy)   0.0.0.0:9000->9000/tcp, :::9000->9000/tcp, 0.0.0.0:9002->9002/tcp, :::9002->9002/tcp                                                                  deploy-audit-protocol-1
5547fb5c1ab4   ipfs/kubo:release                      "/sbin/tini -- /usr/…"   2 hours ago   Up 2 hours (healthy)   4001/tcp, 8080-8081/tcp, 4001/udp, 0.0.0.0:5001->5001/tcp, :::5001->5001/tcp                                                                          deploy-ipfs-1
999de5864a1b   rabbitmq:3-management                  "docker-entrypoint.s…"   2 hours ago   Up 2 hours (healthy)   4369/tcp, 5671/tcp, 0.0.0.0:5672->5672/tcp, :::5672->5672/tcp, 15671/tcp, 15691-15692/tcp, 25672/tcp, 0.0.0.0:15672->15672/tcp, :::15672->15672/tcp   deploy-rabbitmq-1
2c14926d7cfd   redis                                  "docker-entrypoint.s…"   2 hours ago   Up 2 hours (healthy)   0.0.0.0:6379->6379/tcp, :::6379->6379/tcp      

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

http://localhost:3000/
```
```console
# Silmek container durdurmak için ./bash olan screen içinde CTRL+C yapın ve aşağıdaki komudu girin

docker-compose --profile ipfs down --volumes
```



                                                                                                       
