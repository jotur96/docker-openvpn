

# CONFIGURACION INICIAAL

```bash
docker-compose run --rm openvpn ovpn_genconfig -u udp://<IP-DE-TU-SERVIDOR>
docker-compose run --rm openvpn ovpn_initpki
```

## PERMISOS

```bash
sudo chown -R $(whoami): ./openvpn-data
```

## Configurar TUN

### Verificar que este activado el TUN

```bash
ls -l /dev/net/tun
```

### Si no esta activaod, activar con

```bash
sudo modprobe tun
```


## INICIAR CONTENEDOR

```bash
docker-compose up -d
```

## GENERAR CERTIFICADO DEL CLIENTE

```bash
export CLIENTNAME="el_nombre_del_cliente"
# con contraseña
docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME
# sin contraseña
docker-compose run --rm openvpn easyrsa build-client-full $CLIENTNAME nopass
```

## CREAR ARCHIVO DE CONFIGURACION PARA EL CLIENTE

```bash
docker-compose run --rm openvpn ovpn_getclient $CLIENTNAME > $CLIENTNAME.ovpn
```

## REVOCAR EL CERTIFICADO

```bash
# Dejando los archivos crt, key y req.
docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME
# Borrando los correspondientes archivos crt, key y req.
docker-compose run --rm openvpn ovpn_revokeclient $CLIENTNAME remove
```
