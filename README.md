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
```

root@wireguard:/etc/wireguard# ls -l
total 20
-rw------- 1 root root  45 Oct 18 15:28 00_server_clave_privada
-rw------- 1 root root  45 Oct 18 15:28 00_server_clave_publica
-rw------- 1 root root  45 Oct 18 15:29 01_client_clave_privada
-rw------- 1 root root  45 Oct 18 15:29 01_client_clave_publica


Crear archivo de configuracion servidor 

```
sudo nano /etc/wireguard/wg0.conf
```


La interface eth0 es un ejemplo,  la interface debe ser por donde el servidor este escuchando peticiones


[Interface]
PrivateKey = <Clave privada del server>
Address = 10.0.0.1/32
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
ListenPort = 51820

[Peer]
PublicKey = <Clave pública del cliente>
AllowedIPs = 10.0.0.2/32


Levantado de la conexión VPN en el servidor
```
wg-quick up wg0
```
Persistencia del interfaz de conexión
```
systemctl enable wg-quick@wg0
```
Activación del reenvío de paquetes
```
sysctl -w net.ipv4.ip_forward=1
```






## Instalacion Servidor 

Actualizar servidor 

```
sudo apt-get update && sudo apt-get upgrade -y
```

Instalar wireguard

```
sudo apt-get install wireguard
```

Crear archivo del cliente 


```
sudo nano /etc/wireguard/wg0.conf
```

```
[Interface]
PrivateKey = <Clave privada cliente>
Address = 10.0.0.2/32
DNS = 8.8.8.8

[Peer]
PublicKey = <Clave pública server>
AllowedIPs = 0.0.0.0/0  
Endpoint = IP_Server:51820
```
arrancar cliente vpn 

```
wg-quick up wg0
```

Si se quiere permitir el acceso de internet se debe modificar en el cliente la siguiente linea AllowedIPs a los segmentos solamente internos 









