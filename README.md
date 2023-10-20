# wireguard

## Instalacion Servidor 

Actualizar servidor 

```
sudo apt-get update && sudo apt-get upgrade -y
```

Instalar wireguard

```
sudo apt-get install wireguard
```

IP Forwarding

Configurar ip forwarding para el arranque

```
sudo nano /etc/sysctl.conf
```

y descomentar la siguiente linea 

```
net.ipv4.ip_forward=1
```

verificar con 


```
sudo sysctl -p
```


#### (Opcinal) Configuracion firewall 

Instalar firewall y habilitar 

```
sudo apt install ufw
sudo ufw allow ssh
sudo ufw allow 51820/udp
sudo ufw enable
sudo ufw status
```


Ingresar a carpeta de configuracion


```
cd /etc/wireguard
umask 077
```

Generar llaves publicas y privadas del servidor y cliente

```
wg genkey |tee 00_server_clave_privada | wg pubkey > 00_server_clave_publica
wg genkey |tee 01_cliente_clave_privada | wg pubkey > 01_ciente_clave_publica

root@wireguard:/etc/wireguard# ls -l
total 20
-rw------- 1 root root  45 Oct 18 15:28 00_server_clave_privada
-rw------- 1 root root  45 Oct 18 15:28 00_server_clave_publica
-rw------- 1 root root  45 Oct 18 15:29 01_client_clave_privada
-rw------- 1 root root  45 Oct 18 15:29 01_client_clave_publica

```





