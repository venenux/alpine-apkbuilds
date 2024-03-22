Alpine linux es originalmente una distro para dispositivos de redes salida de LEAF linux.. 
su simplicidad y rapidez (por ser simple) **ha gustado a sus usuarios y 
estos empezaron meterle paquetes de todo tipo.. creyendola equivocamente en una todo uso**.

Esto le dara a ud **la razon de porque no ve muchos paquetes en main que otros tienen**, 
es porque esta originalmente enfocado a servicios orientados a redes. Los paquetes 
que no son de este enfoque estan "todos apiñados" en un repositorio llamado "community".

Una **pista de esto es ver que paquetes como `kamailio`, `asterisk`, `php` 
esta muy al dia inclusive en versiones viejas como `3.6` y `3.7`.** Por ejemplo 
`lua` esta tanto la version 5.1, como 5.2 y 5.3, y php esta 7.4 y 8.3 desde hace mucho 
en los repositorios de alpine y si estan en comunidad mucho mas desde antes.

## Comenzando con APKS y APKBUILDS

Este lugar asume ud ya sabe lo necesario, para informacion de como empezar visite la web 
de la wiki alpine en https://venenux.github.io/alpine-espanol/#/recetas/alpine-recetas-hacer-paquetes-alpine-localmente 
o puede usar el repositorio de wiki https://codeberg.org/alpine/alpine-espanol/src/branch/master/recetas/alpine-recetas-hacer-paquetes-alpine-localmente.md

ESte la dara una guia de como usar este repo, **los comandos funcionan 
aun si su alpine tiene o no instalados paquetes, puede reejecutar estos 
comandos sin problema**.

#### Configuracion entorno abuild

Cada linea es un comando que puede ejecutarse, cada dos lineas significa que 
debe esperar a que termine el comando para ejecutar el siguente.

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
doas mkdir -p /var/cache/distfiles
doas chmod a+w /var/cache/distfiles
doas chgrp abuild /var/cache/distfiles
doas chmod g+w /var/cache/distfiles
abuild-keygen -a -i
mkdir /home/general/Devel
openssl genrsa -out /home/general/Devel/general@venenux.xxx.key.priv 2048
openssl rsa -in /home/general/Devel/general@venenux.xxx.key.priv -pubout -out /etc/apk/keys/general@venenux.xxx.key.pub
git config --global user.email "general@venenux.xxx"
git config --global user.name "generalvenenux"
git config --global pull.rebase=true,
```

#### Recompilando un paquete con abuild que ya esta en el repo

Esto es para cuando el paquete ya existe y lo quiere usar desde el repo

```bash
mkdir -p /home/general/Devel/

cd /home/general/Devel/ && git clone https://codeberg.org/alpine/alpine-apkbuilds

cd /home/general/Devel/alpine-apkbuilds/neofetch

abuild -r
```

> **NOTA** neofetch se usa como ejemplo si este es un paquete que ya existe en ESTE repositorio git

Una vez esto, crear un branch y subir a otro repo igual forkeado, despues enviar un pull request

#### Sacando un paquete desde Alpine con abuild y agregarlo al repo

Esto es para cuando el paquete ya existe o u lo tiene local, y lo agrega al repo

```bash
mkdir -p /home/general/Devel/

cd /home/general/Devel/ && git clone https://codeberg.org/alpine/alpine-apkbuilds

mkdir -p /home/general/Devel/alpine-apkbuilds/neofetch && cd /home/general/Devel/alpine-apkbuilds/neofetch

aria2c https://git.alpinelinux.org/aports/plain/community/neofetch/APKBUILD

abuild -r
```

> **NOTA** neofetch se usa como ejemplo pero es un paquete que ya existe en alpine y lo va agregar a el repo pero modificandolo

Una vez esto, crear un branch y subir a otro repo igual forkeado, despues enviar un pull request

#### Creando un paquete con abuild

Esto es para cuando el paquete no existe o usted lo tiene local, y lo agregara al repo

```bash
mkdir -p /home/general/Devel/

cd /home/general/Devel/ && git clone https://codeberg.org/alpine/alpine-apkbuilds

mkdir -p /home/general/Devel/alpine-apkbuilds/neofetch && cd /home/general/Devel/alpine-apkbuilds/neofetch

aria2c -o neofetch_7.0.1 https://github.com/dylanaraps/neofetch/archive/7.1.0.tar.gz

newapkbuild -f -d "neofetch package" -n neofetch -l MIT neofetch-7.0.1 neofetch_7.1.0.tar.gz

abuild -r
```

> **NOTA** neofetch se usa como ejemplo pero es un paquete que ya existe pero no esta aun empaquetado o se usara otro forma de empaquetar

Una vez esto, crear un branch y subir a otro repo igual forkeado, despues enviar un pull request

## Agregar paquetes

Los paquetes se agregan en un branch llamado "main-contrib" en main solo los miembros del repo pueden agregar directamente

hay 3 directorios, "base", "media" y "system", en el primero paquetes que son simples y no requieren dependencias tanto en ejecucion como en compilacion, 
adicional alli van tambien paquetes que se necesitaran tanto en media como en system. En media va todo lo grafico, juegos y emuladores, 
y en system va desarrollo, produccion y servicios.

# mas informacion

- Author
  - Sodomon as https://codeberg.org/alpine/alpine-espanol/src/branch/master/recetas/alpine-recetas-hacer-paquetes-alpine-localmente.md

Licencia CC-BY-SA-NC solo los autores de estos contenidos pueden permitir fuera de estas reglas de licencias.

# Vease tambien

* Indice general [../README.md](../README.md)
