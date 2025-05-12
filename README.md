### Клонируем проект и создаем пустой `.env`
```
git clone https://github.com/brintadis/jellyfin_server.git
```
```
touch .env
```
![after_clone_ls](src/img/after_clone_ls.png)
### Заполняем `.env`по примеру:

```
MEDIA_PATH=/home/makar/jellyfin_server/media
TZ=Europe/Moscow

# VPN profile settings
VPN_SERVICE_PROVIDER=custom
VPN_TYPE=wireguard
WIREGUARD_ENDPOINT_IP=WIREGUARD_ENDPOINT_IP
WIREGUARD_ENDPOINT_PORT=WIREGUARD_ENDPOINT_PORT
WIREGUARD_PUBLIC_KEY=WIREGUARD_PUBLIC_KEY
WIREGUARD_PRIVATE_KEY=WIREGUARD_PRIVATE_KEY
WIREGUARD_ADDRESSES="WIREGUARD_ADDRESSES"

SONARR_STATIC_CONTAINER_IP=172.20.0.12
RADARR_STATIC_CONTAINER_IP=172.20.0.13
```
### Создаем сеть под jellyfin сервисы
```
docker network create --subnet 172.20.0.0/16 mynetwork
```
### Создаем папку с media, правим доступы
```
mkdir /home/makar/jellyfin_server/media
sudo chown 1000:1000 /home/makar/jellyfin_server/media
```
### Запуск без VPN
```
docker compose -f docker-compose.yml up -d
```
### Запуск с VPN

```
docker compose -f docker-compose-vpn.yml up -d
```

## qBittorrent
`http://localhost:5080`

Default username is admin. Temporary password can be collected from container log `docker logs qbittorrent`

logs example
```
******** Information ********
To control qBittorrent, access the WebUI at: http://localhost:5080
The WebUI administrator username is: admin
The WebUI administrator password was not set. A temporary password is provided for this session: 8hpncXsqy
You should set your own password in program preferences.
Connection to localhost (::1) 5080 port [tcp/*] succeeded!
[ls.io-init] done.
```
![qbit_auth](src/img/qbit_auth.png)


Go to Tools --> Options --> WebUI --> Change password



# FAQ
Q: Вижу ошибку`network mynetwork declared as external, but could not be found`
A: Забыли создать сеть `mynetwork`

Q: При создании сети получаю ошибку `Error response from daemon: invalid pool request: Pool overlaps with other one on this address space`
A: Значит сеть с таким адресом уже существует на вашем сервере. Попробуйте изменить 
```
docker network create --subnet 172.20.0.0/16 mynetwork
```
на 
```
docker network create --subnet 172.21.0.0/16 mynetwork
```

