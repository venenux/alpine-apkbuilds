# APKBUILDx VenenuX

![Alpine linux logo](https://alpinelinux.org/alpinelinux-logo.svg)

Es un repositorio de `APKBUILD` (recetas) para hacer paquetes para [Alpine linux](https://alpinelinux.org/)
tambien para configurar `abuild` para que puedas crear los paquetes a partir de 
los `APKBUILD` que aqui mostramos. **Contiene paquetes que no estan** en alpine 
o quiza nunca estaran, **aparte paquetes para versiones viejas de alpine**, 
tambien incluso **paquetes mejorados o superiores**

Si detecta algun error en los APKBUILD por favor avisar en los [Issues](https://codeberg.org/alpine/alpine-apkbuilds/issues) 

Si deseas contribuir.. o como usar este repo favor leer [documentos/README.md](documentos/README.md)

## Acerca Alpine linux

Alpine linux es originalmente una distro para dispositivos de redes salida 
de LEAF linux.. su simplicidad y rapidez (por ser simple) **ha gustado a sus usuarios y 
estos empezaron meterle paquetes de todo tipo.. creyendola equivocamente en una todo uso**.

Esto le dara a ud **la razon de porque no ve muchos paquetes en main que otros tienen**, 
es porque esta originalmente enfocado a servicios orientados a redes. Los paquetes 
que no son de este enfoque estan "todos apiñados" en un repositorio llamado "community".

### Porque tienen las recetas aqui?

Una **pista de esto es ver que paquetes como `kamailio`, `asterisk`, `php` 
esta muy al dia inclusive en versiones viejas como `3.6` y `3.7`.** Por ejemplo 
`lua` esta tanto la version 5.1, como 5.2 y 5.3, y php esta 7.4 y 8.3 desde hace mucho 
en los repositorios de alpine y si estan en comunidad mucho mas desde antes.

Estas son las caracteristicas que hacen que los paquetes los tengamos aqui:

* Los oficiales de alpine, incluso tienen la configuracion de los creadores 
originales, que no esta ni cerca de lo que alpine contiene. Un ejemplo, tienen 
el openbox pero las configuraciones asumen LXDE el cual no existe en alpine.
* No existen en alpine, ejemplo LXDE, este no existe en alpine, entonces aqui 
ofrecemos los archivos necesarios para que puedan compilarlos en alpine, si solo 
deseas los binarios listos para usar, deberas usar el instructivo de nuestra wiki
* Tenemos mejores parches y versiones mejoradas, por ejemplo mientras en alpine 
se empaqueta gitea, aqui ya teniamos forgejo listo apra usar en todas las versiones 
de alpine, por ejemplo la mejor version de alpine para i386 es la 3.12.

### Estructura de los APKBUILDs

Hay 3 directorios, los paquetes debe colocarlos segun funcionalidad y dependencias

### base

Colocar aqui si el paquete cumple con lo siguente:

* **No tiene complicadas al ser instalados**, como `neofetch` una vez instalado 
no necesita mas dependencias que `bash`, que tambien es un paquete base y al mismo 
tiempo es un paquete que esta en alpine en sus repos oficiales desde siempre, en 
cambio `minecraft` necesita al ser instalado varios componentes externos.
* **No tiene compile depends mas que las mismas en main de alpine**, ejem `neofecth` 
para compilar solo necesita `make`, que esta en main y no en comunity, pero en cambio
el paquete `orc` no puede estar en base, porque no esta en main lo que necesita para 
compilarse, sino en comunity.
* **Es un paquete que es necesario para otros**, ojo con esto, si se necesita para
compilar otro paquete en los otros directorios, pero no esta en alpine y cumple 
con las dos anteriores previas. Sino debe ir a el directorio `system`.

### system

Las mismas condicines anteriores pero relajadas, con la salvedad que:

* **Tiene mas dependencias que las que ya estan en alpine** por lo que las buscara 
en el directorio "base" de nuestro repo.
* **Es necesario o una dependencia para otros paquetes** por lo que sera necesario 
que este incluido pero que como no cumple con las dependencias faciles o simples 
se incluye aqui, ejemplos de esto es librerias.
* **Es un paquete que se necesita para servidores o desarrollo** por ejemplo las 
famosas php, apache2, nodejs etc

### media

Asumiremos que aqui se colocaran la mayoria ya que seran si:

* **Es un paquete de juegos, video, audio, internet, educacion**, que por lo 
mas general, no cumplira con las dos condiciones anteriores de los paquetes base, 
por ejemplo telegram debe estar en este directorio si ud lo empaquetase.
* **Es un paquete para un escritorio o interfaz grafica** como el escritorio 
que fue removido `LXDE` o una aplicacion que alpine no quiere como `anydesk`.

### Donde consiguo los paquetes listo para instalar?

Los resultados de las recetas se estara subiendo a github, si vamos a llenarles 
a maycosoft su trasero y que consumamos gratis sus productos.

TODO

### Contactos

- 📱 Telegram https://t.me/alpine_linux
  - 🇨🇴 https://t.me/alpine_linux_espanol
  - 📡 https://t.me/latam_programadores
- Matrix
  - 👥 https://matrix.to/#/#alpine-linux-espanol:matrix.org
