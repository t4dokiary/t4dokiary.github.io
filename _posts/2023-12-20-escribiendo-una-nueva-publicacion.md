---
title: Escribiendo una Nueva Publicacion
author: t4dokiary
date: 2023-12-20 01:35:00 +0800
categories: [Blog, Tutorial]
tags: [Escribiendo]
---

Este tutorial te guiará sobre cómo escribir una publicación en la plantilla _Chirpy_, y vale la pena leerlo incluso si ya has usado Jekyll antes, ya que muchas características requieren que se establezcan variables específicas.

## Nombramiento y Ruta

Crea un nuevo archivo nombrado `YYYY-MM-DD-TITLE.EXTENSION`{: .filepath} y colócalo en `_posts`{: .filepath} del directorio raíz. Ten en cuenta que la `EXTENSION`{: .filepath} debe ser una de `md`{: .filepath} o `markdown`{: .filepath}. Si quieres ahorrar tiempo creando archivos, considera usar el plugin [`Jekyll-Compose`](https://github.com/jekyll/jekyll-compose) para lograrlo.

## Front Matter

Básicamente, necesitas llenar el [Front Matter](https://jekyllrb.com/docs/front-matter/) de la siguiente manera en la parte superior de la publicación:

```yaml
---
title: TÍTULO
date: YYYY-MM-DD HH:MM:SS +/-TTTT
categories: [CATEGORIA_PRINCIPAL, SUB_CATEGORIA]
tags: [ETIQUETA]     # Los nombres de las etiquetas siempre deben estar en minúsculas
---
```

> El _layout_ de las publicaciones se ha establecido por defecto en `post`, por lo que no es necesario agregar la variable _layout_ en el bloque Front Matter.
{: .prompt-tip }

### Zona Horaria de la Fecha

Para registrar con precisión la fecha de lanzamiento de una publicación, no solo debes configurar la `timezone` de `_config.yml`{: .filepath}, sino también proporcionar la zona horaria de la publicación en la variable `date` de su bloque Front Matter. Formato: `+/-TTTT`, por ejemplo, `+0800`.

### Categorías y Etiquetas

Las `categories` de cada publicación están diseñadas para contener hasta dos elementos, y el número de elementos en `tags` puede ser de cero a infinito. Por ejemplo:

```yaml
---
categories: [Animal, Insecto]
tags: [abeja]
---
```

### Información del Autor

La información del autor de la publicación generalmente no necesita ser completada en el _Front Matter_, se obtendrá de las variables `social.name` y la primera entrada de `social.links` del archivo de configuración por defecto. Pero también puedes sobrescribirla de la siguiente manera:

Agregando información del autor en `_data/authors.yml` (Si tu sitio web no tiene este archivo, no dudes en crear uno).

```yaml
<author_id>:
  name: <nombre_completo>
  twitter: <twitter_del_autor>
  url: <pagina_principal_del_autor>
```
{: file="_data/authors.yml" }


Y luego usa `author` para especificar una única entrada o `authors` para especificar múltiples entradas:

```yaml
---
author: <author_id>                     # para una sola entrada
# o
authors: [<author1_id>, <author2_id>]   # para múltiples entradas
---
```


Dicho esto, la clave `author` también puede identificar múltiples entradas.

> El beneficio de leer la información del autor desde el archivo `_data/authors.yml`{: .filepath } es que la página tendrá la meta etiqueta `twitter:creator`, lo que enriquece las [Twitter Cards](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started#card-and-content-attribution) y es bueno para el SEO.
{: .prompt-info }

## Tabla de Contenidos

Por defecto, la **T**abla **d**e **C**ontenidos (TOC) se muestra en el panel derecho de la publicación. Si quieres desactivarla globalmente, ve a `_config.yml`{: .filepath} y establece el valor de la variable `toc` en `false`. Si quieres desactivar la TOC para una publicación específica, añade lo siguiente al [Front Matter](https://jekyllrb.com/docs/front-matter/) de la publicación:

```yaml
---
toc: false
---
```

## Comentarios

El interruptor global de comentarios está definido por la variable `comments.active` en el archivo `_config.yml`{: .filepath}. Después de seleccionar un sistema de comentarios para esta variable, los comentarios se activarán para todas las publicaciones.

Si quieres cerrar los comentarios para una publicación específica, añade lo siguiente al **Front Matter** de la publicación:

```yaml
---
comments: false
---
```

## Matemáticas

Por razones de rendimiento del sitio web, la función matemática no se cargará por defecto. Pero se puede habilitar con:

```yaml
---
math: true
---
```

Después de habilitar la función matemática, puedes añadir ecuaciones matemáticas con la siguiente sintaxis:

- **Matemáticas en bloque** deben añadirse con `$$ math $$` con líneas en **blanco obligatorias** antes y después de `$$`
- **Matemáticas en línea** (en líneas) deben añadirse con `$$ math $$` sin ninguna línea en blanco antes o después de `$$`
- **Matemáticas en línea** (en listas) deben añadirse con `\$$ math $$`

```markdown
<!-- Matemáticas en bloque, mantener todas las líneas en blanco -->

$$
Expresión_matemática_LaTeX
$$

<!-- Matemáticas en línea, SIN líneas en blanco -->

"Lorem ipsum dolor sit amet, $$ Expresión_matemática_LaTeX $$ consectetur adipiscing elit."

<!-- Matemáticas en línea en listas, escapar el primer `$` -->

1. \$$ Expresión_matemática_LaTeX $$
2. \$$ Expresión_matemática_LaTeX $$
3. \$$ Expresión_matemática_LaTeX $$
```

## Mermaid

[**Mermaid**](https://github.com/mermaid-js/mermaid) es una excelente herramienta de generación de diagramas. Para habilitarlo en tu publicación, añade lo siguiente al bloque YAML:

```yaml
---
mermaid: true
---
```

