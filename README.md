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

```bash
nano CMakeLists.txt
nano prj.conf
mkdir src
cd src
nano main.c
cd ..
```

### Compilar el proyecto especificando la tarjeta destino:

```bash
west build -b beagleconnect_freedom@C7/cc1352p7 -p always
```

### Ajustes de configuración del Kernel (Opcional):

```bash
west build -t guiconfig
```

---


