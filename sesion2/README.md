Asignatura Cloud Computing del Máster en Ingeniería Informática. 

Departamento de Ciencias de la Computación e Inteligencia Artificial.

Universidad de Granada.

<HR>

Profesor: **Manuel J. Parra-Royón**

Email: **manuelparra@decsai.ugr.es**

Tutorías: **Viernes, de 17:30 a 18:30, despacho D31 (4ª planta) Escuela Técnica Superior de Ingenierías Informática y de Telecomunicación (ETSIIT).**

Material de prácticas de la asignatura: **https://github.com/manuparra/PracticasCC**

<HR>


# Sesión 2: Despliegue automatizado de software y servicios 

Tabla de contenido:

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


## Despliegue automatico de servicios y software

El despliegue de herramientas y servicios, se puede realizar de múltiples formas. Desde las herramientas tradicionales 
como utilizar un shell-script hasta las más modernas y completas como utilizar ``ansible``, ``chef``, ``puppet`` o ``salt``.

¿Cuál es la ventaja de usar una de las herramientas de automatización como ``ansible`` ?

No es necesario 'a priori' conocer la distribución de linux o el soporte final del Sistema Operativo sobre el cual vamos 
a desplegar el software o los servicios. Con ``ansible`` podemos indicarle que instale unos paquetes, pero no le decimos que use 
``yum`` para el caso concreto, por lo que es la propia herramienta ``ansible`` la que 'conoce' como tiene que instalar el software para 
el sistema  final.

Con este tipo de herramientas de automatización conseguiremos:

- Automatizar el aprovisionamiento de máquinas.
- Centralizar la gestión e instalación de servivios y software.
- Gestionar la configuración de los servicios de esas máquinas.
- Realizar despliegues y orquestar los servicios.
- Dotar a nuestros despliegues de solidez.

En cambio si usamos los métodos tradicionales de usar un ``shell-script``, necesitaremos adaptarlo a nuestra distribución, copiarlo en todos 
los nodos, etc. 

## Breve introducción a ANSIBLE

Ansible es una herramienta de gestión y aprovisionamiento de configuraciones, similar a Chef, Puppet o Salt.

Sólo SSH:  utiliza SSH para conectarse a servidores y ejecutar las Tareas configuradas en los llamados ``playbooks``.

Una cosa agradable acerca de Ansible es que es muy fácil convertir scripts bash (todavía una manera popular de realizar la gestión de configuración) en Ansible Tasks. Puesto que se basa principalmente en SSH, no es difícil ver por qué este podría ser el caso - Ansible termina ejecutando los mismos comandos.

 Ansible automatiza el proceso antes de ejecutar Tareas utilizando contextos. Con este contexto, Ansible es capaz de manejar la mayoría de los casos de límite - del tipo que solemos tratar con scripts más largos y cada vez más complejos.

Las Tareas en ANSIBLE son identificables. Sin una gran cantidad de codificación extra, los scripts shell-bash no suelen funcionar de forma segura una y otra vez. Ansible utiliza "Hechos", que es información del sistema y del entorno que recopila ("contexto") antes de ejecutar Tareas.

Ansible usa estos hechos para comprobar el estado y ver si necesita cambiar algo para obtener el resultado deseado. Esto hace que sea seguro ejecutar Ansible Tasks contra un servidor una y otra vez.

### Instalación de ANSIBLE

En el nodo principal de atcstack.ugr.es ya está instalado ANSIBLE, con lo que no tendrás que instalarlo.

Si lo quieres instalar en las intancias de openstack que hayas creado para orquestar la instalación desde una de ella, usa:

```
sudo yum install epel-release
sudo yum install ansible
```


### Definición del fichero de inventario

