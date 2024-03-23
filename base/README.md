# APKBUILDx VenenuX

![Alpine linux logo](https://alpinelinux.org/alpinelinux-logo.svg)

Es un repositorio de `APKBUILD` (recetas) para hacer paquetes para [Alpine linux](https://alpinelinux.org/)

Si detecta algun error en los APKBUILD por favor avisar en los [Issues](https://codeberg.org/alpine/alpine-apkbuilds/issues) 

Si deseas contribuir.. o como usar este repo favor leer [../documentos/README.md](../documentos/README.md)

### Que hay en este directorio

Aqui se colocan las recetas o `APKBUILD`S para paquetes comunmente que:

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

Si deseas contribuir.. o como usar este repo favor leer [../documentos/README.md](../documentos/README.md)

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
