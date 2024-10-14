# IPServices & Atacks

Bienvenidos a la guia de seguridad de activos realizada gracias al [curso](https://skillsforall.com/es/course/endpoint-security?courseLang=es-XL) de Cisco de Ciberseguridad 

## ARP

El **Protocolo de Resolución de Direcciones** (Address Resolution Protocol) es un protocolo utilizado en las redes locales para mapear direcciones IP a direcciones MAC. 

Las direcciones MAC son únicas para cada dispositivo en una red, y **ARP permite que los dispositivos se comuniquen entre sí dentro de una red local** (LAN) al **asociar una dirección IP** **con** la dirección **MAC** correspondiente. 

<img align="center" src="/img/1ºimagenn.PNG"  />

### Vulnerabilidades de ARP

- **Ausencia de autenticación**: ARP confía ciegamente en las respuestas ARP que recibe, sin verificar si la información es legítima.
- **Caché ARP sin protección**: Las entradas en la caché ARP pueden ser modificadas por cualquier dispositivo en la red, lo que permite que un atacante sobrescriba asociaciones IP-MAC con información falsa.
- **Susceptibilidad a ataques locales**: Debido a que ARP solo se utiliza en redes locales, cualquier dispositivo conectado a la misma red puede lanzar un ataque ARP sin requerir acceso privilegiado.

#### ARP Poisoning

El **ARP Poisoning** (envenenamiento ARP) es una técnica utilizada por los atacantes para modificar las tablas ARP de los dispositivos dentro de una red. El objetivo es engañar a los dispositivos para que asocien una dirección IP legítima con la dirección MAC del atacante, permitiéndole interceptar, modificar o redirigir el tráfico.

El proceso es el siguiente:

- **Escaneo de la red**: El atacante primero identifica los dispositivos en la red y sus direcciones IP y MAC.

- **Envío de mensajes falsos**: El atacante envía mensajes ARP falsificados a los dispositivos, haciéndoles creer que la dirección MAC del atacante está asociada a una dirección IP legítima, como la del router o de otro equipo.

- **Engaño en la tabla ARP**: Los dispositivos afectados guardan esta información falsa en su tabla ARP, por lo que ahora envían sus datos al atacante en lugar de al dispositivo original.

- **Intercepción del tráfico**: El atacante recibe los datos destinados al dispositivo legítimo y puede:

  - Leerlos o modificarlos.

  - Enviarlos al verdadero destino para no ser descubierto.

------

## DNS

El **Sistema de Nombres de Dominio** (DNS) es un componente que traduce nombres de dominio, como *[www.ejemplo.com](http://www.ejemplo.com)*, a direcciones IP, permitiendo que los dispositivos se conecten entre sí de manera sencilla, si no fuese por este sistema los usuarios tendrían que introducir y conocer las IPs de cada web.

<img align="center" src="/img/2ºimagenn.PNG"  />

### Vulnerabilidades de DNS

Pueden ser explotadas por los atacantes para interrumpir servicios, robar información o realizar ataques avanzados, a continuacion veremos los mas comunes

#### **DNS Open Resolver Attacks** 

Es un servidor DNS que acepta consultas de cualquier dirección IP, en lugar de limitarse a un conjunto de usuarios confiables. Aunque los resolvers DNS abiertos son útiles, también pueden ser explotados.

**Proceso:**

- Los atacantes aprovechan estos servidores abiertos para realizar **amplificación DDoS (Denegación de Servicio Distribuido)**.
- El atacante envía consultas DNS pequeñas pero maliciosas, con la dirección IP del objetivo falsificada.
- El resolver DNS genera respuestas mucho más grandes y las envía a la víctima en lugar del atacante, lo que satura el ancho de banda de la víctima.

*Como consecuencia se puede interrumpir el servicio del objetivo al sobrecargarlo con grandes volúmenes de trafico*

#### **DNS Stealth Attacks** (Ataques Sigilosos de DNS)

Los atacantes comprometen el sistema DNS de manera que sea difícil de detectar, permitiéndoles controlar el tráfico DNS sin ser descubiertos.

**Proceso**

- Los atacantes comprometen un servidor DNS o manipulan sus registros de forma que las consultas legítimas se redirigen a direcciones IP controladas por el atacante.
- El tráfico es interceptado sin que las víctimas lo noten, y puede ser utilizado para el robo de datos, espionaje o redirección a sitios maliciosos.

*El ataque puede ser usado para llevar a cabo ataques de tipo man-in-the-middle (MITM) o para redirigir el tráfico a sitios de phishing sin que los usuarios lo noten.*

#### DNS Domain Shadowing Attacks (Ataques de Sombras de Dominio DNS)

El **domain shadowing** es un tipo avanzado de ataque donde los atacantes comprometen cuentas de registradores de dominio legítimos para crear subdominios maliciosos sin que el propietario del dominio lo sepa.

**Proceso**

- El atacante gana acceso a la cuenta del propietario del dominio (por phishing o credenciales robadas).
- Crea subdominios bajo el dominio legítimo, pero estos subdominios están controlados por el atacante y son utilizados para actividades maliciosas.
- Los subdominios se utilizan para actividades de malware, phishing, o control de botnets.

*El dominio que es legitimo se ve comprometido y los subdominios maliciosos se usan para propagar ataques sin que el propietario del dominio original lo detecte fácilmente.*

#### DNS Tunneling Attacks (Ataques de Túnel DNS)

El **DNS Tunneling** es una técnica que utiliza el tráfico DNS para transferir datos entre un cliente y un servidor de manera encubierta, a menudo para eludir medidas de seguridad de la red.

**Proceso**

- Un atacante crea un túnel que utiliza las consultas y respuestas DNS para ocultar el tráfico de red.
- Las consultas DNS, que normalmente solo contienen información sobre la resolución de dominios, se usan para transportar datos (como archivos o comandos) entre el atacante y un servidor comprometido.
- Este tráfico puede pasar desapercibido porque los servidores DNS y firewalls suelen no analizar el contenido detallado de las consultas DNS.

*El DNS tunneling puede ser utilizado para exfiltrar datos de manera encubierta o para controlar remotamente malware en redes que bloquean otros tipos de tráfico.*

------

## DHCP(*Dynamic Host Configuration Protocol*)

El **Protocolo de Configuración Dinámica de Host** es un protocolo de red que asigna automáticamente direcciones IP y otros parámetros de red (como la puerta de enlace o DNS) a dispositivos conectados a una red. Este protocolo simplifica mucho la administración de redes.

<img align="center" src="/img/3ºimagenn.PNG"  />

### Vulnerabilidades

Aunque DHCP simplifica la gestión de redes, tiene vulnerabilidades que pueden ser explotadas por atacantes, especialmente en redes locales no protegidas. Una de las vulnerabilidades más comunes es el **DHCP Spoofing**

#### DHCP Spoofing

Es un ataque en el que un atacante introduce un servidor DHCP malicioso en la red para engañar a los dispositivos y hacer que se conecten a su red falsa en lugar de la red legítima.

**Proceso**

- **Inserción de un servidor DHCP falso**: El atacante conecta un dispositivo en la red (como un ordenador o un dispositivo móvil) y lanza un servidor DHCP falso.

- **Intercepción de peticiones DHCP**: Cuando un dispositivo nuevo se conecta a la red o cuando un dispositivo existente renueva su configuración de red, envía una solicitud DHCP (DHCP Discover) buscando una dirección IP. El servidor DHCP falso del atacante responde rápidamente antes que el servidor DHCP legítimo, proporcionando configuraciones controladas por el atacante.

- **Entrega de información falsa**: El servidor DHCP falso asigna direcciones IP incorrectas a los dispositivos, además de proporcionar información maliciosa, como:

  - **Puerta de enlace predeterminada falsa**: Redirige el tráfico de los dispositivos a un router controlado por el atacante.

  - **Servidores DNS falsos**: El atacante proporciona un servidor DNS malicioso que redirige el tráfico web a sitios de phishing o servidores de malware.

El atacante puede acceder a información confidencial como contraseñas o datos bancarios. Los usuarios pueden ser llevados a sitios web falsos sin darse cuenta. La red puede volverse inestable o inoperativa si las configuraciones DHCP incorrectas afectan a múltiples dispositivos.*
