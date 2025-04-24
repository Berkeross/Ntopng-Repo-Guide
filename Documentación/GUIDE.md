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
- Una máquina virtual con [Linux Ubuntu](https://ubuntu.com/download/desktop)
  
Dentro de VirtualBox utilizaremos 2 tipos de redes:  
* Una es la red WAN para dar internet.  
* Y la red LAN que viene por defecto en VirtualBox.

## <ins>PFsense</ins>
Para que PFsense detecte cuál es cada red, se asignan manualmente de la siguiente manera:
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

> [!IMPORTANT]  
> Para loguearse en la página de PFsense utilizar:  
> **Login**: admin  
> **Password**: pfsense


(seguir mirando esto https://packages.ntop.org/apt/)
