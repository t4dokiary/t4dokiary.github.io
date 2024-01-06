---
title: Personalizar el Favicon
author: t4dokiary
date: 2023-12-20 03:35:00 +0800
categories: [Blog, Icon]
tags: [favicon]
---

Los [favicons](https://www.favicon-generator.org/about/) de [**Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy/) están ubicados en el directorio `assets/img/favicons/`{: .filepath}. Puede que quieras reemplazarlos por los tuyos propios. Las siguientes secciones te guiarán para crear y reemplazar los favicons predeterminados.

## Generar el favicon

Prepara una imagen cuadrada (PNG, JPG o SVG) con un tamaño de 512x512 o más, y luego ve a la herramienta en línea [**Real Favicon Generator**](https://realfavicongenerator.net/) y haz clic en el botón <kbd>Selecciona tu imagen de Favicon</kbd> para subir tu archivo de imagen.

En el siguiente paso, la página web mostrará todos los escenarios de uso. Puedes mantener las opciones predeterminadas, desplazarte hasta la parte inferior de la página y hacer clic en el botón <kbd>Genera tus Favicons y código HTML</kbd> para generar el favicon.

## Descargar y Reemplazar

Descarga el paquete generado, descomprímelo y elimina los siguientes dos de los archivos extraídos:

- `browserconfig.xml`{: .filepath}
- `site.webmanifest`{: .filepath}

Luego copia los archivos de imagen restantes (`.PNG`{: .filepath} y `.ICO`{: .filepath}) para cubrir los archivos originales en el directorio `assets/img/favicons/`{: .filepath} de tu sitio Jekyll. Si tu sitio Jekyll aún no tiene este directorio, solo créalo.

La siguiente tabla te ayudará a entender los cambios en los archivos de favicon:

| Archivo(s)          | De la Herramienta en Línea        | De Chirpy   |
|---------------------|:---------------------------------:|:-----------:|
| `*.PNG`             | ✓                                 | ✗           |
| `*.ICO`             | ✓                                 | ✗           |

>  ✓ significa mantener, ✗ significa eliminar.
{: .prompt-info }

La próxima vez que construyas el sitio, el favicon será reemplazado por una edición personalizada.
