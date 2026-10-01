# Guia-de-Apuntes-Introduccion-a-Linux-y-Zephyr

Este repositorio contiene los apuntes, comandos básicos y la estructura de trabajo fundamental para el desarrollo de aplicaciones embebidas utilizando **Zephyr RTOS** en entornos **Linux**, aprendidos durante la sesión del semillero universitario.

---

## Contenido
1. [Navegación y Comandos Básicos de Linux](#1-navegación-y-comandos-básicos-de-linux)
2. [Entornos Virtuales en Python (`venv`)](#2-entornos-virtuales-en-python-venv)
3. [Estructura Estándar de un Proyecto en Zephyr](#3-estructura-estándar-de-un-proyecto-en-zephyr)
4. [Flujo de Trabajo y Compilación con `west`](#4-flujo-de-trabajo-y-compilación-con-west)
---

## 1. Navegación y Comandos Básicos de Linux

El trabajo en Zephyr se realiza principalmente a través de la interfaz de línea de comandos (CLI). A continuación se resumen los comandos básicos de navegación y gestión de archivos:

| Comando | Descripción | Ejemplo |
| :--- | :--- | :--- |
| `ls` | Listar archivos y directorios en la carpeta actual. | `ls` |
| `cd <dir>` | Cambiar de directorio (navegar). | `cd zephyrproject/zephyr/` |
| `cd ..` | Retroceder un nivel en la jerarquía de carpetas. | `cd ..` |
| `cd ~` | Volver al directorio raíz del usuario (`home`). | `cd ~` |
| `mkdir <nombre>` | Crear un nuevo directorio o carpeta. | `mkdir Codigo_Prueba_1` |
| `rmdir <nombre>` | Eliminar una carpeta vacía. | `rmdir scr` |
| `rm <archivo>` | Eliminar archivos. | `rm *.*` |
| `rm -rf <dir>` | Eliminar archivos y carpetas recursivamente y por la fuerza. | `rm -rf scr` |
| `nano <archivo>` | Abrir el editor de texto ligero en la terminal. | `nano CMakeLists.txt` |

---

## 2. Entornos Virtuales en Python (`venv`)

Zephyr utiliza herramientas basadas en Python (como la herramienta CLI `west`). Para aislar las dependencias y evitar errores del sistema, se trabaja dentro de un entorno virtual.

* Si el prompt muestra `(digital)` o `(.venv)` al inicio, significa que el entorno virtual está activo.
* Si el comando `west` no es reconocido, activa el entorno ejecutando:

```bash
source ~/zephyrproject/.venv/bin/activate
```
## 3. Estructura Estándar de un Proyecto en Zephyr

Para que el sistema de compilación entienda el proyecto, se requiere una estructura mínima de archivos y carpetas:

```bash
Codigo_Prueba_1/
├── CMakeLists.txt    # Configuración de compilación con CMake
├── prj.conf          # Configuración Kconfig (módulos y drivers de Zephyr)
└── src/              # Carpeta del código fuente
    └── main.c        # Código fuente principal en C
```
---

## 4. Flujo de Trabajo y Compilación con west

Pasos secuenciales para crear, escribir y compilar una aplicación:

### Ingresar a la carpeta de Zephyr:

```bash
cd ~/zephyrproject/zephyr
```

### Crear y entrar al directorio del proyecto:

```bash
mkdir Codigo_Prueba_1
cd Codigo_Prueba_1
```

### Crear la estructura del proyecto:

En Zephyr RTOS, el sistema de compilación se basa en la combinación de **CMake** (para la gestión y construcción del proyecto) y **Kconfig** (para la configuración modular del sistema operativo). Para que el entorno reconozca y compile correctamente una aplicación, es obligatorio contar con la siguiente estructura: 

A continuación se detalla la creación de cada archivo esencial y su función dentro del proyecto:

#### Creaciòn archivo CMakelists.txt:

El archivo `CMakeLists.txt` es la guía principal de compilación. Le indica a CMake la versión mínima requerida, vincula las librerías y macros del núcleo de Zephyr, define el nombre formal del proyecto y especifica qué archivos de código fuente deben procesarse para generar el ejecutable.

```bash
nano CMakeLists.txt
```

#### Creaciòn archivo prj.conf:

El archivo `prj.conf` utiliza el sistema Kconfig para personalizar el kernel de Zephyr. A través de este archivo se pueden activar o desactivar controladores de hardware (GPIO, I2C, SPI, UART), habilitar pilas de red/Bluetooth, ajustar tamaños de pila (stack) de los hilos de ejecución o activar opciones de depuración, todo sin necesidad de modificar el código en C.

```bash
nano prj.conf
```
#### Creaciòn carpeta src y archivo main.c:

Por estándar en desarrollo con Zephyr y lenguaje C, todo el código fuente de la aplicación debe organizarse dentro del directorio `src`. El archivo `main.c` contiene la función `main()`, la cual representa el punto de entrada del programa una vez que el RTOS ha finalizado su secuencia de arranque e inicialización de periféricos.

```bash
mkdir src
cd src
nano main.c
cd ..
```

### Compilar el proyecto especificando la tarjeta destino:

El comando `west build` compila la aplicación indicando la versión exacta del hardware mediante `-b`. La bandera `-p always` (pristine) realiza una compilación limpia, eliminando la caché de construcciones anteriores para prevenir errores de compilación residuales.

```bash
west build -b beagleconnect_freedom@C7/cc1352p7 -p always
```

### Asignar Permisos al Puerto Serial:

En Linux, los dispositivos USB/Serial asignados en la ruta `/dev/ttyACM0` requieren permisos de lectura y escritura para que el usuario pueda programarlos o comunicarse con ellos.

```bash
sudo chmod 666 /dev/ttyACM0
```

### Subir el código a la placa (Flashear):

Transfiere el archivo binario generado durante la compilación a la memoria flash del microcontrolador conectado.

```bash
west flash
```

---


