# TURN/STUN сервер

`sudo apt install coturn`

/etc/default/coturn
```
TURNSERVER_ENABLED=1
```

/etc/turnserver.conf
```conf
# Основные порты (по умолчанию 3478 для UDP/TCP)
listening-port=3478
tls-listening-port=5349

# Внешний IP-адрес сервера
external-ip=ВАШ_ПУБЛИЧНЫЙ_IP

# Realm — доменное имя или IP
realm=example.com

# Аутентификация (создание пользователя)
user=username:password

# Отпечатки в сообщениях (требуется для WebRTC)
fingerprint
lt-cred-mech
```

#### Открытие портов в Firewall
Для работы STUN/TURN необходимо открыть следующие порты: 
 - 3478 UDP/TCP — основной порт.
 - 5349 UDP/TCP — если используете TLS.
 - 49152-65535 UDP — диапазон портов для ретрансляции медиаданных (Relay).
```bash
sudo ufw allow 3478/udp
sudo ufw allow 3478/tcp
sudo ufw allow 49152:65535/udp
```

```bash
sudo systemctl restart coturn
```
### Восстановление ext4
Пусть битый сектор это /dev/sdb1, тогда

`sudo blkid /dev/sdb1` - убеждаемся что файловая система ext4
`sudo mke2fs -n /dev/sdb1` - ищем суперблоки (для отладки, сейчас не понадобится)
`sudo e2fsck -fy /dev/sdb1` - сам процесс восстановления
