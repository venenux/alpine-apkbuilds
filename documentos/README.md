# Como contribuir y usar este repositorio

Debe cunmplir con tes aspectos, los cuales se presentan en secciones:

Este documento esta inspirado en https://sodomon.gitlab.io/post/guia-abuild/ Realizado por @sodomon 

## 1 - saber usar git y forks en codeberg

El procedimiento es simple, debes tener como basico 3 cosas en tu linux, que 
son el navegador web, el programa git y las utilidades de editor, ya que compilar 
y probar el paquete no es necesario para agregar, corregir o quitar algo, pero 
puede ser crucial si necesitas corroborar funcione (leer la segunda seccion).

* Ve a Codeberg y tener una cuenta, sino registrate usando la de gitlab o github.
* Al iniciar sesion, ve a nuestro repo https://codeberg.org/alpine/alpine-apkbuilds
* Debes forkear nuestro repo https://codeberg.org/repo/fork/43677
* Despues deber editar en la interfaz el archvo que deseas mejorar
    * Opcionalmente puedes uan vez forkeado clonar con git el repo y editarlo en tu pc
* Terminado de editar debes guardar en tu fork pero usando una nueva rama
    * Si clonaste debes crear una rama nueva donde guardaras estos cambios
* Despues de todo esto en tu fork se presentara un nuevo boton verde para enviar
* Al pulsar el boton de "Pull Request", se enviara tu propuesta a nuestro repo.

Despues debes esperar a que se coloquen comentarios en tu propuesta, para 
correcciones. Para saber como operar con los empaquetamiento.. lee las secciones:

## 2 - Anotaciones sobre APK y ABUILD

Aunque se asume lo necesario se le resume aqui, pero para informacion mucho mas 
emplia debe visite https://venenux.github.io/alpine-espanol/#/recetas/alpine-recetas-hacer-paquetes-alpine-localmente 
o puede usar el repositorio de wiki https://codeberg.org/alpine/alpine-espanol/src/branch/master/recetas/alpine-recetas-hacer-paquetes-alpine-localmente.md

### Configuracion entorno abuild

Cada linea es un comando que puede ejecutarse, cada dos lineas en blanco significa 
que debe esperar a que termine el comando para ejecutar el siguente.

```bash
apk add shadow shadow-doc shadow-uidmap bash bash-doc bash-dev \
 doas doas-doc doas-sudo-shim coreutils coreutils-doc tree tree-doc \
 man-db man-pages zlib zlib-doc wget wget-doc curl curl-doc aria2 aria2-doc \
 sed sed-doc lsof lsof-doc less less-doc groff groff-doc gawk gawk-doc \
 zip zip-doc p7zip p7zip-doc xz xz-doc tar tar-doc file file-doc
 
 abuild abuild-rootbld abuild-doc build-base gcc-doc make-doc patch-doc \
 arch-install-scripts arch-install-scripts-doc lzip-doc tar-doc zlib-doc 
 apk-tools-doc alpine-sdk git git-doc

useradd -m -U -c "" -G abuild,wheel,input,disk,floppy,cdrom,dialout,audio,video,lp,netdev,games,users,ping general

cat >  /etc/doas.d/general.conf << EOF
general ALL=(ALL) ALL
EOF

for u in $(ls /home); do for g in abuild disk lp floppy audio cdrom dialout video lp netdev games users ping; do addgroup $u $g; done;done
```

Ahora cierre la sesión de la cuenta de root, e inicie sesión como `general`. 
A partir de aquí todo se puede hacer en una cuenta de usuario normal, y las 
operaciones que requieren privilegios de superusuario se pueden hacer con `doas`.

```
mkdir -m 775 -p /home/general/Devel
git config --global pull.rebase=true
git config --global ssh.postBuffer 2000000000
git config --global http.postBuffer 2000000000
git config --global https.postBuffer 2000000000
doas mkdir -m 775 -p /var/cache/distfiles
doas chgrp abuild /var/cache/distfiles
doas sed -i 's|export CFLAGS\s*=.*|export CFLAGS="-O2"|g' /etc/abuild.conf
doas sed -i 's|SRCDEST\s*=.*|SRCDEST=/var/cache/distfiles|g' /etc/abuild.conf
doas mkdir -m 775 -p /home/general/Devel/packages
doas chown general:abuild /home/general/Devel
doas sed -i 's|REPODEST\s*=.*|REPODEST=\$HOME/Devel/packages/|g' /etc/abuild.conf
git config --global user.email "general@venenux.xxx"
git config --global user.name "generalvenenux"
doas sed -i 's|.*PACKAGER\s*=.*|PACKAGER="generalvenenux <general@venenux.xxx>"|g' /etc/abuild.conf
doas sed -i 's|.*MAINTAINER\s*=.*|MAINTAINER="\$PACKAGER"|g' /etc/abuild.conf
abuild-keygen -a -i -n
```

