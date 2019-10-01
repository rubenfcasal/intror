Generación de informes
======================





R Markdown
----------

R-Markdown es recomendable para difundir análisis realizados con `R` en formato HTML, PDF y DOCX (Word), entre otros. 

### Introducción

R-Markdown permite combinar Markdown con `R`. [Markdown](http://daringfireball.net/projects/markdown/) se diseñó inicialmente para la creación de páginas web a partir de documentos de texto de forma muy sencilla y rápida (tiene unas reglas sintácticas muy simples). Actualmente gracias a múltiples herramientas como [pandoc](http://pandoc.org/) permite generar múltiples tipos de documentos (incluido LaTeX; ver [Pandoc Markdown](http://rmarkdown.rstudio.com/authoring_pandoc_markdown.html))


Para más detalles ver <http://rmarkdown.rstudio.com>.

También se dispone de información en la ayuda de *RStudio*:

-   *Help > Markdown Quick Reference*

-   *Help > Cheatsheets > R Markdown Cheat Sheet*

-   *Help > Cheatsheets > R Markdown Reference Guide*

Al renderizar un fichero rmarkdown se generará un documento que incluye el código `R` 
y los resultados incrustados en el documento. 
En *RStudio* basta con hacer clic en el botón **Knit HTML**. 
En `R` se puede emplear la funcion `render` del paquete *rmarkdown* 
(por ejemplo: `render("8-Informes.Rmd")`).
También se puede abrir directamente el informe generado:
```
library(rmarkdown)
browseURL(url = render("8-Informes.Rmd"))
```

### Inclusión de código R

Se puede incluir código R entre los delimitadores ` ```{r} ` y ` ``` `. Por defecto, se mostrará el código, se evaluará y se mostrarán los resultados justo a continuación:

```r
head(mtcars[1:3])
```

```
##                    mpg cyl disp
## Mazda RX4         21.0   6  160
## Mazda RX4 Wag     21.0   6  160
## Datsun 710        22.8   4  108
## Hornet 4 Drive    21.4   6  258
## Hornet Sportabout 18.7   8  360
## Valiant           18.1   6  225
```

```r
summary(mtcars[1:3])
```

```
##       mpg             cyl             disp      
##  Min.   :10.40   Min.   :4.000   Min.   : 71.1  
##  1st Qu.:15.43   1st Qu.:4.000   1st Qu.:120.8  
##  Median :19.20   Median :6.000   Median :196.3  
##  Mean   :20.09   Mean   :6.188   Mean   :230.7  
##  3rd Qu.:22.80   3rd Qu.:8.000   3rd Qu.:326.0  
##  Max.   :33.90   Max.   :8.000   Max.   :472.0
```


En *RStudio* pulsando "Ctrl + Alt + I" o en el icono correspondiente se incluye un trozo de código.

Se puede incluir código en línea empleando `` `r código` ``, 
por ejemplo `` `r 2 + 2` `` produce 4.


### Inclusión de gráficos

Se pueden generar gráficos:

\begin{center}\includegraphics[width=0.7\linewidth]{12-Informes_files/figure-latex/figura1-1} \end{center}

Los trozos de código pueden tener nombre y opciones, se establecen en la cabecera de la forma 
` ```{r nombre, op1, op2} ` 
(en el caso anterior no se muestra el código, al haber empleado ` ```{r, echo=FALSE} `). 
Para un listado de las opciones disponibles ver <http://yihui.name/knitr/options>.

En *RStudio* se puede pulsar en los iconos a la derecha del chunk para establecer opciones, 
ejecutar todo el código anterior o sólo el correspondiente trozo.


### Inclusión de tablas

Las tablas en markdown son de la forma:
```
| First Header  | Second Header |
| ------------- | ------------- |
| Row1 Cell1    | Row1 Cell2    |
| Row2 Cell1    | Row2 Cell2    |
```
Por ejemplo:

Variable  | Descripción
--------  | -----------------------------------------------------------
mpg	  |  Millas / galón (EE.UU.) 
cyl	  |  Número de cilindros
disp  |	 Desplazamiento (pulgadas cúbicas)
hp	  |  Caballos de fuerza bruta
drat  |  Relación del eje trasero
wt    |  Peso (miles de libras)
qsec  |  Tiempo de 1/4 de milla
vs    |  Cilindros en V/Straight (0 = cilindros en V, 1 = cilindros en línea)
am    |  Tipo de transmisión (0 = automático, 1 = manual)
gear  |  Número de marchas (hacia adelante)
carb  |  Número de carburadores


Para convertir resultados de R en tablas de una forma simple se puede emplear la función `ktable` del paquete *knitr*:

```r
knitr::kable(
  head(mtcars), 
  caption = "Una kable knitr"
)
```

\begin{table}[t]

\caption{(\#tab:kable)Una kable knitr}
\centering
\begin{tabular}{l|r|r|r|r|r|r|r|r|r|r|r}
\hline
  & mpg & cyl & disp & hp & drat & wt & qsec & vs & am & gear & carb\\
\hline
Mazda RX4 & 21.0 & 6 & 160 & 110 & 3.90 & 2.620 & 16.46 & 0 & 1 & 4 & 4\\
\hline
Mazda RX4 Wag & 21.0 & 6 & 160 & 110 & 3.90 & 2.875 & 17.02 & 0 & 1 & 4 & 4\\
\hline
Datsun 710 & 22.8 & 4 & 108 & 93 & 3.85 & 2.320 & 18.61 & 1 & 1 & 4 & 1\\
\hline
Hornet 4 Drive & 21.4 & 6 & 258 & 110 & 3.08 & 3.215 & 19.44 & 1 & 0 & 3 & 1\\
\hline
Hornet Sportabout & 18.7 & 8 & 360 & 175 & 3.15 & 3.440 & 17.02 & 0 & 0 & 3 & 2\\
\hline
Valiant & 18.1 & 6 & 225 & 105 & 2.76 & 3.460 & 20.22 & 1 & 0 & 3 & 1\\
\hline
\end{tabular}
\end{table}

Otros paquetes proporcionan opciones adicionales: *xtable*, *stargazer*, *pander*, *tables* y *ascii*.

### Extracción del código R

Para generar un fichero con el código R se puede emplear la función `purl` del paquete *knitr*. Por ejemplo:
```
purl("8-Informes.Rmd")
```
Si se quiere además el texto rmarkdown como comentarios tipo *spin*, se puede emplear:
```
purl("8-Informes.Rmd", documentation = 2)
```

Spin
----

Una forma rápida de crear este tipo de informes a partir de un fichero de código R es emplear la funcion 
`spin` del paquete *knitr* (ver p.e. <http://yihui.name/knitr/demo/stitch>).

Para ello se debe comentar todo lo que no sea código R de una forma especial:

-   El texto rmarkdown se comenta con `#' `. Por ejemplo:
    ```
    #' # Este es un título de primer nivel
    #' ## Este es un título de segundo nivel
    ```
-   Las opciones de un trozo de código se comentan con `#+`. Por ejemplo:
    ```
    #+ setup, include=FALSE
    opts_chunk$set(comment=NA, prompt=TRUE, dev='svg', fig.height=6, fig.width=6)
    ```

Para generar el informe se puede emplear la funcion `spin` del paquete *knitr*. Por ejemplo: `spin("Ridge_Lasso.R"))`.
También se podría abrir directamente el informe generado:
```
browseURL(url = knitr::spin("Ridge_Lasso.R"))
```
Pero puede ser recomendable renderizarlo con rmarkdown:
```
library(rmarkdown)
browseURL(url = render(knitr::spin("Ridge_Lasso.R", knit = FALSE)))
```
En *RStudio* basta con pulsar "Ctrl + Shift + K" o seleccionar *File > Knit Document* (en las últimas versiones también *File > Compile Notebook* o hacer clic en el icono correspondiente).

