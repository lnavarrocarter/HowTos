Git Techniques
==============

Ténicas Git para los machos alfa.

[[TOC]]

## Desplegar (deploy) en VPS

#### Requerimientos

1. VPS con Ubuntu/Debian, configurada adecuadamente con Apache, MySQL, PHP, Composer, Node y otros
2. Git en máquina de desarrollo y VPS
3. Un VirtualHost de Apache ya configurado. Para esto, Webmin y Virtualmin son muy buenas opciones.

#### Configurando la VPS

En la VPS, creamos una carpeta para guardar nuestras repos con `mkdir /var/repos && cd /var/repos`.

Luego, creamos una carpeta para el repositorio específico que queremos desplegar con `mkdir [tu-repo].git && cd [tu-repo].git`

Luego, inicializamos un nuevo repositorio utilizando `git init --bare`. El `--bare` flag es para crear un repositorio especial que no es para trabajar, sino que recibe pushes.

Luego, creamos un git hook que va a decirle a git que haga algo luego de recibir un push. Hacemos esto con `cd hooks && sudo nano post-receive`. Vamos a añadir el siguiente código:

```
#!/bin/sh
git --work-tree=[ubicacion/de/tus/archivos/html/para/servir] --git-dir=/var/repos/[tu-repo].git checkout -f
```

`--work-tree` le dice a git dónde copiar los archivos recibidos del push. Por ello, debemos apuntarlo al DocumentRoot del proyecto en el que estamos trabajando. `--git-dir` simplemente le dice a git cuál es el directorio del repositorio bare que ha recibido el push. Además, si es que tu proyecto lo necesita, puedes añadir la siguiente línea al final `composer install --no-dev`.

Luego de esto, debemos hacer el post-recieve ejecutable con `sudo chmod +x post-receive`

#### Configurando el entorno local para hacer push a la VPS

Luego de que ya hemos realizado todo lo anterior, ahora debemos aregar un `remote` a nuestro repositorio local. Tal como existe un `remote origin` que es el lugar donde recibimos los push de nuestro source code, podemos agregar un nuevo remote para decirle a git que pusheé cambios a otro repositorio. Esto se hace simplemente utilizando `git remote add production ssh://[usuario-vps]@[fqdn-vps]/var/repos/[tu-repo].git`. Para que esto funcione bien, debes tener configurado el acceso a tu VPS de ese usuario por medio de SSH Keys. Hay muchos tutoriales que enseñan cómo hacer eso en internet.

Ahora, puedes pushear tus cambios utilizando `git push production master`