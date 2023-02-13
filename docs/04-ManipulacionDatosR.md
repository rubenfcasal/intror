# Manipulación de datos {#manipulacion}




<!-- 
---
title: "Manipulación de datos"
author: "Estadística"
date: "Grado en Inteligencia Artificial (UDC)"
output: 
  bookdown::html_document2:
    pandoc_args: ["--number-offset", "3,0"]
    toc: yes 
    # mathjax: local            # copia local de MathJax, hay que establecer:
    # self_contained: false     # las dependencias se guardan en ficheros externos 
  bookdown::pdf_document2:
    keep_tex: yes
    toc: yes 
---

bookdown::preview_chapter("04-ManipulacionDatosR.Rmd")
knitr::purl("04-ManipulacionDatosR.Rmd", documentation = 2)
knitr::spin("04-ManipulacionDatosR.R",knit = FALSE)
-->

El punto de partida para (casi) cualquier estudio estadístico son los datos.

> "In God we trust, all others must bring data."
>
> "Without data, you’re just another person with an opinion."
>
> --- W. E. Deming

Como ya se comentó anteriormente, el objeto de R en el que se suele almacenar un conjunto de datos es el `data.frame` (ver Sección \@ref(data-frames)).
En este capítulo se muestran las herramientas básicas disponibles en el paquete base de R para la manipulación de conjuntos de datos.
Otras alternativas más avanzadas pero que pueden resultar de gran interés son las que proporcionan las librerías `tidyverse` (ver Apéndice) o `data.table`, aunque pueden requerir de cierto tiempo de aprendizaje y no serían muy recomendables para usuarios que se están iniciando en R.

Como también se mostró en capítulos anteriores, podemos crear conjuntos de datos mediante código (Sección \@ref(data-frames)) o cargar bases de datos disponibles en librerías de R con el comando `data()` (Sección \@ref(data-paquetes)). 
Sin embargo, normalmente importaremos los datos de un archivo externo.


## Importación y exportación de datos {#importar-exportar}

Como ya se comentó en la Sección \@ref(load-save) podemos cargar y almacenar datos en ficheros (normalmente con extensión *.RData* o *.rda*) con las funciones `load()` y `save()` que emplean el formato por defecto de R (datos binarios comprimidos).
Por ejemplo:

```r
res <- load("datos/empleados.RData")
res # Devuelve el nombre de los objetos cargados en memoria
```

```
## [1] "empleados"
```
En estos casos hay que tener en cuenta que, aunque es lo habitual, el nombre del conjunto de datos puede no coincidir con el nombre del archivo, incluso puede contener varios objetos^[Para almacenar un único objeto de forma que se pueda cargar posteriormente especificando el nombre, se pueden emplear las funciones `saveRDS()` y `readRDS()`.].

```r
# Guardar
save(empleados, file = "datos/empleados_new.RData")
```

Además R es capaz de importar datos externos en casi cualquier formato (aunque puede requerir instalar paquetes adicionales), entre ellos:

-   Archivos de texto (con distintos formatos).

-   Archivos en otros formatos: Excel, SPSS...

-   Bases de datos relacionales: MySQL, SQLite, PostgreSQL...

-   Formatos web: HTML, XML, JSON...

A continuación se muestran algunos ejemplos empleando código. 
Adicionalmente en RStudio se puede emplear los submenús en *File > Import Dataset* (se previsualizará el resultado y escribirá el código por nosotros).


### Lectura de archivos de texto {#archivos-texto}

En R para leer archivos de texto se suele utilizar la función `read.table()`:

```r
read.table(file, header = FALSE, sep = "", dec = ".", ...)  
```
Los principales argumentos son:

-   `header`: indica si el fichero tiene cabecera (`header = TRUE`) o no
    (`header = FALSE`). Por defecto toma el valor `header = FALSE`.

