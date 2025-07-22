# APKBUILDx VenenuX

![Alpine linux logo](https://alpinelinux.org/alpinelinux-logo.svg)

Es un repositorio de `APKBUILD` (recetas) para hacer paquetes para [Alpine linux](https://alpinelinux.org/)

Si detecta algun error en los APKBUILD por favor avisar en los [Issues](https://codeberg.org/alpine/alpine-apkbuilds/issues) 

Si deseas contribuir.. o como usar este repo favor leer [../documentos/README.md](../documentos/README.md)

## Listado de paquetes

| nombre fuente  | paquetes generados | fuentes y APKBUILD |
| ---------------------------- | ----------------------------- | ------------------------------------------- |
| annobin                      | annobin annobin-doc           | [annobin](annobin/PKGBUILD)                 |

## Pre requisitos para que los paquetes esten aqui

Aqui se colocan las recetas o `APKBUILD`S para paquetes comunmente que:

* **Tiene mas dependencias que las que ya estan en alpine** por lo que las buscara 
en el directorio "base" de nuestro repo si necesita otra mas.
* **Es necesario o una dependencia para otros paquetes** por lo que sera necesario 
que este incluido pero que como no cumple con las dependencias faciles o simples 
se incluye aqui, ejemplos de esto es librerias.
* **Es un paquete que se necesita para servidores o desarrollo** por ejemplo las 
famosas php, apache2, nodejs etc

Si deseas contribuir.. o como usar este repo favor leer [../documentos/README.md](../documentos/README.md)

### Donde consiguo los paquetes listo para instalar?

Los resultados de las recetas se estara subiendo a github (para consumirles a 
ellos y no darle carga a codeberg/gitlab), si! vamos a llenarles a maycosoft 
su trasero y que consumamos gratis sus productos.

Revisa [../README.md](../README.md) para como usar nuestros repos

### Contactos

- 📱 Telegram https://t.me/alpine_linux
  - 🇨🇴 https://t.me/alpine_linux_espanol
  - 📡 https://t.me/latam_programadores
- Matrix
  - 👥 https://matrix.to/#/#alpine-linux-espanol:matrix.org
