# formacion-angular

Plantilla para el curso impartido por la empresa **Mimacom**.

# Acerca de

El objetivo de la formación es adquirir conocimientos suficientes para abordar tareas de desarrollo en proyectos con tecnología **Angular**. Se impartirá tanto formación teórica como casos prácticos, mostrando la configuración de los proyectos sobre el IDE de desarrollo **VSCode** y los aspectos más relevantes del lenguaje, Javascript y Typescript.

Este proyecto está configurado para ser editado en **[VSCode](https://code.visualstudio.com/)** y sus extensiones para **[Desarrollo Remoto](https://code.visualstudio.com/docs/remote/remote-overview)**.

Concretamente el desarrollo se realiza en un contenedor **Docker** con una imagen que, en su primera ejecución, instala **Node 12 (erbium)**.

# Prerequisitos

*  Tener instalado **Git**
*  Tener instalado **Docker**
*  Tener instalado **VSCode**

## Instalación de **Git**

*  Actualiza la lista de paquetes disponibles:

`sudo apt-get update`

*  Instala GIT:

`sudo apt-get install git`

## Instalación de **Docker**

### Ubicación del software

El software de instalación se encuentra disponible en la página web del [producto](https://download.docker.com/linux/static/stable/) así como en el Almacén de Software de la [Unidad de Operaciones de Treelogic](smb://almacen/operaciones/), en la carpeta **/software/system/docker**.

### Proceso de instalación

1. Extrae el archivo. En el momento de escribir estas instrucciones, la última versión disponible es la 19.03.8:

`tar xzvf docker-19.03.8.tgz`

2. Copia los binarios a **/usr/bin**

`sudo cp docker/* /usr/bin`

3. Crea el grupo **docker**

`sudo groupadd docker`

4. Añade tu usuario al grupo **docker**

`sudo usermod -aG docker $USER`

5. Cierra tu sesión y vuelve a abrirla de modo que tu pertenencia a los grupos sea re-evaluada

> A partir de Ubuntu 18.04, cuando sales de la sesión de Gnome, algunos de los procesos del usuario no finalizan instantáneamente y si vuelves a iniciar sesión se recupera la que ya tenías (y por tanto, no se re-evalúan los grupos). Para asegurarte, lo mejor es que reinicies el equipo.

### Comprobación de la instalación
1. Arranca el demonio **dockerd**

`sudo dockerd &`

2. Comprueba que la instalación ha sido correcta

`docker run hello-world`

> Este comando descarga una imagen de prueba y la ejecuta en un contenedor. Cuando el contenedor se ejecuta, imprime un mensaje de información y finaliza.

3.- Finaliza el demonio que hemos arranacado manualmente

`sudo killall dockerd`

### Integración con systemd
Ahora que ya está instalado y hemos comprobado que funciona correctamente seguramente querremos integrar **Docker** con **systemd**.

1. Instala los dos ficheros de unidad (**docker.service** y **docker.socket**) que tienes en el almacén o en el [repositorio de github](https://github.com/moby/moby/tree/master/contrib/init/systemd) de docker.

`sudo cp docker.service /etc/systemd/system`

`sudo cp docker.socket /etc/systemd/system`

2. Fuerza la recarga de la configuración de **systemd**

`sudo systemctl daemon-reload`

3. Arranca el servicio **docker** manualmente

`sudo systemctl start docker`

4. Configuramos el servicio para que se inicie automáticamente al arrancar el equipo

`sudo systemctl enable docker`

### Autocompletado en línea de comandos

1. Copia el fichero de script de autocompletado (**docker**) que tienes en el almacén o en el [repositorio de github](https://github.com/docker/docker-ce/tree/master/components/cli/contrib/completion/bash) de docker en **/etc/bash_completion.d/**

`sudo cp docker /etc/bash_completion.d/docker`

## Instalación del entorno de desarrollo

### Ubicación del software

El software de instalación se encuentra disponible en la página [web del producto](https://code.visualstudio.com/download) así como en el [Almacén de Software de la Unidad de Operaciones de Treelogic](smb://almacen/operaciones/), en la carpeta **/software/programming/javascript/vscode**.

Instalaremos la última versión de Visual Studio Code disponible.  En el momento de escribir estas instrucciones, la última versión disponible es la 1.43.0:

* code-stable-1583783449.tar.gz (Linux 64 bits)

> En Linux será necesario instalar el paquete **libgconf-2-4** (*error while loading shared libraries: libgconf-2.so.4: cannot open shared object file: No such file or directory*) si éste no se encuentra previamente instalado.

La instalación consiste en descomprimir el fichero correspondiente en la ubicación deseada (por ejemplo **/software/VSCode-linux-x64**)

### Preparación del entorno de desarrollo

Por defecto, la configuración del usuario se guardará en **$HOME/.config/Code/User/settings.json** y las extensiones instaladas se almacenarán en **$HOME/.vscode/extensions**.

### Ejecución del entorno de desarrollo

Para arrancar el entorno de desarrollo basta con añadir al PATH la ruta correspondiente:

`export PATH=/software/VSCode-linux-x64:$PATH`

> Para que sea permanente, lo podemos añadir al fincar del fichero **$HOME/.bashrc**

Una vez configurado podemos arrancar el VSCode mediante el comando `code`

### Extensiones

Una vez instalado podemos proceder a instalar las [extensiones](https://code.visualstudio.com/docs/editor/extension-gallery) que requiere nuestro proyecto que, como ya hemos comentado, se guardarán en la carpeta **extensions** de nuestro directorio de configuración. Para este proyecto debemos instalar las siguientes extensiones:

* Desarrollo en remoto

[Remote Development](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack) (ms-vscode-remote.vscode-remote-extensionpack)

* Git

[GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) (eamodio.gitlens)

# Descarga del proyecto y puesta en marcha

* Clona en local el proyecto

`git clone http://gitlab.treelogic.local/formacion/angular/formacion-angular.git`

> Si tienes configuradas tus [claves SSH en GitLab](https://docs.gitlab.com/ee/ssh/), lo cual es preferible, utiliza `git clone git@gitlab.treelogic.local:formacion/angular/formacion-angular.git` en su lugar.

* Sitúate en el directorio creado

`cd formacion-angular`

* Antes de abrir el proyecto en el contedor usando **VSCode**, es necesario adaptar los siguiente parámetros en el fichero **./devcontainer/devcontainer.json**:
*  **runArgs**
    *  **"-u", "1000:1000"**: 1000:1000 se debe cambiar por tu UID, GID.  Para conocerlos puedes ejecutar el comando `echo "$(id -u):$(id -g)"` en una consola.
    * **"--group-add", "1001"**: 1001 se debe cambiar por el GID del grupo **docker**.  Para conocerlo puedes ejecutar el comando `cut -d: -f3 < <(getent group docker)` en una consola.
    * **"--mount", "type=bind,source=/usr/bin/docker,target=/home/user/.local/bin/docker"**: Asegúrate que **/usr/bin/docker** es la ruta correcta al binario de tu instalación de docker local.  Si no es así, actualíza dicha ruta.

* Además, deben existir los siguientes directorios en tu directorio **HOME** (si no existen, créalos):

    *  **.nvm/.cache**: Se usa para compartir las distribuciones descargadas de Node entre el host y el contenedor.
    *  **.npm/_cacache**:  Se usa para compartir la caché de paquetes de Node entre el host y el contenedor.
    *  **.ssh**: Se usa para compartir las credenciales ssh entre el host y el contenedor.

* También debe existir el siguiente fichero en tu directorio **HOME** (si no existe, créalo):
    *  **.gitconfig**: Se usa para compartir la configuración de git entre el host y el contenedor.

* Arranca el Visual Studio Code

`code .`

El plugin de desarrollo remoto reconocerá que la carpeta contiene configuración para el desarrollo en un contenedor y nos preguntará si deseamos volver a abrirlo en un contenedor, a lo que responderemos afirmativamente.

# Primeros pasos con Angular

* Instalación del Command Line Interface de Angular en el directorio actual

`npm install @angular/cli --no-save`

* Creación de una aplicación Angular en el directorio actual

`ng new appName --prefix tree --style scss --skip-git --directory ./`

> Es posible que el Angular CLI se queje de la existencia de los ficheros .gitignore  y README.md en el directorio actual. De ser así, podemos eliminar temporalmente dicho fichero, ejecutar de nuevo el comando y, una vez creado el proyecto, sustituir dichos ficheros que ha generado el CLI de Angular por los del repositorio.
