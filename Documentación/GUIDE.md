# GUIDE
</br>

### REQUISITOS DE SISTEMA  
Utilizando VirtualBox se utilizan 2 maquinas virtuales abiertas simultáneamente.  
Teniendo en cuenta esto utilicé aproximadamente:  
* 4GB de memoria RAM (2GB para cada máquina).  
* 3 hilos (GPU), 2 hilos para la maquina Ubuntu, y 1 hilo para PFsense.  
* 10GB para PFsense y 50GB para Linux/Ubuntu.

### MÁQUINAS Y RED
Para estas máquinas virtuales se requiere de un virtualizador, en esta guía se utiliza **VirtualBox**, estas van a ser:  
- Una máquina virtual con [PFsense](https://www.pfsense.org/download/)
- Una máquina virtual con [Linux Ubuntu 24.04](https://ubuntu.com/download/desktop)
  
Dentro de VirtualBox utilizaremos 2 tipos de redes:  
* Una es la red WAN para dar internet.  
* Y la red LAN que viene por defecto en VirtualBox.

## <ins>PFsense</ins>
Antes de iniciar la máquina hay que darle <ins>click derecho</ins> y entrar en configuración. Dentro de configuración vamos a la pestaña <ins>Network</ins>, donde hay que habilitar el <ins>Adapter 2</ins>. Ahí cambiamos la opción WAN por INTERNAL NETWORK, y en opciones avanzadas, en **Promiscuous Mode**, habilitamos **Allow All** y le damos a Accept.

Una vez finalizada la instalación. Para que PFsense detecte cuál es cada red, se asignan manualmente de la siguiente manera:
* Se elige la opción 1.
* En la opción de VLANs se selecciona simplemente Enter.
* Cuando pida identificar la red WAN, se elige la opción que esté configurada en VirtualBox como Adapter 1.
* Cuando pida identificar la red LAN, se elige la opción que esté configurada en VirtualBox como Local Network, que en este caso sería Adapter 2.

> [!NOTE]  
> Teniendo en cuenta cómo identifica las redes PFsense, se puede definir que Adapter1 = em0 - Adapter2 = em1 - Adapter3 = em2 - Adapter4 = em3. Siempre teniendo en cuenta que estamos utilizando VirtualBox.

Cuando se termine de configurar, hay que ir a la opción 2 donde:

* Se selecciona la red LAN con una opción numérica.
* Luego elegiremos la IP de la LAN (Ej: 192.168.1.1).
* Luego la máscara, que será de 24.
* En la opción de ingresar una IP Gateway, simplemente se le da al botón Enter.
* En la opción de ingresar una IP v6, se da Enter nuevamente.
* Cuando se solicite elegir si queremos DHCP para la red LAN, ingresa "Y".
* El rango IP para el DHCP empezaría (utilizando el ejemplo anterior) con 192.168.1.2 a 192.168.1.10. Luego damos Enter y esperamos a que se guarden los cambios.

Cuando se termine de configurar, nos va a dar una dirección web que, utilizando el ejemplo anterior, quedaría "http://192.168.1.1/" y con eso ya estaría terminada la configuración de PFsense.

> [!NOTE]  
> Para loguearse en la página de PFsense utilizar:  
> **Login**: admin  
> **Password**: pfsense

### <ins>Linux/Ubuntu</ins> 
Antes de iniciar la máquina, vamos a la configuración y en la opción de Network elegimos en Adapter1 la opción de "**Internal Network**".  
Sigue la instalación y, al ingresar al escritorio, configuramos la red.  

La misma se configura desde la parte superior derecha:
* Wired Connected
  * Wired Settings
    * Clic en la "tuerca"
      * Ir a la pestaña "IPv4"

Aquí configuramos la dirección IP manualmente con alguna IP dentro del rango DHCP o simplemente dejar que DHCP lo conecte automaticamente.

Para probar que la conexión es estable, accedemos a "http://192.168.1.1/" (ejemplo). Si accede, la red LAN funciona correctamente.

## <ins>Ntopng</ins>
Una vez conectado a Pfsense, utilize la terminal para ejecutar para actualizar el repositorio apt.

```bash
sudo apt update
sudo apt upgrade
```

Luego se instalan las dependencias necesarias.

```bash
apt-get install software-properties-common wget gnupg
add-apt-repository universe
```

Con las dependencias instaladas, hay que dirigirse al directorio `/var/cache/apt/archives/` utilizando:

```bash
cd /var/cache/apt/archives/
sudo wget https://packages.ntop.org/apt/24.04/all/apt-ntop.deb
```

Ahora se debe instalar el repositorio con el comando:

```bash
sudo apt install ./apt-ntop.deb
```
</br>

Una vez instalado, se alcualiza el repositorio volviendo a utilizar `Sudo apt update`, para luego instalar las los paquetes de **Ntopng**:

```bash
sudo apt install pfring-dkms nprobe ntopng n2disk cento ntap
```

### configuración 

Luego de la instalacion se debe confiigura antes de levantar el servicio. Para esto se debe dirigir a la ruta `/etc/ntopng/ntopng.conf` y en esta utilizar el comando nano, siendo de esta forma:

```bash
cd /etc/ntopng/ntopng.conf
sudo nano ntopng.conf
```

En este archivo se debe configurar guiandose con el archivo de ejemplo [ntopng.conf](/Documentación/ntopng.conf). Aqui se debe configuar las siguients lineas como indica el archivo: #-i, #-w, #-m.

> [!IMPORTANT]  
> Todas las líneas que contienen `#` están comentadas. Quitar el `#` en las líneas que quieras configurar.

Una vez configurado se deve habilitar el servicio utilizando:

```bash
sudo systemctl enable ntopng
sudo systemctl start ntopng
```

### Monitorización
para ingresar a ntopng en la web se utiliza `http://TU_IP:3000`, utilizando las credenciales 











