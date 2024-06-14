# IPv6 Conceptos y Ejemplos

- [IPv6 Conceptos y Ejemplos](#ipv6-conceptos-y-ejemplos)
  - [Introducción al reenvío de Host](#introducción-al-reenvío-de-host)
    - [Visualizar tablas de enrutamiento de Host](#visualizar-tablas-de-enrutamiento-de-host)
    - [Enrutamiento Estático](#enrutamiento-estático)
    - [Enrutamiento Dinámico](#enrutamiento-dinámico)
  - [Unidifusión, Multidifusión, Anycast](#unidifusión-multidifusión-anycast)
  - [Longitud de prefijo IPv6](#longitud-de-prefijo-ipv6)
  - [Tipos de direcciones de unidifusión IPv6](#tipos-de-direcciones-de-unidifusión-ipv6)
    - [Dirección de Difusión Global (GUA)](#dirección-de-difusión-global-gua)
    - [Dirección Local de Enlace (LLA)](#dirección-local-de-enlace-lla)

IPv6 conceptos básicos y ejemplos de dos cursos: CISCO y PROFE24.

## Introducción al reenvío de Host

En IPv4 o IPv6 los paquetes se crean en el host de origen. Este host, dirige los paquetes al host de destino. Para ello se crean las tablas de enrutamiento.

La función de la **capa de red** es dirigir los paquetes entre los hosts:

- **A si mismo**. Un host puede hacerse ping a sí mismo, al enviar un paquete a una dirección **IPv4 127.0.0.1** o a una **IPv6 ::1**, a la que se hace referencia como la interfaz de bucle invertido.
- **Host local**. Este es un host de destino que se encuentra en la misma red local que el host emisor. Los host de origen y destino comparten la misma dirección de red.
- **Host remoto**. Este es un host de destino en una red remota. Los host de origen y destino con comparten la misma dirección de red.

En **IPv4**, el dispositivo de origen utiliza su propia máscara de subred junto con su propia dirección IPv4 y la dirección IPv4 de destino para realizar esta determinación.

En **IPv6** el enrutador local anuncia la dirección de red local (prefijo) a todos los dispositivos de la red.

### Visualizar tablas de enrutamiento de Host

En un OS con W10/11, el comando `route print` o `netstat -r` se usa para mostrar la tabla de enrutamiento del host. La tabla muestra tres secciones realizadas con las conexiones de red TCP/IP actuales:

- Lista de interfaces. Enumera la dirección de control de acceso a medios MAC y el número de interfaz asignado de cada interfaz con capacidad de red en el host, incluidos los adaptadores Ethernet, WiFi.
- Tabla de enrutamiento IPv4. Es una lista de todas las rutas IPv4 conocidas, incluidas las conexiones directas, las redes locales y las rutas locales predeterminadas.
- Tabla de enrutamiento IPv6. Es una lista de todas las rutas IPv6 conocidas, incluidas las conexiones directas, las redes locales y las rutas locales predeterminadas.

La tabla de enrutamiento almacena tres tipos de enlaces de ruta:

- Redes conectadas directamente. Enlaces de ruta de red activas.
- Redes remotas. Enlaces de ruta de red están conectadas a otros Routers.
- Ruta predeterminada. Los Routers incluyen un enlace de ruta predeterminada, una puerta de enlace predeteminada.

El comando para visualizar la tabla de enrutamiento es `show ip route` en modo EXEC privilegiado. Entre las fuentes de ruta comunes se incluyen:

- **L**. Dirección IP de interfaz local conectada directamente.
- **C**. Red conectada directamente.
- **S**. La ruta estática fue configurada manualmente por un Administrador.
- **O**. Open Shortest Path First (OSPF).
- **D**. Enhanced Interior puerta de enlace predeterminada Routing Protocol (EIGRP).

### Enrutamiento Estático

Son enlaces de ruta que se configuran manualmente. La ruta estática incluye la dirección de red remota y la dirección IP del enrutador de salto siguiente.

### Enrutamiento Dinámico

Permite a los Routers aprender automáticamente sobre redes remotas, incluida una ruta predeterminada, de otros Routers. Se comparte la información mediante el protocolo dinámico (OSPF, EIGRP) actualizando las tablas. Este protocolo hará automáticamente lo siguiente:

- Detectar redes remotas.
- Mantener información de enrutamiento actualizada.
- Elija el mejor camino hacia las redes de destino.
- Intente encontrar una nueva mejor ruta si la ruta actual ya no está disponible.

## Unidifusión, Multidifusión, Anycast

Existen tres categorías de direcciones IPv6:

- **Difusión**. Una dirección de difusión IPv6 identifican de forma exclusiva una interfaz en un dispositivo con IPv6 habilitado.
- **Multidifusión**. Una dirección de multidifusión IPv6 se utiliza para enviar un único paquete a varios destinos.
- **Anycast**. Una dirección IPv6 *anycast* es cualquier dirección de difusión que se puede asignar a varios dispositivos.

## Longitud de prefijo IPv6

El **prefijo** de una dirección IPv4 se puede identificar mediante la máscara de subred. En **IPv6** no utiliza la notación decimal de máscara de subred. La longitud de prefijo se representa en notación de barra inclinada y se usa para indicar la porción de red de una dirección IPv6.

La **longitud de prefijo** puede ir de 0 a 128. La recomendada es /64.

![Prefijo IPv6](imagenes/prefijoipv6.png)

El **prefijo** o la porción de red de la dirección tiene 64 bits de longitud, dejando otros 64 bits para la ID de interfaz de la dirección.

La autoconfiguración de direcciones sin estado **SLAAC** utiliza 64 bits para el ID de la interfaz, también facilita la creación y gestión de subredes.

## Tipos de direcciones de unidifusión IPv6

Las direcciones IPv6 de difusión identifican de forma exclusiva una interfaz. Existen diferentes tipos de **direcciones de difusión IPv6 Unicast**.

- Unidifusión global.
- Link-Local.
- Loopback (**::1/128**)
- Dirección sin especificar (**::**)
- Local única (fc00::/7 - fdff::/7)
- IPv4 integrada.

### Dirección de Difusión Global (GUA)

Las **GUA** pueden configurarse estáticamente o asignarse dinámicamente.

Estas son similares a las direcciones IPv4 públicas. Son enrutables de Internet globalmente exclusivas.

### Dirección Local de Enlace (LLA)

Se requiere para cada dispositivo habilitado para IPv6. Los **LLA** se utilizan para comunicarse con otros dispositivos en el mismo enlace local en una subred. No se pueden enrutar más allá del enlace.