-   `sep`: carácter separador de columnas que por defecto es un espacio
    en blanco (`sep = ""`). Otras opciones serían: `sep = ";"` si el separador es
    un ";", `sep = "\t"` si el separador es una tabulación, etc.

-   `dec`: carácter utilizado en el fichero para los números decimales.
    Por defecto se establece `dec = "."`. Si los decimales vienen dados
    por "," se utiliza `dec = ","`

Para más detalles y argumentos adicionales ver `help(read.table)`.

Por ejemplo, supongamos que en el subdirectorio *datos* del directorio actual de trabajo está el fichero *empleados.txt* (con valores separados por espacios y con los los nombres de las columnas en la primera línea). 
Para leer este fichero y almacenarlo en un data.frame podemos emplear el siguiente código:


```r
# Session > Set Working Directory > To Source...?
datos <- read.table(file = "datos/empleados.txt", header = TRUE)
# head(datos)
str(datos)
```

```
## 'data.frame':	474 obs. of  10 variables:
##  $ id      : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ sexo    : chr  "Hombre" "Hombre" "Mujer" "Mujer" ...
##  $ fechnac : chr  "2/3/1952" "5/23/1958" "7/26/1929" "4/15/1947" ...
##  $ educ    : int  15 16 12 8 15 15 15 12 15 12 ...
##  $ catlab  : chr  "Directivo" "Administrativo" "Administrativo" "Administrativo" ...
##  $ salario : num  57000 40200 21450 21900 45000 ...
##  $ salini  : int  27000 18750 12000 13200 21000 13500 18750 9750 12750 13500 ...
##  $ tiempemp: int  98 98 98 98 98 98 98 98 98 98 ...
##  $ expprev : int  144 36 381 190 138 67 114 0 115 244 ...
##  $ minoria : chr  "No" "No" "No" "No" ...
```


Si el fichero estuviese en el directorio *c:\\datos* bastaría con especificar
`file = "c:/datos/empleados.txt"`.

Además están disponibles otras funciones con valores por defecto de los parámetros adecuados para otras situaciones. 
Por ejemplo, para ficheros separados por tabuladores se puede utilizar `read.delim()` o `read.delim2()` (ver también la Sección \@ref(datos-excel)):

```r
read.delim(file, header = TRUE, sep = "\t", dec = ".")
read.delim2(file, header = TRUE, sep = "\t", dec = ",")
```


