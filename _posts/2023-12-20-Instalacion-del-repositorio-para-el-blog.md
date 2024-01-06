---
title: Instalacion del repositorio para el blog
author: t4dokiary
date: 2023-12-20 02:35:00 +0800
categories: [Blog, Instalacion]
tags: [Instalacion]
---

## Prerrequisitos

Sigue las instrucciones en [Jekyll Docs](https://jekyllrb.com/docs/installation/) para completar la instalación del entorno básico. También es necesario instalar [Git](https://git-scm.com/).

## Instalación

### Creando un Nuevo Sitio

Hay dos maneras de crear un nuevo repositorio para este tema:

- [**Usando el Iniciador Chirpy**](#opcion-1-usando-el-iniciador-chirpy) - Fácil de actualizar, aísla archivos del proyecto irrelevantes para que puedas concentrarte en escribir.
- [**Fork en GitHub**](#opcion-2-fork-en-github) - Conveniente para el desarrollo personalizado, pero difícil de actualizar. A menos que estés familiarizado con Jekyll y estés decidido a ajustar o contribuir a este proyecto, no se recomienda este enfoque.

#### Opción 1. Usando el Iniciador Chirpy

Inicia sesión en GitHub y navega a [**Chirpy Starter**][starter], haz clic en el botón <kbd>Usar esta plantilla</kbd> > <kbd>Crear un nuevo repositorio</kbd>, y nombra el nuevo repositorio `USERNAME.github.io`, donde `USERNAME` representa tu nombre de usuario en GitHub.

#### Opción 2. Fork en GitHub

Inicia sesión en GitHub para [hacer fork de **Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy/fork), y luego renómbralo a `USERNAME.github.io` (`USERNAME` significa tu nombre de usuario).

Luego, clona tu sitio a la máquina local. Para poder construir archivos de JavaScript más adelante, necesitamos instalar [Node.js][nodejs], y luego ejecutar la herramienta:

```console
$ bash tools/init
```

> Si no deseas desplegar tu sitio en GitHub Pages, agrega la opción `--no-gh` al final del comando anterior.
{: .prompt-info }

El comando anterior:

1. Cambiará el código a la [última etiqueta][latest-tag] (para asegurar la estabilidad de tu sitio: ya que el código para la rama por defecto está en desarrollo).
2. Eliminará archivos de muestra no esenciales y se ocupará de los archivos relacionados con GitHub.
3. Construirá archivos de JavaScript y los exportará a `assets/js/dist/`{: .filepath }, luego hará que sean rastreados por Git.
4. Creará automáticamente un nuevo commit para guardar los cambios anteriores.

### Instalando Dependencias

Antes de ejecutar el servidor local por primera vez, ve al directorio raíz de tu sitio y ejecuta:

```console
$ bundle
```

## Uso

### Configuración

Actualiza las variables de `_config.yml`{: .filepath} según sea necesario. Algunas de ellas son opciones típicas:

- `url`
- `avatar`
- `timezone`
- `lang`

### Opciones de Contacto Social

Las opciones de contacto social se muestran en la parte inferior de la barra lateral. Puedes activar/desactivar los contactos especificados en el archivo `_data/contact.yml`{: .filepath }.

### Personalizando la Hoja de Estilos

Si necesitas personalizar la hoja de estilos, copia la `assets/css/jekyll-theme-chirpy.scss`{: .filepath} del tema al mismo camino en tu sitio Jekyll, y luego añade el estilo personalizado al final de ella.

A partir de la versión `6.2.0`, si deseas sobrescribir las variables SASS definidas en `_sass/addon/variables.scss`{: .filepath}, copia el archivo principal sass `_sass/main.scss`{: .filepath} en el directorio `_sass`{: .filepath} de la fuente de tu sitio, luego crea un nuevo archivo `_sass/variables-hook.scss`{: .filepath} y asigna un nuevo valor.

### Personalizando Activos Estáticos

La configuración de activos estáticos se introdujo en la versión `5.1.0`. El CDN de los activos estáticos está definido por el archivo `_data/origin/cors.yml`{: .filepath }, y puedes reemplazar algunos de ellos según las condiciones de la red en la región donde se publica tu sitio web.

Además, si deseas alojar los activos estáticos tú mismo, consulta el [_chirpy-static-assets_](https://github.com/cotes2020/chirpy-static-assets#readme).

### Ejecutando el Servidor Local

Es posible que desees previsualizar el contenido del sitio antes de publicarlo, así que simplemente ejecútalo con:

```console
$ bundle exec jekyll s
```

Después de unos segundos, el servicio local se publicará en _<http://127.0.0.1:4000>_.

## Despliegue

Antes de comenzar el despliegue, revisa el archivo `_config.yml`{: .filepath} y asegúrate de que la `url` esté configurada correctamente. Además, si prefieres el [**sitio del proyecto**](https://help.github.com/en/github/working-with-github-pages/about-github-pages#types-of-github-pages-sites) y no usas un dominio personalizado, o deseas visitar tu sitio web con una URL base en un servidor web que no sea **GitHub Pages**, recuerda cambiar el `baseurl` por el nombre de tu proyecto que comience con una barra, por ejemplo, `/nombre-del-proyecto`.

Ahora puedes elegir _UNA_ de las siguientes métodos para desplegar tu sitio Jekyll.

### Desplegar Usando GitHub Actions

Hay algunas cosas que preparar.

- Si estás en el plan gratuito de GitHub, mantén tu repositorio del sitio público.
- Si has comprometido `Gemfile.lock`{: .filepath} al repositorio, y tu máquina local no está ejecutando Linux, ve a la raíz de tu sitio y actualiza la lista de plataformas del archivo de bloqueo:

  ```console
  $ bundle lock --add-platform x86_64-linux
  ```

A continuación, configura el servicio _Pages_.

1. Navega a tu repositorio en GitHub. Selecciona la pestaña _Settings_, luego haz clic en _Pages_ en la barra de navegación izquierda. Luego, en la sección **Source** (bajo _Build and deployment_), selecciona [**GitHub Actions**][pages-workflow-src] en el menú des
