<div align="center">

# YOCTO

Resumen de trabajo final de ***Sistema operativo II***.<br>
Creado por: [Jonathan Cagua](https://github.com/jonathancagua)<br>

</div>

# Tabla de contenidos 馃挕

- [Requisitos](#requisitos)
- [Conceptos](#conceptos)
- [Instalaci贸n](#instalaci贸n)
- [Configuraci贸n](#configuraci贸n)
- [Bitbake](#bitbake)
- [Qemu](#qemu)

# Requisitos
- 50 Gbytes de espacio libre en disco

- Que ejecute una distribuci贸n de Linux compatible (es decir, versiones recientes de Fedora, openSUSE, CentOS, Debian o Ubuntu). 

-  
	Git 1.8.3.1 o superior

	tar 1.27 o superior

	Python 3.4.0 o superior

Los paquetes y la instalaci贸n de los mismos var铆an en funci贸n de su sistema de desarrollo.

(*) Instala los paquetes necesarios para que Yocto funcione desde
        https://www.yoctoproject.org/docs/latest/ref-manual/ref-manual.html#ubuntu-packages

Ejecutar en consola el siguiente comando:

    $ sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint3 xterm zstd liblz4-tool

# Conceptos
## Qu茅 es el Proyecto Yocto

Para entender el resultado proporcionado por el Proyecto Yocto, podemos utilizar la analog铆a de la m谩quina de computaci贸n

    Entrada: Conjunto de datos que describen lo que queremos, es decir, nuestra especificaci贸n (Configuraci贸n del Kernel, Nombre del Hardware, Paquetes/Binarios a instalar)

    Salida: Producto embebido basado en Linux (Kernel Linux, Sistema de Archivos Ra铆z, Bootloader, 脕rbol de Dispositivos, Toolchain)

## Poky

Poky es una distribuci贸n de referencia de Yocto Project. 

Yocto Project utiliza Poky para construir im谩genes (kernel, sistema y software de aplicaci贸n) para el hardware objetivo.

A nivel t茅cnico es un repositorio combinado de los componentes
- Bitbake
- OpenEmbedded Core
- meta-yocto-bsp
- Documentaci贸n

Nota: Poky no contiene archivos binarios, es un ejemplo de c贸mo construir tu propia distribuci贸n de Linux desde el c贸digo fuente.

### 驴Cu谩l es la diferencia entre poky y Yocto?

La diferencia exacta entre Yocto y Poky es que Yocto se refiere a la organizaci贸n ( como uno se referir铆a a 'canonical', la compa帽铆a detr谩s de Ubuntu ), y Poky se refiere a los bits reales descargados ( an谩logo a 'Ubuntu' )

## Metadata 

Un conjunto de datos que describe y da informaci贸n sobre otros datos.

Los metadata se refieren a las instrucciones de compilaci贸n
Comandos y datos utilizados para indicar qu茅 versiones de software se utilizan
De d贸nde se obtienen
Cambios o adiciones al propio software ( parches ) que se utilizan para corregir errores o personalizar el software para su uso en una situaci贸n particular

Los metadatos son una colecci贸n de
- Archivos de configuraci贸n (.conf)
- Recetas (.bb y .bbappend)
- Clases (.bbclass)
- Los includes (.inc)

## Proyecto OpenEmbedded

Desde http://www.openembedded.org/wiki/Main_Page

OpenEmbedded ofrece el mejor entorno de compilaci贸n cruzada de su clase. Permite a los desarrolladores crear una distribuci贸n Linux completa para sistemas embebidos.

### 驴Cu谩l es la diferencia entre OpenEmbedded y el Proyecto Yocto?

El Proyecto Yocto y OpenEmbedded comparten una colecci贸n central de metadatos llamada openembedded-core. 

Sin embargo, las dos organizaciones permanecen separadas, cada una con su propio enfoque

OpenEmbedded proporciona un amplio conjunto de metadatos para una gran variedad de arquitecturas, caracter铆sticas y aplicaciones

	No es una distribuci贸n de referencia
	Est谩 dise帽ada para ser la base de otras.

El Proyecto Yocto se centra en proporcionar herramientas, metadatos y paquetes de soporte de placas (BSP) potentes, f谩ciles de usar, interoperables y bien probados para un conjunto b谩sico de arquitecturas y placas espec铆ficas.


### OpenEmbedded-Core(oe-core)

El Proyecto Yocto y OpenEmbedded han acordado trabajar juntos y compartir un conjunto com煤n de metadatos (recetas, clases y archivos asociados): oe-core

## Bitbake

Bitbake es un componente central del Proyecto Yocto.

B谩sicamente realiza la misma funcionalidad que make.

Es un programador de tareas que analiza el c贸digo mixto de python y shell script

El c贸digo parseado genera y ejecuta tareas, que son b谩sicamente un conjunto de pasos ordenados seg煤n las dependencias del c贸digo.

Lee las recetas y las sigue buscando paquetes, construy茅ndolos e incorporando los resultados en im谩genes de arranque.

Hace un seguimiento de todas las tareas que se procesan para asegurar su finalizaci贸n, maximizando el uso de los recursos de procesamiento para reducir el tiempo de construcci贸n y siendo predecible.

## meta-yocto-bsp

Un Board Support Package (BSP) es una colecci贸n de informaci贸n que define c贸mo soportar un dispositivo de hardware particular, un conjunto de dispositivos o una plataforma de hardware

El BSP incluye informaci贸n sobre las caracter铆sticas de hardware presentes en el dispositivo y la informaci贸n de configuraci贸n del kernel junto con cualquier controlador de hardware adicional necesario.

El BSP tambi茅n enumera cualquier componente de software adicional que se requiera, adem谩s de una pila de software Linux gen茅rica, tanto para las caracter铆sticas esenciales como para las opcionales de la plataforma.

La capa meta-yocto-bsp en Poky mantiene varios BSPs como el Beaglebone, EdgeRouter, y versiones gen茅ricas de m谩quinas IA de 32 y 64 bits.

### M谩quinas soportadas

Texas Instruments BeagleBone (beaglebone)
Freescale MPC8315E-RDB (mpc8315e-rdb)
PCs y dispositivos basados en Intel x86 (genericx86 y genericx86-64)
Ubiquiti Networks EdgeRouter Lite (edgerouter)

Nota: Para desarrollar en un hardware diferente, necesitar谩s complementar Poky con capas Yocto espec铆ficas para el hardware.

## Conclusi贸n

Poky incluye 
- algunos componentes de OE (oe-core)
- bitbake
- demo-BSP's
- scripts de ayuda para configurar el entorno
	emulador QEMU para probar la imagen
![plot](img/poky.png)


# Instalaci贸n
## Repositorio poky
Usar la versi贸n 3.4 de yocto con el nombre **Honister**. Clonar el repositorio Poky:

    git clone -b honister git://git.yoctoproject.org/poky poky-honister

Ingresar a la carpeta creada y hacer pull del repositorio para bajar cambios:

    cd poky-honister
    git pull --all --prune

## Capas meta adicionales
Clonar los repositorios dentro de ***poky-honister***:

    git clone -b honister git://git.yoctoproject.org/meta-raspberrypi
    git clone -b honister git://git.openembedded.org/meta-openembedded
    git clone -b honister https://github.com/meta-qt5/meta-qt5.git

# Configuraci贸n
Salir de la carpeta **poky-honister** para generar los archivos de compilaci贸n fuera de esta carpeta:
 
    cd ..
    source poky-honister/oe-init-build-env build_folder
 
Se crea un nuevo directorio **build_folder**. Dentro de la carpeta se crea un directorio con el nombre de  **conf** con los siguientes archivos:
- bblayers.conf
- local.conf
- templateconf.cfg

## Editando local.conf para simulaci贸n con qemu
Cambiar la siguiente l铆nea:

    # This sets the default machine to be qemux86-64 if no other machine is selected:
    MACHINE ??= "qemux86-64"

Seleccionar la maquina para ejecutar yocto con qemu
    
    # This sets the default machine to be qemux86-64 if no other machine is selected:
    MACHINE ??= "qemuarm"
    
## Editando local.conf para raspberry
Cambiar la siguiente l铆nea:

    # This sets the default machine to be qemux86-64 if no other machine is selected:
    MACHINE ??= "qemux86-64"

Dependiendo de la raspberry que se quiera utilizar (raspberrypi0, raspberrypi0w, raspberrypi3, raspberrypi3-64, raspberrypi4, raspberrypi4-64)
    
    # This sets the default machine to be qemux86-64 if no other machine is selected:
    MACHINE ??= "raspberrypi4-64"

A帽adir las siguientes l铆neas al final:

    IMAGE_FSTYPES = "ext4.xz rpi-sdimg"
    SDIMG_ROOTFS_TYPE = "ext4.xz"

# Bitbake
Dentro de la carpeta **build_folder** ejecutar el siguiente comando para construir una imagen minima.

    bitbake core-image-minimal

En la **[documentaci贸n de Yocto](https://docs.yoctoproject.org/ref-manual/images.html)**  se encontrar谩 una breve descripci贸n de las descripciones de im谩genes disponibles.

Por ejemplo:

 - core-image-minimal: una peque帽a imagen capaz de permitir que un dispositivo arranque.
 - core-image-base: una imagen solo para consola totalmente compatible con el hardware del dispositivo de destino.
 - core-image-full-cmdline: una imagen solo para consola con m谩s funciones completas del sistema de Linux instaladas.

Despu茅s de unas horas el BitBake termina y el **sdimg** se muestra en el siguiente directorio:

    /build_folder/tmp/deploy/images/qemuarm

    o

    /build_folder/tmp/deploy/images/raspberrypi4-64

# Qemu
Las m谩quinas basadas en QEMU permiten realizar pruebas y desarrollos sin necesidad de hardware real.

Actualmente se admiten emulaciones para:

        鈥? ARM
        鈥? MIPS
        鈥? MIPS64
        鈥? PowerPC
        鈥? X86
        鈥? X86_64


Poky proporciona un script 'runqemu' que le permitir谩 iniciar el QEMU utilizando im谩genes generadas por yocto.

El script runqemu se ejecuta como:

    runqemu <machine> <zimage> <filesystem>

donde:

- < machine > es la m谩quina/arquitectura a utilizar (qemuarm/qemumips/qemuppc/qemux86/qemux86-64)

- < zimage > es la ruta de acceso a un n煤cleo (e.g. zimage-qemuarm.bin)
- < filesystem > es la ruta a una imagen ext2 (e.g. filesystem-qemuarm.ext2) o un directorio nfs

Las instrucciones completas de uso se pueden ver ejecutando el comando sin especificar opciones.

Dentro de la carpeta construdia **build_folder**
ejecutar el siguiente comando:

    $ runqemu qemuarm

## Nographic

Es posible ejecutar QEMU sin la ventana gr谩fica a帽adiendo nographic a la l铆nea de comandos

    $ runqemu qemuarm nographic

## Para a帽adir un paquete concreto en su sistema de archivos ra铆z.

Abrir su archivo local.conf y a帽ada el nombre de la receta a continuaci贸n

    IMAGE_INSTALL += "nombre-receta"

Por ejemplo, 

    IMAGE_INSTALL += "usbutils" para lsusb
o 

    IMAGE_INSTALL_append = " usbutils"

### Exit QEMU

Salir de QEMU haciendo click en el icono de apagado o escribiendo Ctrl-C en la ventana de transcripci贸n de QEMU.







