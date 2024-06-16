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
    - [GUA IPv6](#gua-ipv6)
    - [Estructura IPv6 GUA](#estructura-ipv6-gua)
      - [Prefijo de enrutamiento global](#prefijo-de-enrutamiento-global)
      - [ID de subred](#id-de-subred)
      - [ID de interfaz](#id-de-interfaz)
    - [IPv6 LLA](#ipv6-lla)

- - -

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

- - -

## Longitud de prefijo IPv6

El **prefijo** de una dirección IPv4 se puede identificar mediante la máscara de subred. En **IPv6** no utiliza la notación decimal de máscara de subred. La longitud de prefijo se representa en notación de barra inclinada y se usa para indicar la porción de red de una dirección IPv6.

La **longitud de prefijo** puede ir de 0 a 128. La recomendada es /64.

![Prefijo IPv6](imagenes/prefijoipv6.png)

El **prefijo** o la porción de red de la dirección tiene 64 bits de longitud, dejando otros 64 bits para la ID de interfaz de la dirección.

La autoconfiguración de direcciones sin estado **SLAAC** utiliza 64 bits para el ID de la interfaz, también facilita la creación y gestión de subredes.

- - -

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

Las **direcciones locales únicas** con rango **fc00::/7 a fdff::/7** aún no se implementan regularmente. Se pueden usar **direcciones locales únicas** para dirigir dispositivos a los que no se debe acceder desde el exterior.

Las **direcciones locales únicas IPv6** tienen cierta similitud con las direcciones privadas RFC 1918 para IPv4, aunque las diferencias son:

- Las direcciones locales únicas se utilizan para el direccionamiento local dentro de un sitio o entre una cantidad limitada de sitios.
- Se pueden utilizar direcciones locales únicas para dispositivos que nuncan necesitarán acceder a otra red.
- Las direcciones locales únicas no se enrutan o traducen globalmente a una dirección IPv6 global.

### GUA IPv6

Las **direcciones IPv6 de difusión globales (GUA)** son globalmente únicas y enrutables en Internet IPv6. Estas direcciones son equivalentes a las direcciones IPv4 públicas.

La Autoridad de Asignación de Números de Internet (IANA) que asigna bloques de direcciones IPv6 a los cinco RIR. Actualmente, solo se están asignando GUAs con los primeros tres bir de **001** ó **2000::/3**.

![GUA IPv6](imagenes/guaipv6.png)

La figura muestra el rango de valores para el primer sexteto donde el primer dígito hexadecimal para las **GUA** disponibles actualmente comienza con un **2** o un **3**. La dirección **2001:odb8::/32** se reservó para fines de documentación, incluido el uso en ejemplos.

Dirección IPv6 con un **prefijo** de enrutamiento global /48 y un **prefijo** /64.

![GUA IPv6 con prefijo](imagenes/guaipv6b.png)

Las **GUA** tienen tres partes:

- Prefijo de enrutamiento global.
- ID de subred.
- ID de interfaz.

### Estructura IPv6 GUA

#### Prefijo de enrutamiento global

Es la porción de prefijo, o de red, de la dirección que asigna el proveedor a un cliente o a un sitio. Los ISP asignan un **prefijo de enrutamiento global /48** a sus clientes.

Por ejemplo: la dirección IPv6 2001:db8:acad::/48 tiene un prefijo de enrutamiento global, que indica, que los primeros 48 bits (*3 sextetos*) (2001:db8:acad) es cómo el ISP conoce este prefijo (red). Los dos puntos dobles (::) que siguen a la longitud del prefijo /48 significa que el resto de la dirección contiene todos los 0.

El tamaño del prefijo global determina el tamaño de la ID de subred.

#### ID de subred

El campo **ID de subred** es el área entre el **prefijo de enrutamiento global** y la **ID de interfaz**. IPv6 se diseña teniendo en cuenta la subred, utilizando la ID de subred, para identificar una subred dentro de su ubicación. Cuando mayor es la ID, más subredes habrá disponibles.

Las direcciones IPv6 suelen tener un prefijo de enrutamiento global /48. Usando una longitud de prefijo /64 típica, los primeros cuatro sextetos son para la porción de red de la dirección y el cuarto sexteto indica la ID de subred. Los cuatro sextetos restantes son para la ID de interfaz.

#### ID de interfaz

La **ID de interfaz IPv6** equivale a la porción de host de una dirección IPv4. Un único Host puede tener varias interfaces, cada una con una o más direcciones IPv6. Recomendándose en la mayoría de los casos utilizar subredes /64, lo que crea una ID de interfaz de 64 bits.

Una subred o prefijo /64 (*prefijo de enrutamiento global + ID de subred*) deja 64 bits para el ID de interfaz. Esto se recomienda para permitir que los dispositivos habilitados para **SLAAC** creen su propia ID de interfaz de 64 bits.

En IPv6 se pueden asignar las direcciones de host compuestas solo por ceros y unos a un dispositivo. La dirección *all-1s*, se puede utiliar porque las direcciones de difusión no se utilizan en IPv6. También se puede utilizar la dirección compuesta solo por ceros, aunque se reserva como una dirección **Anycast** de subred y enrutador, y se debe asignar solo a Routers.

### IPv6 LLA

Una **dirección local de enlace IPv6 (LLA)** permite que un dispositivo se comunique con otros dispositivos habilitados para IPv6 en el mismo enlace y sólo en ese enlace (subred). Los paquetes con un **LLA** de origen o destino no se pueden enrutar más allá del enlace desde el que se originó el paquete.

Mientras que la **GUA** no es un requisito, sin embargo, cada interfaz de red habilitada para IPv6 debe tener una **LLA**.

Si un **LLA** no se configura manualmente en una interfaz, el dispositivo creará automáticamente el suyo sin comunicarse con un servidor DHCP. Los hosts con IPv6 habilitado crean un **LLA** de IPv6 incluso si el dispositivo no tiene asignada una dirección IPv6 de difusión global. Esto permite que los dispositivos con IPv6 habilitado se comuniquen con otros dispositivos con IPv6 habilitado en la misma subred, incluyendo la puerta de enlace.

Las direcciones **LLA IPv6** están en el rango **fe80::/10** donde /10 indica, que los primeros 10 bits son *1111 1110 10xx xxxx*. El primer sexteto tiene un rango de *1111 1110 1**000 0000*** (fe80) a *1111 1110 10**11 1111*** (febf).

Por lo general, es el **LLA** del Router y no la **GUA** la que se usa, como puerta de enlace predeterminada para otros dispositivos en el enlace.

Hay dos maneras en que un dispositivo puede obtener una **LLA**:

- **Estáticamente**, el dispositivo es configura manualmente.
- **Dinámicamente**, el dispositivo crea su propia ID de interfaz usando valores generados aleatoriamente o usando el **Método de Identificador Único Extendido (EUI)**, que usa la dirección de control de acceso a medios MAC del cliente junto con bits adicionales.