Ansible trabaja contra múltiples sistemas en su infraestructura al mismo tiempo. Esto lo hace seleccionando porciones de sistemas listados en el inventario de Ansible, que por defecto se guarda en la ubicación ``/etc/ansible/hosts``. Se puede especificar un archivo de inventario diferente utilizando la opción ``-i <path>`` en la línea de comandos al utilizar los  PlayBooks.

Este inventario no sólo es configurable, sino que también puede utilizar múltiples archivos de inventario al mismo tiempo y extraer el inventario de fuentes dinámicas o de nube o diferentes formatos (YAML, ini, etc).

Para nuestras prácticas definiremos un fichero de hosts propio, donde se hará el inventario de máquinas que se usarán para todo el curso.




### PlayBooks básicos

Los Playbooks pueden ejecutar múltiples Tareas y proporcionar algunas funciones más avanzadas que nos perderíamos usando comandos ad-hoc. 

Los PlayBooks son ficheros de texto en formato YAML.


Un ejemplo de fichero PlayBook:

```
---
- hosts: local
  become: true
  tasks:
   - name: Install Nginx
     apt: pkg=nginx state=installed update_cache=true
```

Lo desgranamos:

- El PlayBook se ejecutará en el equipo llamado ``local``
- Contiene una serie de tareas identificadas por ```tasks```:
 - Una de las tareas es instalar NGINX, debe estar instalado y además que se actualice la cache del repositorio de paquetes.
 - Además la tarea es que use ``apt`` para instalar.

¿Cómo lo ejecutamos?:

```
ansible-playbook -s nginx.yml
```



## Despliegue de software y servicios sobre MV

Creamos una nueva instancia dentro de OpenStack. Para esta instancia, usamos una IP del rango correspondiente para cada usuario.
Esta instancia la usaremos para probar el despliegue de software dentro de la MV.

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

Nos conectamos a ella:

```
ssh -i ficherokey.sh fedora@192.168.0.XXX
```

Si todo es correcto, salimos de la MV creada y volvemos al nodo de atcstack.ugr.es (si estás dentro de la instancia, sal con ``exit``).

### Creamos el fichero de inventario

Dentro de tu HOME de atcstack, debes crear un fichero de inventario de hosts. Este fichero contiene la lista de IP/Hosts que 
se usarán para desplegar e instalar software.

Para ello, creamos un fichero que se llame por ejemplo ``hosts`` (usa pico, joe, vi/vim, etc...):

```
vi hosts
```

y añadimos el siguiente contenido:

```
[MVs]
192.168.0.110 ansible_ssh_private_key_file=tuficherokey.pem  ansible_connection=ssh ansible_user=fedora
```

y guardamos el fichero

Explicamos:

- Grupo de Hosts que se llama ```[MVs]```
- Línea de hosts:
 - IP de la instancia que queramos desplegar:
 - ansible_ssh_private_key_file=tuficherokey.pem Indica el nombre del fichero de tu llave de ssh que se usará para conectar a la instancia.
 - ansible_user=fedora Indica el usuario que usaremos para conectar


En este fichero iremos guardando todo el inventario de MVs que tengamos que desplegar con software o servicios para la práctica. En él tendrás que ir añadiendo todas tus instancias.

### Instalamos un servicio web

Creamos un fichero que se llame por ejemplo: ```nginx.yml``` y añadimos el siguiente contenido (usa pico, joe, vi/vim, etc... instala el que quieras):

```
---
- hosts: MVs
  become: true
  tasks:
   - name: Install Nginx
     package: pkg=nginx state=installed 


```

Y ahora ejecutamos:

```
ansible-playbook -s nginx.yml
```

Ya está instalado NGINX, pero no está iniciado en la MV, por lo que tenemos que añadir al fichero ``nginx.yml`` lo siguiente:

```
---
- hosts: MVs
  become: true
  tasks:
   - name: Install Nginx
     package: pkg=nginx state=installed
     notify:
      - Start Nginx

  handlers:
   - name: Start Nginx
     service: name=nginx state=restarted enabled=yes
```


##


