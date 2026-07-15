---
title: "01 Web Scraping ENAHO 2004-2025"
output:
  md_document:
    variant: gfm+footnotes
    preserve_yaml: TRUE
knit: (function(inputFile, encoding) {
  rmarkdown::render(inputFile, encoding = encoding, output_dir = "../_posts") })
date: 2026-05-05
permalink: /portfolio/Portfolio-01-2026-05-05-ENAHO
excerpt_separator: <!--more-->
toc: true
header:
 og_image: "posts/Portfolio-01-2026-05-05-ENAHO/shared_legend_right-1.png"
tags:
  - GIS
  - visualization
  - peacekeeping
---

Script automatizado en Stata para descargar, organizar y extraer los módulos de la Encuesta Nacional de Hogares (ENAHO) del portal de [Microdatos](https://proyectos.inei.gob.pe/microdatos/) del INEI (2004–2025). Incluye gestión de módulos, extracción de archivos ZIP y una estructura reproducible para facilitar el procesamiento de datos.

<!--more-->

1. Abrir el archivo 
```bash
Download-ENAHO-2004-2025.do
```

2. Modificar la ruta donde se almacenarán los archivos descargados.
```stata
global Path = "E:\07. GitHub\01-Web-Scraping-ENAHO-2004-2025"
```

3. Si lo deseas, modificar el rango de años:
```stata
local y_start = 4
local y_end   = 25
```

4. Seleccionar los módulos que deseas descargar:
```stata
foreach j in 1 2 3 4 5 {
```

5. Ejecutar el script.

## 3. Módulos disponibles <a id="3"></a>

| **Nro.** | **Módulo** | **Descripción** |
|---:|---|---|
| 1 | Módulo 1 | Características de la Vivienda y del Hogar |
| 2 | Módulo 2 | Características de los Miembros del Hogar |
| 3 | Módulo 3 | Educación |
| 4 | Módulo 4 | Salud |
| 5 | Módulo 5 | Empleo e Ingresos |
| 6 | Módulo 7 | Gastos en Alimentos y Bebidas |
| 7 | Módulo 8 | Instituciones Benéficas |
| 8 | Módulo 9 | Mantenimiento de la Vivienda |
| 9 | Módulo 10 | Transportes y Comunicaciones |
| 10 | Módulo 11 | Servicios de la Vivienda |
| 11 | Módulo 12 | Esparcimiento, Diversión y Servicios Culturales |
| 12 | Módulo 13 | Vestido y Calzado |
| 13 | Módulo 15 | Gastos de Transferencias |
| 14 | Módulo 16 | Muebles y Enseres |
| 15 | Módulo 17 | Otros Bienes y Servicios |
| 16 | Módulo 18 | Equipamiento del Hogar |
| 17 | Módulo 22 | Producción Agrícola |
| 18 | Módulo 23 | Subproductos Agrícolas |
| 19 | Módulo 24 | Producción Forestal |
| 20 | Módulo 25 | Gastos en Actividades Agrícolas y/o Forestales |
| 21 | Módulo 26 | Producción Pecuaria |
| 22 | Módulo 27 | Subproductos Pecuarios |
| 23 | Módulo 28 | Gastos en Actividades Pecuarias |
| 24 | Módulo 34 | Variables Calculadas (Resumen) |
| 25 | Módulo 37 | Programas Sociales |
| 26 | Módulo 77 | Ingresos del Trabajador Independiente |
| 27 | Módulo 78 | Bienes y Servicios para el Cuidado Personal |
| 28 | Módulo 84 | Participación Ciudadana |
| 29 | Módulo 85 | Gobernabilidad, Democracia y Transparencia |
| 30 | Módulo 1825 | Beneficiarios de Instituciones sin fines de lucro: Olla Común |
| 31 | Módulo 2081 | Crianza de Mascotas en el Hogar |
| 32 | Módulo 2082 | Inseguridad Alimentaria |

## 4. Funcionamiento del script <a id="4"></a>
El script realiza automáticamente las siguientes tareas:

1. Crea la estructura de carpetas del proyecto.
2. Recorre los años seleccionados.
3. Obtiene el Código de Encuesta correspondiente a cada año.
4. Recorre los módulos seleccionados.
5. Descarga cada archivo ZIP desde el portal oficial del INEI.
6. Descomprime automáticamente cada archivo.
7. Conserva el archivo ZIP cuando ocurre un error durante la extracción.

El proceso completo puede resumirse mediante el siguiente flujo:

El siguiente diagrama resume el flujo de ejecución del script para descargar y extraer automáticamente los módulos de la **Encuesta Nacional de Hogares (ENAHO)**:

```mermaid
---
title: Flujo del proceso de descarga y extracción de la ENAHO (2004–2025)
---
flowchart TD

A([Inicio]) --> B[Definir directorio de trabajo]
B --> C[Crear carpeta principal ENAHO]
C --> D{{Iterar por años<br/>2004–2025}}
D --> E[Obtener Código de Encuesta]
E --> F{{Iterar por módulos}}
F --> G[Construir URL de descarga]
G --> H[Descargar archivo ZIP]
H --> I[Descomprimir archivo ZIP]
I --> J{¿Extracción exitosa?}
J -- Sí --> K[Continuar con el siguiente módulo]
J -- No --> L[Conservar archivo ZIP<br/>Mostrar mensaje de extracción manual]
L --> K
K --> M{¿Quedan módulos?}
M -- Sí --> F
M -- No --> N{¿Quedan años?}
N -- Sí --> D
N -- No --> O([Fin])
```
*Elaboración propia.* <br>
***Nota:** El diagrama muestra el flujo de ejecución del script, incluyendo la iteración por años y módulos, la construcción de la URL de descarga, la obtención de los archivos desde el portal oficial del INEI y su extracción automática. En caso de que un archivo comprimido presente inconsistencias, el script conserva el archivo `.zip` y notifica al usuario que la extracción debe realizarse manualmente.*

## 5. Resultado 📂<a id="5"></a>
Al finalizar la ejecución se obtiene una estructura similar a la siguiente:

```text
ENAHO/
│
├──2004/
│   ├── enaho01-2004.dta
│   ├── enaho02-2004.dta
│   └── ...
│
├──2005/
│
├──...
│
└──2025/
```
Cada carpeta contiene todos los módulos descargados y extraídos para el año correspondiente.

## 6. Observaciones ⚠️<a id="6"></a>
En algunos años, determinados archivos ZIP publicados por el INEI presentan inconsistencias que impiden su extracción automática mediante Stata.
Cuando esto ocurre, el script muestra un mensaje indicando que el archivo debe descomprimirse manualmente. El archivo ZIP descargado se conserva para facilitar este proceso.

## Licencia <a id="9"></a>
Este proyecto está licenciado bajo la Licencia MIT. Consulta el archivo [LICENSE](/LICENSE) para más detalles.

## Autor 👨‍💻<a id="10"></a>

**Carlos Eduardo Torres García**
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/carlo4-eduardo-torres-garcia/)
[![X Twitter](https://img.shields.io/badge/Twitter-000000?style=flat&logo=x&logoColor=white)](https://x.com/Carlo4_Eduardo)

[**⬆ Volver al inicio**](#a)


We can either supply each individual subplot to `plot_grid()`
separately, or we can use the `plotlist` argument to pass a list of
plots; good thing we saved them in a list:

``` r
## use COWplot to combine and add single legend
plot_grid(plotlist = maps, labels = LETTERS[1:5], label_size = 10, nrow = 2)
```

<img src="/images/posts/geom-sf-facet/individual_legends-1.png" style="display: block; margin: auto;" />

I tried using the name of each country as the subplot label, but because
[label positioning is relative to the width of
labels](https://github.com/wilkelab/cowplot/issues/32#issuecomment-198428848)
it was impossible to get them all nicely left-aligned. As a result, I
had to settle on using letters to label the subplots and then
identifying them in the figure caption in text. As you’ll see
[later](#bonus-still-to-solve), there’s no perfect way of accomplishing
this and you’ll have to make a trade-off somewhere.

Setting aside that compromise, there’s still one issue with this plot
that we can fix. We’re measuring the same thing (attacks on UN
peacekeeping personnel) in all five choropleths, so there’s no need for
five separate scales.

## Shared legend

The `cowplot`
[documentation](https://wilkelab.org/cowplot/articles/shared_legends.html)
demonstrates how to use the `get_legend()` function to extract the
legend from one of the subplots and then add it as another element to
`plot_grid()`, placing it in the bottom right like we sort of managed to
do with `tmap`. However, we need to add
`theme(legend.position = 'none')` to the ggplot call for each subplot,
otherwise we’ll just end up with six legends. That means we need to
apply to each element of our list of maps, which means it’s another job
that `map()` is perfect for! We’ll use `map()` to take each subplot in
`maps` and remove the legend from it, then use `get_legend()` to add a
legend in the bottom right.

``` r
## use COWplot to combine and add single legend
plot_grid(plotlist = c(map(.x = maps,
                           .f = function(x) x + theme(legend.position = 'none'))),
          get_legend(maps[[1]]),
          labels = LETTERS[1:5], label_size = 10, nrow = 2)
```

<img src="/images/posts/geom-sf-facet/shared_legend_missing-1.png" style="display: block; margin: auto;" />
This doesn’t look right! We told `plot_grid()` to start with our maps,
so why is the legend the first thing in the plot? If you look closely at
the documentation for `plot_grid()`, you’ll see that the `...` argument
comes before the `plotlist` argument in the function definition. Even
when we specify `plotlist` first, the function will add `plotlist` after
`...`.[^6] To fix this, all we need to do is concatenate the results of
`get_legend()` with the results of our call to `map()`. Note that we
need to first transform the former to a list with `list()`, otherwise
each element of it will be concatenated separately rather than as a
`grob` object:

``` r
## use COWplot to combine and add single legend
plot_grid(plotlist = c(map(.x = maps,
                           .f = function(x) x + theme(legend.position = 'none')),
                       list(get_legend(maps[[1]]))),
          labels = LETTERS[1:5],
          label_size = 10,
          nrow = 2)
```

<img src="/images/posts/geom-sf-facet/shared_legend_wrong-1.png" style="display: block; margin: auto;" />

So far so good. But if we try using a different map in our call to
`get_legend()`, things get weird:

``` r
## use COWplot to combine and add single legend
plot_grid(plotlist = c(map(.x = maps,
                           .f = function(x) x + theme(legend.position = 'none')),
                       list(get_legend(maps[[4]]))),
          labels = LETTERS[1:5], label_size = 10, nrow = 2)
```

<img src="/images/posts/geom-sf-facet/shared_legend_wrong2-1.png" style="display: block; margin: auto;" />
Each subplot has its own unique legend that’s automatically generated
from the values of `attacks` it contains. This is even worse than it
might seem at first glance, because it means that the various subplots
are in no way comparable to one another!

## Accurate shared legend

To avoid misrepresenting the data, we need to ensure that each subplot
has the same legend. The easiest way to do this is to manually set the
legend for each subplot in our call to `scale_fill_continuous()`. Even
though we’re manually setting the bounds of the legend, that doesn’t
mean we have to hard code them. We can use a simpler version of our code
to join attacks to ADM2s and then calculate the highest number of
attacks across *all* countries in the data. Then we take advantage of
the fact that `scale_fill_continuous()` can pass additional parameters
to `continuous_scale()` via the `...` argument. The `continuous_scale()`
function is a low-level function used throughout `ggplot2` to construct
continuous scales, and it has a `limits` argument that sets the bounds
of the scale. All we have to do is pass the minimum and maximum (logged)
numbers of attacks in the data and we’re in business:

``` r
st_join(adm, acled) %>% 
  st_drop_geometry() %>%   # we don't need a map at the end; drop geometry to speed up
  group_by(NAME_0, NAME_1, NAME_2) %>% 
  summarize(attacks = log1p(sum(!is.na(event_id_cnty)))) %>% 
  pull(attacks) %>%        # extract attacks variable
  range() -> attacks_range # get min and max

## create maps in separate plots, force common scale between them
maps_shared <- map(.x = pko_countries, 
                   .f = function(x) adm %>% 
                     filter(NAME_0 == x) %>% 
                     st_join(acled) %>% 
                     group_by(NAME_0, NAME_1, NAME_2) %>% 
                     summarize(attacks = log1p(sum(!is.na(event_id_cnty)))) %>% 
                     ggplot(aes(fill = attacks)) +
                     geom_sf(lwd = NA) +
                     scale_fill_continuous(limits = attacks_range,
                                           name = 'PKO targeting\nevents (logged)') +
                     theme_rw() +
                     theme(axis.text = element_blank(),
                           axis.ticks = element_blank()))
```

Now all that’s left is to use `plot_grid()` to put it all together:

``` r
## use COWplot to combine and add single legend
plot_grid(plotlist = c(map(.x = maps_shared,
                           .f = function(x) x + theme(legend.position = 'none')),
                       list(get_legend(maps_shared[[1]]))),
          labels = LETTERS[1:5], label_size = 10, nrow = 2)
```

<img src="/images/posts/geom-sf-facet/shared_legend_right-1.png" style="display: block; margin: auto;" />

And unlike before, the legend is identical regardless of which subplot
we use with `get_legend()`:

``` r
## use COWplot to combine and add single legend
plot_grid(plotlist = c(map(.x = maps_shared,
                           .f = function(x) x + theme(legend.position = 'none')),
                       list(get_legend(maps_shared[[4]]))),
          labels = LETTERS[1:5], label_size = 10, nrow = 2)
```

<img src="/images/posts/geom-sf-facet/shared_legend_right2-1.png" style="display: block; margin: auto;" />

This approach is still useful even if you’re not working with spatial
data. `plot_grid()` is powerful because it lets you make asymmetric
arrangements like this example from the `cowplot`
[documentation](https://wilkelab.org/cowplot/articles/plot_grid.html):

``` r
p1 <- ggplot(mtcars, aes(disp, mpg)) + 
  geom_point()
p2 <- ggplot(mtcars, aes(qsec, mpg)) +
  geom_point()

plot_grid(p1, p2, labels = c('A', 'B'), rel_widths = c(1, 2))
```

<img src="/images/posts/geom-sf-facet/plot_grid_asymmetric-1.png" style="display: block; margin: auto;" />

If the units you’re faceting by contain substantially different
observations, you might end up in a situation where the automatically
generated legends are different from one another. Manually creating the
scale of the legend and ensuring it’s the same for all plots would solve
this problem here, too.

# Bonus: still to solve

Don’t let anyone convince you they know everything. I still haven’t
managed to get my ideal (conditional on regular faceting with
`facet_wrap()` being out of the question) solution to this working. I
tried to create five subplots and just add a facet label to each, with
each one being a facet of one panel. Straightforward enough, right?

``` r
maps_facet <- map(.x = pko_countries, 
                  .f = function(x) adm %>% 
                    filter(NAME_0 == x) %>% 
                    st_join(acled) %>% 
                    group_by(NAME_0, NAME_1, NAME_2) %>% 
                    summarize(attacks = log1p(sum(!is.na(event_id_cnty)))) %>% 
                    ggplot(aes(fill = attacks)) +
                    geom_sf(lwd = NA) +
                    scale_fill_continuous(limits = attacks_range,
                                          name = 'PKO targeting\nevents (logged)') +
                    facet_wrap(~NAME_0) +
                    theme_rw() +
                    theme(axis.text = element_blank(),
                          axis.ticks = element_blank()))

plot_grid(plotlist = c(map(.x = maps_facet,
                           .f = function(x) x + theme(legend.position = 'none')),
                       list(get_legend(maps_facet[[1]]))),
          nrow = 2)
```

<img src="/images/posts/geom-sf-facet/shared_legend_facet_calc-1.png" style="display: block; margin: auto;" />

Not so much, and no amount of tinkering with the `align` and `axis`
arguments to `plot_grid()` has yielded any improvement. The specific
paper this plot is for doesn’t have any other plots with facets, so I’m
content to go with my inelegant solution of lettered labels and a key to
them in the figure caption. If that weren’t the case, I might still be
fiddling with this and getting deeper and deeper into the source code
for `plot_grid()`.

[^1]: If you’re wondering why the largest county area is in the ballpark
    of 0.25, it’s because the data are in [square
    degrees](https://en.wikipedia.org/wiki/Square_degree), an old non-SI
    unit of measurement that’s defined in terms of how much the field of
    view from a given point is obstructed by an object. GIS is so easy
    these days, folks.

[^2]: The more I learn about how `ggplot2` and `sf` work under the hood,
    the more amazed I am that `geom_sf()` Just Works in 80% of cases,
    let alone works at all.

[^3]: The answer also listed the `geom_spatial()` function from the
    `ggspatial` package as an alternative option, but I couldn’t get it
    to work. The answer is three and a half years old, which means it’s
    very possible something changed in either `sf` or `ggspatial` that
    broke this solution. So it goes.

[^4]: It’s much more powerful and easily customizable than
    `gridExtra::grid.arrange()`.

[^5]: They can also contain heterogeneous elements which will come in
    handy [later](#shared-legend).

[^6]: If you check out the actual source code of `plot_grid()`, line 9
    shows you that the function is indeed putting `...` ahead of
    `plotlist`: `plots <- c(list(...), plotlist)`.