La secuencia de comandos es imprescindible en el mismo orden, asi el ultimo 
comando ejecutara correctamente las llaves publica y privada. OJO depende de que 
coloque bien su correo y usuario cambiando "general@venenux.xxx".

### Recompilando un paquete con abuild que ya esta en el repo

Esto es para cuando el paquete ya existe y lo quiere usar desde el repo

```bash
mkdir -p /home/general/Devel/ && cd /home/general/Devel

git clone https://codeberg.org/alpine/alpine-apkbuilds

cd /home/general/Devel/alpine-apkbuilds/base/neofetch

echo "aqui editar los archivos presentes y salvar"

abuild -r
```

> **NOTA** neofetch se usa como ejemplo SOLO SERVIRA SI YA TENEMOS ESTE PAQUETE EN EL GIT

Una vez esto, crear un branch y subir a otro repo igual forkeado, despues enviar un pull request

### Sacando un paquete desde Alpine con abuild y agregarlo al repo

Esto es para cuando el paquete ya existe o u lo tiene local, y lo agrega al repo

```bash
mkdir -p /home/general/Devel/ && cd /home/general/Devel

git clone https://codeberg.org/alpine/alpine-apkbuilds

mkdir -p /home/general/Devel/alpine-apkbuilds/base/neofetch && cd /home/general/Devel/alpine-apkbuilds/base/neofetch

aria2c https://git.alpinelinux.org/aports/plain/community/neofetch/APKBUILD

echo "aqui editar los archivos presentes y salvar"

abuild -r
```

> **NOTA** neofetch se usa como ejemplo pero SI YA LO TENEMOS LE FALLARA ESTOS COMANDOS

Una vez esto, crear un branch y subir a otro repo igual forkeado, despues enviar un pull request

### Creando un paquete local con abuild

Esto es para cuando el paquete no existe o usted lo tiene local, y lo agregara al repo

```bash
mkdir -p /home/general/Devel/ && cd /home/general/Devel

git clone https://codeberg.org/alpine/alpine-apkbuilds

cd /home/general/Devel/alpine-apkbuilds/base

newapkbuild -f -d "neofetch packages" -n neofetch -l MIT https://github.com/dylanaraps/neofetch/archive/7.1.0.tar.gz

cd /home/general/Devel/alpine-apkbuilds/base/neofetch
echo "aqui debe editar mcuhos archivos para que funcione"

aria2c -o neofetch-7.0.1 https://github.com/dylanaraps/neofetch/archive/7.1.0.tar.gz

abuild checksum

abuild -r
```

> **NOTA** neofetch se usa como ejemplo PERO SOLO SERVIRA SI NO TENEMOS ESTE PAQUETE EN EL REPO

Una vez esto, crear un branch y subir a otro repo igual forkeado, despues enviar un pull request

### Creando un paquete directo internet con abuild

Esto es para cuando el paquete lo toma directo desde internet, y lo agregara al repo

```bash
mkdir -p /home/general/Devel/ && cd /home/general/Devel

git clone https://codeberg.org/alpine/alpine-apkbuilds

cd /home/general/Devel/alpine-apkbuilds/base

newapkbuild -f -d "neofetch packages" -n neofetch -l MIT https://github.com/dylanaraps/neofetch/archive/7.1.0.tar.gz

cd /home/general/Devel/alpine-apkbuilds/base/neofetch
echo "aqui debe editar mcuhos archivos para que funcione"

abuild checksum

abuild -r
```

> **NOTA** neofetch se usa como ejemplo PERO SOLO SERVIRA SI NO TENEMOS ESTE PAQUETE EN EL REPO

Una vez esto, crear un branch y subir a otro repo igual forkeado, despues enviar un pull request

## 3 - Como y donde agregar paquetes

Si realizaste el procedimeinto descrito en el primer seccion de este documento, 
ahora debes saber donde colocar tu paquete, y como hacerlo, el como hacerlo es 
descrito en la segunda secion, todo lo previo a esta misma

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

### binarios compilados

En esta guia se configura para que los encuentre en `/home/general/Devel/packages/`

# mas informacion

- Authores
  - Sodomon as https://codeberg.org/alpine/alpine-espanol/src/branch/master/recetas/alpine-recetas-hacer-paquetes-alpine-localmente.md
  - Mckaygerhard : ajustes y mejoras, correccion de comandos faltantes, paquetes faltants para no parir en la distro que siempre la falta algo

Licencia CC-BY-SA-NC solo los autores de estos contenidos pueden permitir fuera de estas reglas de licencias.

# Vease tambien

* Indice general [../README.md](../README.md)
Este documento esta inspirado en https://sodomon.gitlab.io/post/guia-abuild/ Realizado por @sodomon 
