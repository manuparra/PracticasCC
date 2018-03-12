Asignatura Cloud Computing del Máster en Ingeniería Informática. 

Departamento de Ciencias de la Computación e Inteligencia Artificial.

Universidad de Granada.

<HR>

Profesor: **Manuel J. Parra-Royón**

Email: **manuelparra@decsai.ugr.es**

Tutorías: **Viernes, de 17:30 a 18:30, despacho D31 (4ª planta) Escuela Técnica Superior de Ingenierías Informática y de Telecomunicación (ETSIIT).**

Material de prácticas de la asignatura: **https://github.com/manuparra/PracticasCC**

<HR>


[Estructura y asignación de puertos](./estructura/README.md)

# Sesión 1: OpenStack

Tabla de contenido:

  * [Requisitos iniciales](./sesion1/README.md#requisitos-iniciales)
  * [Credenciales y acceso inicial](./sesion1/README.md#credenciales-y-acceso-inicial)
  * [Acceso vía WEB](./sesion1/README.md#acceso-vía-web)
  * [Acceso vía SSH](./sesion1/README.md#acceso-vía-ssh)
  * [Gestión de OpenStack desde Horizon](./sesion1/README.md#gestión-de-openstack-desde-horizon)
    + [Pantalla inicial](./sesion1/README.md#pantalla-inicial)
    + [Gestión de imagenes](./sesion1/README.md#gestión-de-imagenes)
    + [Creación de credenciales de usuario (par de claves)](./sesion1/README.md#creación-de-credenciales-de-usuario--par-de-claves-)
    + [Crear reglas del grupo de seguridad](./sesion1/README.md#crear-reglas-del-grupo-de-seguridad)
    + [Creación de fichero de autenticacion de usuario (RC file)](./sesion1/README.md#creación-de-fichero-de-autenticacion-de-usuario--rc-file-)
    + [Topología de red](./sesion1/README.md#topología-de-red)
    + [Creación de instancias](./sesion1/README.md#creación-de-instancias)
  * [Gestión de OpenStack desde el shell](./sesion1/README.md#gestión-de-openstack-desde-el-shell)
    + [Inicio de la sesión en el shell](./sesion1/README.md#inicio-de-la-sesión-en-el-shell)
    + [Autenticación en OpenStack vía Shell](./sesion1/README.md#autenticación-en-openstack-v-a-shell)
    + [Identificación de los elementos en OpenStack](#identificación-de-los-elementos-en-openstack)
    + [Gestionar las imágenes](./sesion1/README.md#gestionar-las-imágenes)
    + [Gestionar las redes e IPs](./sesion1/README.md#gestionar-las-redes-e-ips)
    + [Gestionar los grupos de seguridad](./sesion1/README.md#gestionar-los-grupos-de-seguridad)
    + [Gestionar los Flavor](./sesion1/README.md#gestionar-los-flavor)
    + [Gestionar los pares de claves](./sesion1/README.md#gestionar-los-pares-de-claves)
    + [Crear un instancia](./sesion1/README.md#crear-un-instancia)
    + [Crear un instancia con una IP estática](#crear-un-instancia-con-una-ip-est-tica)
    + [Consultar instancias](./sesion1/README.md#consultar-instancias)
    + [Gestionar las instancias (estados y acciones)](./sesion1/README.md#gestionar-las-instancias--estados-y-acciones-)
    + [Acceso SSH a las instancias](./sesion1/README.md#acceso-ssh-a-las-instancias)
  * [Ejercicio práctico A (instalación de servicios de forma manual)](./sesion1/README.md#ejercicio-práctico-a--instalación-de-servicios-de-forma-manual-)
  * [Ejercicio práctico B (instalación de servicios utilizando cloud-init)](./sesion1/README.md#ejercicio-práctico-b--instalación-de-servicios-utilizando-cloud-init-)
    + [Script](./sesion1/README.md#script)
    + [Cloud-Init](./sesion1/README.md#cloud-init)
    + [Lanzar la instancia con inyección de software:](./sesion1/README.md#lanzar-la-instancia-con-inyecci-n-de-software-)

# Sesión 2: Despliegue automatizado de software y servicios 

Tabla de contenido:

  * [Requisitos iniciales](./sesion2/#requisitos-iniciales)
  * [Credenciales y acceso inicial](./sesion2/#credenciales-y-acceso-inicial)
  * [Acceso vía WEB](./sesion2/#acceso-v-a-web)
  * [Acceso vía SSH](./sesion2/#acceso-v-a-ssh)
  * [Despliegue automatico de servicios y software](./sesion2/#despliegue-automatico-de-servicios-y-software)
  * [Breve introducción a ANSIBLE](./sesion2/#breve-introducci-n-a-ansible)
    + [Elementos en ANSIBLE:](./sesion2/#elementos-en-ansible-)
    + [Instalación de ANSIBLE](./sesion2/#instalación-de-ansible)
    + [Definición del fichero de inventario](./sesion2/#definición-del-fichero-de-inventario)
    + [PlayBooks básicos](./sesion2/#playbooks-básicos)
  * [Despliegue de software y servicios sobre MV](./sesion2/#despliegue-de-software-y-servicios-sobre-mv)
    + [Creamos el fichero de inventario](./sesion2/#creamos-el-fichero-de-inventario)
    + [Instalamos un servicio web](./sesion2/#instalamos-un-servicio-web)
  * [Despliegue de servicios relacionados con la práctica del curso](./sesion2/#despliegue-de-servicios-relacionados-con-la-práctica-del-curso)


# Sesión 3:

Tabla de contenido:


  * [Credenciales y acceso inicial](./sesion3/#credenciales-y-acceso-inicial)
  * [Acceso vía WEB](./sesion3/#acceso-v-a-web)
  * [Acceso vía SSH](./sesion3/#acceso-v-a-ssh)
  * [Despliegue automatico de servicios y software](./sesion3/#despliegue-automatico-de-servicios-y-software)
    + [Creación de script de inicio/parada y orquestación de MVs](./sesion3/#creaci-n-de-script-de-inicio-parada-y-orquestaci-n-de-mvs)
  * [Contenedores con DOCKER](./sesion3/#contenedores-con-docker)
    + [VIRTUAL MACHINES](./sesion3/#virtual-machines)
    + [CONTAINERS](./sesion3/#containers)
    + [KATAContainers](./sesion3/#katacontainers)
  * [Ventajas de DOCKER](./sesion3/#ventajas-de-docker)
  * [Despliegue de Contenedores](./sesion3/#despliegue-de-contenedores)
    + [Instalación de DOCKER en tiempo de instanciación](./sesion3/#instalaci-n-de-docker-en-tiempo-de-instanciaci-n)
    + [Trabajo con DOCKER: Gestión de contenedores](./sesion3/#trabajo-con-docker--gesti-n-de-contenedores)
      - [Parametrización de contenedores con DOCKER](./sesion3/#parametrizaci-n-de-contenedores-con-docker)
    + [Creación de contenedores enlazados](./sesion3/#creaci-n-de-contenedores-enlazados)
      - [Maquina Virtual 1](./sesion3/#maquina-virtual-1)
      - [Maquina Virtual 2](./sesion3/#maquina-virtual-2)
    + [Instalación del servicio completo OWNCLOUD + MYSQL](./sesion3/#instalaci-n-del-servicio-completo-owncloud---mysql)


# Sesión 4:

Tabla de contenido:

  * [Requisitos iniciales](./sesion4/#requisitos-iniciales)
  * [Credenciales y acceso inicial](./sesion4/#credenciales-y-acceso-inicial)
  * [Acceso vía WEB](./sesion4/#acceso-v-a-web)
  * [Acceso vía SSH](./sesion4/#acceso-v-a-ssh)
  * [Despliegue y gestión de servicios de autenticación de usuarios](./sesion4/#despliegue-y-gesti-n-de-servicios-de-autenticaci-n-de-usuarios)
    + [Entrenando con LDAP](./sesion4/#entrenando-con-ldap)
    + [LDAP Basics](./sesion4/#ldap-basics)
    + [Objetos y clases](./sesion4/#objetos-y-clases)
    + [Atributos](./sesion4/#atributos)
    + [Entradas](./sesion4/#entradas)
    + [DIT](./sesion4/#dit)
    + [Distinguished name (dn). Nombre distinguido.](./sesion4/#distinguished-name--dn--nombre-distinguido)
    + [Ejemplo de la estructura de LDAP](./sesion4/#ejemplo-de-la-estructura-de-ldap)
  * [Verificando el estado del directorio LDAP](./sesion4/#verificando-el-estado-del-directorio-ldap)
    + [Añadir un nuevo usuario](./sesion4/#a-adir-un-nuevo-usuario)
  * [Cambiar el Password de un usuario en LDAP](./sesion4/#cambiar-el-password-de-un-usuario-en-ldap)
    + [Modificando cuentas de usuario con LDAP: DELETE, MODIFY.](./sesion4/#modificando-cuentas-de-usuario-con-ldap--delete--modify)
  * [Añadir una UO a LDAP:](./sesion4/#a-adir-una-uo-a-ldap-)
  * [Buscando y encontrado dentro del DIT](./sesion4/#buscando-y-encontrado-dentro-del-dit)
  * [Ejercicio: Crear un servicio de directorio LDAP en contendor dentro de una MV](./sesion4/#ejercicio--crear-un-servicio-de-directorio-ldap-en-contendor-dentro-de-una-mv)



