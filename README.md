### Asignatura Cloud Computing del Máster en Ingeniería Informática. 

Departamento de Ciencias de la Computación e Inteligencia Artificial.

Universidad de Granada.


Profesor: **Manuel J. Parra-Royón**

Email: **manuelparra@decsai.ugr.es**

Tutorías: **Viernes, de 17:30 a 18:30, despacho D31 (4ª planta) Escuela Técnica Superior de Ingenierías Informática y de Telecomunicación (ETSIIT).**

# Sesión 1: OpenStack

Tabla de contenido:

  * [Requisitos iniciales](#requisitos-iniciales)
  * [Credenciales y acceso inicial](#credenciales-y-acceso-inicial)
  * [Acceso vía WEB](#acceso-vía-web)
  * [Acceso vía SSH](#acceso-vía-ssh)
  * [Gestión de OpenStack desde Horizon](#gestión-de-openstack-desde-horizon)
    + [Pantalla inicial](#pantalla-inicial)
    + [Gestión de imagenes](#gestión-de-imagenes)
    + [Creación de credenciales de usuario (par de claves)](#creación-de-credenciales-de-usuario--par-de-claves-)
    + [Crear reglas del grupo de seguridad](#crear-reglas-del-grupo-de-seguridad)
    + [Creación de fichero de autenticacion de usuario (RC file)](#creación-de-fichero-de-autenticacion-de-usuario--rc-file-)
    + [Topología de red](#topología-de-red)
    + [Creación de instancias](#creación-de-instancias)
  * [Gestión de OpenStack desde el shell](#gestión-de-openstack-desde-el-shell)
    + [Inicio de la sesión en el shell](#inicio-de-la-sesión-en-el-shell)
    + [Autenticación en OpenStack vía Shell](#autenticación-en-openstack-v-a-shell)
    + [Identificación de los elementos en OpenStack](#identificación-de-los-elementos-en-openstack)
    + [Gestionar las imágenes](#gestionar-las-imágenes)
    + [Gestionar las redes e IPs](#gestionar-las-redes-e-ips)
    + [Gestionar los grupos de seguridad](#gestionar-los-grupos-de-seguridad)
    + [Gestionar los Flavor](#gestionar-los-flavor)
    + [Gestionar los pares de claves](#gestionar-los-pares-de-claves)
    + [Crear un instancia](#crear-un-instancia)
    + [Crear un instancia con una IP estática](#crear-un-instancia-con-una-ip-est-tica)
    + [Consultar instancias](#consultar-instancias)
    + [Gestionar las instancias (estados y acciones)](#gestionar-las-instancias--estados-y-acciones-)
    + [Acceso SSH a las instancias](#acceso-ssh-a-las-instancias)
  * [Ejercicio práctico A (instalación de servicios de forma manual)](#ejercicio-práctico-a--instalación-de-servicios-de-forma-manual-)
  * [Ejercicio práctico B (instalación de servicios utilizando cloud-init)](#ejercicio-práctico-b--instalación-de-servicios-utilizando-cloud-init-)
    + [Script](#script)
    + [Cloud-Init](#cloud-init)
    + [Lanzar la instancia con inyección de software:](#lanzar-la-instancia-con-inyecci-n-de-software-)




## Requisitos iniciales

- Tener cuenta de acceso a atcstack.ugr.es.
- Conocimientos básicos del SHELL.
- Conceptos básicos de Cloud y Máquinas Virtuales.

## Credenciales y acceso inicial

Cada alumno tiene asignado un nombre de usuario y una clave que servirán para autenticarse dentro del cluster de OpenStack. 
El nombre de usuario y clave asignado a cada alumno se informará en la primera sesión de prácticas.

El acceso al cluster de OpenStack se realiza a través de los siguientes puntos de entrada (*es necesario estar conectado a la VPN de la UGR*):

- Entorno WEB OpenStack Horizon: http://atcstack.ugr.es/dashboard/auth/login/?next=/dashboard/
- Consola del cluster OpenStack: ssh usuario@atcstack.ugr.es

Para ambos es necesario utilizar las mismas credenciales de acceso.

## Acceso vía WEB

Para acceder vía web, utilizamos un navegador para la dirección:  http://atcstack.ugr.es/dashboard/auth/login/?next=/dashboard/


![LoginON](imgs/login_on.png)

Por defecto en Domain, usamos ``default``

## Acceso vía SSH

Para usar SSH, utilízalo desde la consola de Linux o bien desde Windows usando la aplicación ``putty``.

Si usas Windows descarga ``putty`` desde: https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html e indica los siguientes datos en la pantalla de cofiguración:

- Hostname or IP: ``atcstack.ugr.es``
- Port: ``22``
- Connection Type: ``SSH``

Y luego ``Open`` para conectar, donde te pedirá despues las credenciales de acceso.

Si usas SSH desde una consola:

``ssh usuario@atcstack.ugr.es``


## Gestión de OpenStack desde Horizon

Una parte importante de la gestión de OpenStack, se puede realizar desde la interfaz web. Si bien es más limitada que la interfaz de comandos, ofrece una serie de 
elementos básicos para poder realizar la instanciación de Máquinas Virtuales y la configuración y gestión de aspectos sencillos de OpenStack de una forma muy intuitiva.

Esta primera parte de guión nos centraremos en utilizar todo lo que nos ofrece la herramienta web.

### Pantalla inicial

La primera vez que se acceder a OpenStack Horizon, se muestra la siguiente pantalla:

![HorizonInicio](imgs/pantalla_inicio_horizon.png)

En ella se puede consultar el estado general de nuestra quota de instancias de Máquinas Virtuales y otros recursos que estamos consumiendo como CPUs, RAM o número de redes (IPs).

### Gestión de imagenes

Para acceder a esta sección vamos al menú de la derecha: *Project -> Compute --> Images*.

Se han cargado unas imagenes específicas con las distribuciones de Linux más comunes y que nos permitirán desplegar cualquier software dentro de las prácticas:

![Imagemes](imgs/imagenes.png)

Para las prácticas usaremos de forma indistinta: *CirrOS, Fedora27 (yum), Ubuntu16 (apt-get), CentOS7(yum) y CoreOS*.


### Creación de credenciales de usuario (par de claves)

Para poder conectar y acceder por SSH a las intancias que se creen desde OpenStack, es necesario crear un par de claves para la autenticación. Para ello vamos al menú: *Project -> Compute --> Access & Security* y luego la pestaña *KeyPairs*:

![KeyPair](imgs/keypair.png)

Ahora creamos nuestro par de claves utilizando la opción: *Create Key Pair*.

![CrearKeyPair](imgs/crearkeypair.png)

Le asignamos un nombre y luego pulsamos en *Create Key Pair*

Esto automáticamente creará un par de claves que podremos usar para poder conectar a nuestras Máquinas Virtuales, sin necesidad de utilizar usuario/clave, por lo que se facilita bastante la gestión de las imágenes.

Al crear el par de claves, se nos descargará un fichero ``nombre.pem`` que contiene nuestras llave de autenticación que más tarde usaremos.

### Crear reglas del grupo de seguridad

Para poder conectar y acceder a las intancias y **a puertos en las instancias**, es necesario añadir las reglas al grupo de seguridad. Para ello vamos al menú: *Project -> Compute --> Access & Security* y luego la pestaña *Security Groups*:

![security_groups](imgs/security_groups.png)

Una vez ahí pinchamos en el grupo ``default`` y en ``Manage Rules``:

![rules](imgs/rules.png)

Y añadimos las reglas minímas necesarias para poder:

1. Conectar por SSH a las instancias
2. Hacer PING a las instancias.

Recuerda que sin estas dos reglas, no puedes trabajar con las instancias.

1. Hacemos clic en ``Add Rule`` --> Luego en Rule: Seleccionamos ``All ICMP`` y luego Direction: Seleccionamos ``Ingress`` y pulsamos ``ADD`` para guardar
2. Repetimos la regla, pero cambiamos ``Ingress`` por ``Egress``, y pulsamos ``ADD`` para guardar.
3. Hacemos clic en ``Add Rule`` --> Luego en Rule: Seleccionamos ``SSH``  y pulsamos ``ADD`` para guardar.

Por lo que después de aplicar estas reglas, tendremos en la lista lo siguiente:

![rules](imgs/rules_after.png)

Cualquier otro puerto que necesites, debes abrirlo desde aquí.






### Creación de fichero de autenticacion de usuario (RC file)

Para poder interactuar con OpenStack desde el shell, es necesario descargar el fichero de la autenticación del usuario que luego copiaremos a nuestra cuenta en el servidor. Para ello vamos al menú: *Project -> Compute --> Access & Security* y luego la pestaña *API access*:

![KeyPair](imgs/api_access.png)

Para crear el fichero de autenticación de usuario utilizamos la opción ``Download OpenStack RC File v3.0``. *[ NO usar la versión v2.0, ya que no es compatible al 100% con algunos módulos del shell de OpenStack ]*. 

Esto descargará en tu navegador el fichero que contiene el script de autenticación para la sesión de OpenStack desde el shell del servidor. El fichero que se descargará sera de la forma: ``CCproject_USUARIO-openrc.sh``.

### Topología de red

Cada alumno, tendrá asignado un conjunto de direcciones IP en las que podrá desplegar Máquinas Virtuales.  Estas IPs serán de la forma ``192.168.0.XXX``. 
Para ver la configuración de red : *Project -> Network --> Network Topology*. 

![Network Topology](imgs/network_topology.png)

### Creación de instancias

Para la creación de una instancia, nos vamos a la opción: *Project -> Compute --> Instances*:

![Instancias](imgs/instancias.png)

Y usamos la opción ``Launch instance``. Antes de crear una instancia es necesario revisar varios aspectos de la creación de instancias:

![Instancias](imgs/pantalla_instancias.png)

- En esta pantalla se indicará el Nombre de la Instancia ``Instance name``. La zona de disponibilidad, por defecto dejaremos ``NOVA`` y en ``count`` pondremos ``1``.


![Source](imgs/source.png)

- En esta pantalla se indicará la imagen que se usará para la Máquina Virtual que se desplegará. Para este primer ejemplo usaremos Fedora27. Con lo que hacemos click en el botón + para incluirla en el despliegue.


![Source](imgs/flavor.png)

- La selección de un ``flavor`` es importante, ya que es el que nos permite asignarle los recursos a la Máquina Virtual que vamos a crear. Es importante tener en cuenta que el ``flavor`` asignado debe tener caracteristicas que se ajuste a la imagen que vamos a usar; por ejemplo si vamos a usar CentOS7 como imagen y está necesita como mínimo 512 MB de RAM y un espacio en disco de 10GB, tendremos que seleccionar un `flavor` que cubra como mínimo esas características. En caso de no ser así y asignar un `flavor` que no se ajusta, la instanciación de la Máquina Virtual dará error de recursos.
- Para nuestro ejemplo con CentOS7 usamos el `flavor` :

```
m2.medium	VCPU 2	RAM 1 GB	HD 20 GB
```

![Redes](imgs/redes.png)

- La selección de redes ya viene indicada por defecto, por no es necesario indicar nada. Nuestra red por defecto se llama ``provider``.

![Redes](imgs/security.png)

- En la sección security por defecto ya viene marcado el grupo de seguridad ``default``. Esto indica que la creación de todas las Máquinas Virtuales, comparten un grupo de seguridad que define filtros y reglas de acceso IP y gestiona el flujo de tráfico de red desde y hacia la instancia.

![instancia_keypair](imgs/instancia_keypair.png)

- En pasos anteriores hemos creado un par de claves para conectar con nuestras instancias. Por defecto viene seleccionado el par de claves que hemos creado en pasos previos. Este par de claves será inyectado dentro de la instancia en tiempo de arranque.

Una vez completados estos pasos, creamos la instancia desde el botón: **LAUNCH INSTANCE**.

El proceso de instanciación puede durar unos instantes y dependerá del `flavor` usado y sus características. El proceso de arranque de la propia instancia también lleva un tiempo considerable.

La pantalla de gestión de instancias proporciona una visión global del estado de las mismas:

![pantalla_instancias](imgs/pantalla_instancias.png)

Para conocer los detalles de la instancia, hacemos clic en el nombre de la instancia: 


![resumen_instancia](imgs/resumen_instancia.png)

En esta pantalla se pueden conocer los aspectos detallados de la instancia como los recursos usados, la IP y redes asignadas, los puertos IN/OUT abiertos en la MV, etc. 


![log_instancia](imgs/log_instancia.png)

Esta pantalla nos permite ver el LOG de arranque de la Instancia, lo cual es muy útil para verificar que la creación de la instancia fue correcta y por ejemplo se le ha asignado bien la RED, o se ha instalado correctamente algún software. Desde esta pantalla no se puede interactuar con la MV, ya que es simplemente informativa. Para poder interactuar con la MV, sin tener que usar SSH, se utiliza la opción Console.

![log_instancia](imgs/console_instancia.png)

Para conectar con la instancia SIN USAR ssh, a través de QEMU, podemos usar la consola web para verificar que todo está correcto en la MV. No es la mejor opción para interactuar con la MV creada, ya que el tiempo de respuesta es alto. Esta opción simplemente es para poder realizar alguna gestión sin tener que conectar por SSH a la MV.

## Gestión de OpenStack desde el shell


### Inicio de la sesión en el shell

Para poder utilizar OpenStack desde el shell son necesarias 3 cosas:

- Copiar la llave ``.pem`` que se generó en el apartado *Creación de credenciales de usuario (par de claves)*
- Copiar la el fichero de autenticación de la sesion en OpenStack que se ha creado en el apartado *Creación de fichero de autenticacion de usuario (RC file)*
- Conectar por SSH a atcstack.ugr.es


Lo primero que hacemos es copiar los dos ficheros con nuestra llave y script de autenticacion a la cuenta en atcstack.ugr.es. Para ello:

- Abre una nueva consola en tu PC.
- Localiza el fichero PEM y el fichero del script de autenticacion (por defecto en tu carpeta Descargas/Downloads).

y ahora :

- Copia el fichero PEM (cambia el nombre ``fichero.pem`` por el nombre de tu par de claves): 

```
scp fichero.pem tuusuario@atcstack.ugr.es:fichero.pem
```

- Copia el fichero de autenticación: 

```
scp CCproject_USUARIO-openrc.sh tuusuario@atcstack.ugr.es:CCproject_USUARIO-openrc.sh
```


Hecho esto conectamos con SSH al servidor atcstack.ugr.es (o cambiamos a la ventana donde previamente teníamos una sesión):

```
ssh tuusuario@atcstack.ugr.es
```

Usamos el comando ``ls -l`` y vemos que efectivamente los ficheros copiados están disponibles.




### Autenticación en OpenStack vía Shell

Previamente tenemos que haber conectado al cluster:

```
ssh tuusuario@atcstack.ugr.es
```

Para poder usar autenticar en la session  de OpenStack desde el shell, hacemos:

```
source CCproject_USUARIO-openrc.sh
```

Esto nos pedirá nuestra clave de usuario de OpenStack. Una vez validado el usuario, ya podemos utilizar OpenStack desde el shell.  Para comprobar que todo es correcto:

```
openstack image list
```

El resultado será:

![images_list](imgs/images_list.png)

Se debe mostrar la lista de imagenes que hay disponibles en la plataforma.

### Identificación de los elementos en OpenStack

Es importante saber que para referenciar elementos en OpenStack se hace utilizando el ``ID`` o el ``Name`` de cada componente. Por ejemplo al hacer un listado de las imágenes disponibles en OpenStack, nos muestra el ``ID`` o el ``Name``, ambos identificadores son válidos para usarlos en la definición de opciones de otros comandos de OpenStack.

### Gestionar las imágenes

Para ello usamos:

```
openstack image OPCIONES
```

Listado de imágenes completo:


```
openstack image list
```


### Gestionar las redes e IPs

Para ello usamos:

```
openstack network OPCIONES
```

Listado de redes disponibles:

```
openstack network list
```

![network_list](imgs/network_list.png)


### Gestionar los grupos de seguridad

Para ello usamos:

```
openstack security group OPCIONES
```

Listado de los grupos de seguridad disponibles:


```
openstack security group list
```

![security_list](imgs/security_list.png)

### Gestionar los Flavor

Para ello usamos:

```
openstack flavor OPCIONES
```

Listado de flavor:


```
openstack flavor list
```

![flavor_list](imgs/flavor_list.png)


### Gestionar los pares de claves



Para ello usamos:

```
openstack keypair OPCIONES
```

Listado de claves:

```
openstack keypair list
```

![key_list](imgs/key_list.png)

### Crear un instancia

Para crear una instancia, hay que tener en cuenta que como mínimo necesitamos conocer los ``ID`` de los siguientes componentes antes de usar el comando de creación de instancias:

- El ``ID`` de la imagen de Sistema Operativo que se desplegará.
- El ``ID`` del flavor que vamos a utilizar.
- El ``ID`` de la RED que usaremos.
- El ``ID`` del grupo de seguridad que aplicaremos.
- El ``ID`` o ``Name`` del keypair que se usará.

Una vez tengamos todos estos ``ID`` ya podemos lanzar el siguiente comando (ejemplo):

```
openstack server create --flavor XXXXX --image XXXXXX  --nic net-id=XXXXXXX --security-group XXXXXX  --key-name XXXXXXX MI_INSTANCIA
```

El nombre de la instancia viene definido por MI_INSTANCIA, por lo que debes modificar ese valor al que desees.

Un ejemplo completo del comando sería (debes cambiar el ``--key-name test`` por el nombre de tu ``key`` y el nombre de la instancia ``MVPrueba`` por otro nombre):

```
openstack server create --flavor 6630ad0e-55d7-4a44-805a-1a52c37341b3 --image 7c22f753-101a-4a61-b03f-e61b44f51cf7  --nic net-id=04fbef67-adfd-42c4-bb29-0be71ac3cfcf --security-group 80ff5385-5956-4685-87a6-d10be7acf155  --key-name test MVPrueba
```
Al ejecutarlo, obtenemos el resultado del despliegue:

![server_create](imgs/server_create.png)

Ahora esperamos unos minutos y probamos a ver el estado de la creación de la instancia:

```
openstack server list
```

![mv_status](imgs/mv_status.png)

Lo que nos interesa en este listado es el estado de las MVs que hemos lanzado y la IP que se le ha asignado (DINÁMICA). Lee el documento https://docs.openstack.org/nova/latest/reference/vm-states.html, para conocer todos los estados posibles.


### Crear un instancia con una IP estática

Para crear una instancia con una IP estática, consulta la tabla de asignaciones de IP para cada alumno. Esto permitirá crear una instancia con una IP concreta y que no sea dinámica como hasta ahora lo hace automáticamente OpenStack en la configuración por defecto de nuestra plataforma.

**Durante todas las prácticas usaremos las direcciones (rango) de IPs estáticas asignadas para cada alumno.**

Para crear una instancia, hay que tener en cuenta que como mínimo necesitamos conocer los ``ID`` de los siguientes componentes antes de usar el comando de creación de instancias:

- El ``ID`` de la imagen de Sistema Operativo que se desplegará.
- El ``ID`` del flavor que vamos a utilizar.
- El ``ID`` de la RED que usaremos.
- El ``ID`` del grupo de seguridad que aplicaremos.
- El ``ID`` o ``Name`` del keypair que se usará.

Una vez tengamos todos estos ``ID`` ya podemos lanzar el siguiente comando, teniendo en cuenta la opción ``v4-fixed-ip`` donde se especifica la IP concreta que tendrá la instancia.

```
openstack server create --flavor XXXXX --image XXXXXX  --nic net-id=XXXXXX,v4-fixed-ip=192.168.0.XXX --security-group XXXXXX  --key-name XXXXXXX MI_INSTANCIAIP
```

La diferencia con el anterior comando es ``--nic net-id=XXXXXX,v4-fixed-ip=192.168.0.XXX`` donde hay que especificar el valor de la IP estática que deseamos.
	

### Consultar instancias

Para consultar instancias utilizamos:

Listado de claves:

```
openstack server list
```

![mv_status](imgs/mv_status.png)

Para ver en detalle todos los aspectos de la instancia utilizamos:


```
openstack server show <ID/Name>
```

![mv_status_mv](imgs/status_mv.png)

### Gestionar las instancias (estados y acciones)

Estados de las instancias:

![estados](https://docs.openstack.org/nova/latest/_images/graphviz-9808234d14264ae1335b53093f1ef6bf30ed2de5.png)


Pausado de la instancia. Esto almacenrá el estado de la instancia en la RAM y parará la MV.

```
openstack server pause <ID/Name>
```

Para volver a restaurar el estado anterior:

```
openstack server unpause <ID/Name>
```

Suspendido de la instancia. Esto almacerá el estado de la MV y los datos en disco y para la MV. Esta acción libera la CPU y la RAM ocupada por la Mv. Al reanudar la instancia, adquiere de nuevo los recursos.
```
openstack server suspend <ID/Name>
```

Reanudación de la instancia desde el estado de suspendida o pausada:
```
openstack server resume <ID/Name>
```

Borrado de una instancia. Elimina por completo la instancia:

```
openstack server delete <ID/Name>
```

Crear un snapshot del volumen de la instancia:

```
openstack volume snapshot create --volume <IDVOLUME> --force NOMBRESNAPSHOT
```

Previamente debemos ver los volúmenes que tenemos activos:

```
openstack volume list
```

Para borrar un volumen:

```
openstack volume delete <ID/Name>
```

### Acceso SSH a las instancias

Para acceder a todas las instancias que hemos creado, previamente debemos tener copiado en nuestro $HOME de atcstack el fichero ``.pem`` correspondiente. 

Hay que cambiar los permisos del fichero ``.pem`` antes de usarlo con ssh:

```
chmod 400 fichero.pem
```

El acceso desde dentro de ATCSTACK se realiza del siguiente modo (usa como usuario, el correspondiente de cada imagen: en CirrOS es ``cirros``, en fedora27 es ``fedora``)

```
ssh -i fichero.pem fedora@192.168.0.XXX
```

(donde 192.168.0.XXX corresponderá con la IP que la instancia tiene asignada o de la que se ha generado (``openstack server list`` para verlo))

*NO* preguntará por la clave de acceso a la instancia.



## Ejercicio práctico A (instalación de servicios de forma manual)

Una vez que ya puedes acceder a las instancias, vamos a instalarle algún servicio que se pueda consultar desde el exterior. Para ello, dentro de una de las instancias creadas, instalaremos APACHE y PHP.

Accedemos a nuestra instancia:

```
ssh -i fichero.pem fedora@192.168.0.XXX
```


Instalamos el software:

```
sudo yum -y install httpd
sudo yum -y install php
```


Abrimos los puertos localmente en la Instancia:

```
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=443/tcp
sudo firewall-cmd --reload
```

Habilitamos el servicio:

```
sudo systemctl start httpd
sudo systemctl enable httpd
```

Ahora hay que añadir en las reglas del grupo de seguridad el acceso desde y hacia el puerto 80 para nuestras instancias en OpenStack. Para ello, hay que ir al interfaz web:

Vamos al menú: *Project -> Compute --> Access & Security* y luego la pestaña *Security Groups*:

![security_groups](imgs/security_groups.png)

Una vez ahí pinchamos en el grupo ``default`` y en ``Manage Rules``:

![rules](imgs/rules.png)

Añadimos una nueva regla que permita la entrada y salida de trafico de red desde y hacia el puerto 80.


## Ejercicio práctico B (instalación de servicios utilizando cloud-init)

Notas sobre cloud-init : https://cloudinit.readthedocs.io/

CloudInit soporta muchos formatos de entrada para datos de usuario. Sólo tendremos en cuenta ahora dos:

- Scripts (comienzan con ``#!``)
- Cloud Config Files (comienzan con ``#cloud-config``)

### Script

```
#!/bin/bash

 yum -y install httpd
 yum -y install php
 firewall-cmd --permanent --add-port=80/tcp
 firewall-cmd --permanent --add-port=443/tcp
 firewall-cmd --reload
 systemctl start httpd
 systemctl enable httpd
```

### Cloud-Init

Un ejemplo de instalación de paquetes de apache y php:

```
#cloud-config
packages:
  - httpd
  - php
runcmd:
  - [ systemctl, enable, httpd.service ]
  - [ systemctl, start, httpd.service ]
```


### Lanzar la instancia con inyección de software:

Utiliza la opción ``--user-data fichero_cloud_init`` en la llamada a crear la instancia:

```
openstack server create .... --user-data fichero_cloud_init ...
```





