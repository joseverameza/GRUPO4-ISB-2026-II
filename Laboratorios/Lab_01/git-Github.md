# Laboratorio 01 — Introducción al repositorio del curso
## Introducción
En el laboratorio 01 se presentó la estructura básica del repositorio para el proyecto del curso, así como guías sobre Markdown, Git y GitHub, y también sobre VS Code como herramienta para codificar y mantener el repositorio. 

## Markdown — Visual Studio Code
Markdown es el lenguaje utilizado para documentación, especialmente en GitHub. Usando Visual Studio Code, se puede convertir el editor en una herramienta completa para Markdown, facilitando la creación y edición de documentos directamente para el repositorio.

**Pasos:**
1. Descargar e instalar Visual Studio Code.
2. Instalar la extensión **"Markdown Preview Enhanced"**.
3. Crear el archivo `README.md`.

Esta extensión permite previsualizar cómo se verá el documento: haciendo clic derecho sobre el archivo .md y seleccionando Open Preview. Cada edición se refleja en tiempo real en la ventana de previsualización.

## Git — GitHub
Git es un sistema de control de versiones que corre localmente en la computadora, es decir, funciona sin conexión a internet. Organiza el proyecto en cuatro áreas:

* **Directorio de trabajo:** donde se crean y editan los archivos.
* **Área de trabajo:** lugar temporal donde se preparan los cambios antes de guardarlos.
* **Repositorio local:** copia del proyecto guardada en tu computadora.
* **Repositorio remoto:** versión compartida del proyecto (en GitHub).

En la práctica, puedes editar y actualizar tu proyecto en tu computadora local sin depender de internet. De esta forma, cada integrante del equipo puede avanzar en su propia copia offline y luego sincronizar sus cambios con el repositorio remoto.

En lugar de hacer estos cambios desde la terminal, usamos VS Code. Con gh (GitHub CLI) instalado, se asocia la cuenta de Git con la cuenta de GitHub. Desde VS Code se inicia el repositorio y se publica, quedando asociado a tu cuenta de GitHub. A partir de ahí, puedes editar el repositorio y aplicar cambios (commits, push, pull) directamente desde la interfaz de VS Code, sin necesidad de usar la terminal de Git.