Para leer archivos de texto en distintos formatos también se puede emplear el paquete [`readr`](https://readr.tidyverse.org) 
(colección [`tidyverse`](https://www.tidyverse.org/)), para lo que se recomienda
consultar el [Capítulo 11](https://r4ds.had.co.nz/data-import.html) del libro [R for Data Science](http://r4ds.had.co.nz).


### Importación desde SPSS

Podemos importar ficheros de datos en formato SPSS (extensión *.sav*) empleando la función `read.spss()` de la librería `foreign`.
Por ejemplo:


```r
library(foreign)
datos <- read.spss(file = "datos/Employee data.sav", to.data.frame = TRUE)
# head(datos)
str(datos)
```

```
## 'data.frame':	474 obs. of  10 variables:
##  $ id      : num  1 2 3 4 5 6 7 8 9 10 ...
##  $ sexo    : Factor w/ 2 levels "Hombre","Mujer": 1 1 2 2 1 1 1 2 2 2 ...
##  $ fechnac : num  1.17e+10 1.19e+10 1.09e+10 1.15e+10 1.17e+10 ...
##  $ educ    : Factor w/ 10 levels "8","12","14",..: 4 5 2 1 4 4 4 2 4 2 ...
##  $ catlab  : Factor w/ 3 levels "Administrativo",..: 3 1 1 1 1 1 1 1 1 1 ...
##  $ salario : Factor w/ 221 levels "15750","15900",..: 179 137 28 31 150 101 121 31 71 45 ...
##  $ salini  : Factor w/ 90 levels "9000","9750",..: 60 42 13 21 48 23 42 2 18 23 ...
##  $ tiempemp: Factor w/ 36 levels "63","64","65",..: 36 36 36 36 36 36 36 36 36 36 ...
##  $ expprev : Factor w/ 208 levels "Ausente","10",..: 38 131 139 64 34 181 13 1 14 91 ...
##  $ minoria : Factor w/ 2 levels "No","Sí": 1 1 1 1 1 1 1 1 1 1 ...
##  - attr(*, "variable.labels")= Named chr [1:10] "Código de empleado" "Sexo" "Fecha de nacimiento" "Nivel educativo" ...
##   ..- attr(*, "names")= chr [1:10] "id" "sexo" "fechnac" "educ" ...
##  - attr(*, "codepage")= int 1252
```

También se puede emplear la función `spss.get()` del paquete `Hmisc`, lo cual puede ser recomendable si alguna de las variables contiene fechas.


### Importación desde Excel {#datos-excel}

Se pueden leer fichero de Excel (con extensión *.xlsx*) utilizando por ejemplo los paquetes [`openxlsx`](https://cran.r-project.org/web/packages/openxlsx/index.html), [`readxl`](https://readxl.tidyverse.org) (colección [`tidyverse`](https://www.tidyverse.org/)), `XLConnect` o 
[`RODBC`](https://cran.r-project.org/web/packages/RODBC/index.html) (este paquete se empleará más adelante para acceder a bases de datos),
entre otros.

Sin embargo, un procedimiento sencillo consiste en  exportar los datos desde Excel a un archivo de texto separado por tabuladores (extensión *.csv*).
Por ejemplo, supongamos que queremos leer el fichero *coches.xls*:

-   Desde Excel se selecciona el menú
    `Archivo -> Guardar como -> Guardar como` y en `Tipo` se escoge la opción de
    archivo CSV. De esta forma se guardarán los datos en el archivo
    *coches.csv*.

-   El fichero *coches.csv* es un fichero de texto plano (se puede
    editar con Notepad), con cabecera, las columnas separadas por ";", y
    siendo "," el carácter decimal.

-   Por lo tanto, la lectura de este fichero se puede hacer con:

    
    ```r
    datos <- read.table("coches.csv", header = TRUE, sep = ";", dec = ",")
    ```

Otra posibilidad es utilizar la función `read.csv2`, que es
una adaptación de la función general `read.table` con las siguientes
opciones:

```r
read.csv2(file, header = TRUE, sep = ";", dec = ",")
```

Por lo tanto, la lectura del fichero *coches.csv* se puede hacer de modo
más directo con:

```r
datos <- read.csv2("coches.csv")
```

### Exportación de datos

Puede ser de interés la exportación de datos para que puedan leídos con otros programas. 
Para ello, se puede emplear la función `write.table()`. 
Esta función es similar, pero operando en sentido inverso, a `read.table()` (Sección \@ref(archivos-texto)).

Veamos un ejemplo:


```r
tipo <- c("A", "B", "C")
longitud <- c(120.34, 99.45, 115.67)
datos <- data.frame(tipo, longitud)
datos
```

```
##   tipo longitud
## 1    A   120.34
## 2    B    99.45
## 3    C   115.67
```
Para guardar el data.frame `datos` en un fichero de texto se
puede utilizar:

```r
write.table(datos, file = "datos.txt")
```
Otra posibilidad es utilizar la función:

```r
write.csv2(datos, file = "datos.csv")
```
y se creará el fichero *datos.csv* que se puede abrir directamente con Excel.


## Manipulación de datos {#manipulacion-datos}

Una vez cargada una (o varias) bases de datos hay una serie de operaciones que serán de interés para el tratamiento de datos: 

-   Operaciones con variables: 
    - crear
    - recodificar (e.g. categorizar)
    - ...

-   Operaciones con casos:
    - ordenar
    - filtrar
    - ...


A continuación se tratan algunas operaciones *básicas*.

### Operaciones con variables {#op-var}

#### Creación (y eliminación) de variables

Consideremos de nuevo la
base de datos `cars` incluida en el paquete `datasets`:

```r
data(cars)
# str(cars)
head(cars)
```

```
##   speed dist
## 1     4    2
## 2     4   10
## 3     7    4
## 4     7   22
## 5     8   16
## 6     9   10
```

Utilizando el comando `help(cars)`
se obtiene que `cars` es un data.frame con 50 observaciones y dos
variables:

-   `speed`: Velocidad (millas por hora)

-   `dist`: tiempo hasta detenerse (pies)

Recordemos que, para acceder a la variable `speed` se puede
hacer directamente con su nombre o bien utilizando notación
"matricial".

```r
cars$speed
```

```
##  [1]  4  4  7  7  8  9 10 10 10 11 11 12 12 12 12 13 13 13 13 14 14 14 14 15 15
## [26] 15 16 16 17 17 17 18 18 18 18 19 19 19 20 20 20 20 20 22 23 24 24 24 24 25
```

```r
cars[, 1]  # Equivalente
```

```
##  [1]  4  4  7  7  8  9 10 10 10 11 11 12 12 12 12 13 13 13 13 14 14 14 14 15 15
## [26] 15 16 16 17 17 17 18 18 18 18 19 19 19 20 20 20 20 20 22 23 24 24 24 24 25
```
Supongamos ahora que queremos transformar la variable original `speed`
(millas por hora) en una nueva variable `velocidad` (kilómetros por
hora) y añadir esta nueva variable al data.frame `cars`.
La transformación que permite pasar millas a kilómetros es
`kilómetros=millas/0.62137` que en R se hace directamente con:


```r
cars$speed/0.62137
```

 Finalmente, incluimos la nueva variable que llamaremos
`velocidad` en `cars`:

```r
cars$velocidad <- cars$speed / 0.62137
head(cars)
```

```
##   speed dist velocidad
## 1     4    2  6.437388
## 2     4   10  6.437388
## 3     7    4 11.265430
## 4     7   22 11.265430
## 5     8   16 12.874777
## 6     9   10 14.484124
```

También transformaremos la variable `dist` (en pies) en una nueva
variable `distancia` (en metros). Ahora la transformación deseada es
`metros=pies/3.2808`:


```r
cars$distancia <- cars$dis / 3.2808
head(cars)
```

```
##   speed dist velocidad distancia
## 1     4    2  6.437388 0.6096074
## 2     4   10  6.437388 3.0480371
## 3     7    4 11.265430 1.2192148
## 4     7   22 11.265430 6.7056815
## 5     8   16 12.874777 4.8768593
## 6     9   10 14.484124 3.0480371
```

 Ahora, eliminaremos las variables originales `speed` y
`dist`, y guardaremos el data.frame resultante con el nombre `coches`.
En primer lugar, veamos varias formas de acceder a las variables de
interés:

```r
cars[, c(3, 4)]
cars[, c("velocidad", "distancia")]
cars[, -c(1, 2)]
```

Utilizando alguna de las opciones anteriores se obtiene el `data.frame`
deseado:

```r
coches <- cars[, c("velocidad", "distancia")]
# head(coches)
str(coches)
```

```
## 'data.frame':	50 obs. of  2 variables:
##  $ velocidad: num  6.44 6.44 11.27 11.27 12.87 ...
##  $ distancia: num  0.61 3.05 1.22 6.71 4.88 ...
```

Finalmente los datos anteriores podrían ser guardados en un fichero
exportable a Excel con el siguiente comando:

```r
write.csv2(coches, file = "coches.csv")
```

#### Recodificación de variables

Con el comando `cut()` podemos crear variables categóricas a partir de variables numéricas.
El parámetro `breaks` permite especificar los intervalos para la discretización, puede ser un vector con los extremos de los intervalos o un entero con el número de intervalos.
Por ejemplo, para categorizar la variable `cars$speed` en tres intervalos equidistantes podemos emplear^[Aunque si el objetivo es obtener las frecuencias de cada intervalo puede ser más eficiente emplear `hist()` con `plot = FALSE`.]:


```r
fspeed <- cut(cars$speed, 3, labels = c("Baja", "Media", "Alta"))
table(fspeed)
```

```
## fspeed
##  Baja Media  Alta 
##    11    24    15
```

Para categorizar esta variable en tres niveles con aproximadamente el mismo número de observaciones podríamos combinar esta función con `quantile()`:


```r
breaks <- quantile(cars$speed, probs = seq(0, 1, len = 4))
fspeed <- cut(cars$speed, breaks, labels = c("Baja", "Media", "Alta"))
table(fspeed)
```

```
## fspeed
##  Baja Media  Alta 
##    17    16    15
```

Para otro tipo de recodificaciones podríamos emplear la función `ifelse()` vectorial:


```r
fspeed <- ifelse(cars$speed < 15, "Baja", "Alta")
fspeed <- factor(fspeed, levels = c("Baja", "Alta"))
table(fspeed)
```

```
## fspeed
## Baja Alta 
##   23   27
```

Alternativamente en el caso de dos niveles podríamos emplear directamente la función `factor()`:


```r
fspeed <- factor(cars$speed >= 15, labels = c("Baja", "Alta")) # levels = c("FALSE", "TRUE")
table(fspeed)
```

```
## fspeed
## Baja Alta 
##   23   27
```

En el caso de múltiples niveles se podría emplear `ifelse()` anidados o la función [`recode()`](https://www.rdocumentation.org/packages/car/versions/3.0-9/topics/recode) del paquete `car`.

Para acceder directamente a las variables de un data.frame podríamos emplear la función `attach()` para añadirlo a la ruta de búsqueda y `detach()` al finalizar.
Sin embargo esta forma de proceder puede causar numerosos inconvenientes, especialmente al modificar la base de datos, por lo que la recomendación sería emplear `with()`.
Por ejemplo:


```r
fspeed <- with(cars, ifelse(speed < 10, "Baja",
                 ifelse(speed < 20, "Media", "Alta")))
fspeed <- factor(fspeed, levels = c("Baja", "Media", "Alta"))
table(fspeed)
```

```
## fspeed
##  Baja Media  Alta 
##     6    32    12
```

Para manipular factores (variables cualitativas) pueden resultar de interés las herramientas en el paquete [`forcats`](https://forcats.tidyverse.org) de la colección [`tidyverse`](https://tidyverse.org).


### Operaciones con casos {#op-casos}

#### Ordenación

Continuemos con el data.frame `cars`. 
Se puede comprobar que los datos disponibles están ordenados por
los valores de `speed`. A continuación haremos la ordenación utilizando
los valores de `dist`. Para ello utilizaremos el conocido como vector de
índices de ordenación.
Este vector establece el orden en que tienen que ser elegidos los
elementos para obtener la ordenación deseada. 
Veamos un ejemplo sencillo:

```r
x <- c(2.5, 4.3, 1.2, 3.1, 5.0) # valores originales
ii <- order(x)
ii    # vector de ordenación
```

```
## [1] 3 1 4 2 5
```

```r
x[ii] # valores ordenados
```

```
## [1] 1.2 2.5 3.1 4.3 5.0
```
En el caso de vectores, el procedimiento anterior se podría
hacer directamente con: 

```r
sort(x)
```

Sin embargo, para ordenar data.frames será necesario la utilización del
vector de índices de ordenación. A continuación, los datos de `cars`
ordenados por `dist`:

```r
ii <- order(cars$dist) # Vector de índices de ordenación
cars2 <- cars[ii, ]    # Datos ordenados por dist
head(cars2)
```

```
##    speed dist velocidad distancia
## 1      4    2  6.437388 0.6096074
## 3      7    4 11.265430 1.2192148
## 2      4   10  6.437388 3.0480371
## 6      9   10 14.484124 3.0480371
## 12    12   14 19.312165 4.2672519
## 5      8   16 12.874777 4.8768593
```

#### Filtrado

El filtrado de datos consiste en elegir un subconjunto que cumpla determinadas condiciones. 
Para ello se puede utilizar la función [`subset()` ](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/subset) (que además permite seleccionar variables).

A continuación se muestran un par de ejemplos:

```r
subset(cars, dist > 85) # datos con dis>85
```

```
##    speed dist velocidad distancia
## 47    24   92  38.62433  28.04194
## 48    24   93  38.62433  28.34674
## 49    24  120  38.62433  36.57644
```

```r
subset(cars, speed > 10 & speed < 15 & dist > 45) # speed en (10,15) y dist>45
```

```
##    speed dist velocidad distancia
## 19    13   46  20.92151  14.02097
## 22    14   60  22.53086  18.28822
## 23    14   80  22.53086  24.38430
```

También se pueden hacer el filtrado empleando directamente los
correspondientes vectores de índices:


```r
ii <- cars$dist > 85
cars[ii, ]   # dis>85
```

```
##    speed dist velocidad distancia
## 47    24   92  38.62433  28.04194
## 48    24   93  38.62433  28.34674
## 49    24  120  38.62433  36.57644
```

```r
ii <- cars$speed > 10 & cars$speed < 15 & cars$dist > 45
cars[ii, ]  # speed en (10,15) y dist>45
```

```
##    speed dist velocidad distancia
## 19    13   46  20.92151  14.02097
## 22    14   60  22.53086  18.28822
## 23    14   80  22.53086  24.38430
```

En este caso también puede ser de utilidad la función [`which()` ](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/which):


```r
it <- which(ii)
str(it)
```

```
##  int [1:3] 19 22 23
```

```r
cars[it, 1:2]
```

```
##    speed dist
## 19    13   46
## 22    14   60
## 23    14   80
```

```r
# rownames(cars[it, 1:2])

id <- which(!ii)
str(cars[id, 1:2])
```

```
## 'data.frame':	47 obs. of  2 variables:
##  $ speed: num  4 4 7 7 8 9 10 10 10 11 ...
##  $ dist : num  2 10 4 22 16 10 18 26 34 17 ...
```

```r
# Se podría p.e. emplear cars[id, ] para predecir cars[it, ]$speed
# ?which.min
```

### Operaciones con tablas de datos {#op-tablas}

El paquete base de R dispone de diversas herramientas para realizar distintos tipos de operaciones, como:

* Añadir casos o variables:

    - [`rbind()` ](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/rbind): combina vectores, matrices, arrays o data.frames por filas.
    
    - [`cbind()` ](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/cbind): Idem por columnas.

* Combinar tablas:

    - [`match(x, table)`](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/match) devuelve un vector (de la misma longitud que `x`)  con las (primeras) posiciones de coincidencia de `x` en `table` (o `NA`, por defecto, si no hay coincidencia).
    
    - [`pmatch(x, table, ...)`](https://www.rdocumentation.org/packages/base/versions/3.6.1/topics/pmatch): similar al anterior pero con coincidencias parciales de cadenas de texto. 

    Estos operadores devuelven un índice con el que se pueden añadir variables de una segunda tabla. Para realizar consultas combinando tablas puede ser más cómodo el operador `%in%` (`?'%in%'`).

<!-- Pendiente: Ejemplos -->

Sin embargo, como se muestra en el Apéndice \@ref(dplyr) puede resultar más cómodo emplear los paquetes [`dplyr`](https://dplyr.tidyverse.org) y [`tidyr`](https://tidyr.tidyverse.org) de la colección [`tidyverse`](https://tidyverse.org).
