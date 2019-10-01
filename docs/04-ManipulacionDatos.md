Manipulación de datos
=====================






Lectura, importación y exportación de datos
-------------------------------------------


La mayoría de los estudios estadísticos
requieren disponer de un conjunto de datos. Además de la introducción directa, `R` es capaz de
importar datos externos en múltiples formatos:

-   bases de datos disponibles en librerías de `R`

-   archivos de texto en formato ASCII

-   archivos en otros formatos: Excel, SPSS, ...

-   bases de datos relacionales: MySQL, Oracle, ...

-   formatos web: HTML, XML, JSON, ...

-   ....


### Introducción directa

Si los datos son pocos, se
pueden teclear directamente en un vector o en un data.frame. Por ejemplo:


```r
producto <- c("A", "B", "C")
precio <- c(1200, 560, 740)
datos <- data.frame(producto, precio)
datos
```

```
##   producto precio
## 1        A   1200
## 2        B    560
## 3        C    740
```
En ocasiones, tanto para matrices como para data.frames, será de
utilidad la función `edit`. Por ejemplo, una sentencia como:


```r
datos2 <- edit(datos)
```

abre una hoja de cálculo “muy básica” con la que podremos editar los
elementos del data.frame, cambiar sus dimensiones añadiendo filas y/o
columnas, ...

**Nota**: Hay que destacar que `datos` tiene que existir antes de emplear la función `edit`. 
Para modificar directamente el objeto puede ser más cómodo emplear la función `fix()`.


### Lectura desde paquetes

El programa `R`
dispone de múltiples conjuntos de datos. Al teclear:


```r
data()
```

se obtiene un listado de las bases de datos disponibles.

    Data sets in package "datasets":

    AirPassengers           Monthly Airline Passenger Numbers 1949-1960
    BJsales                 Sales Data with Leading Indicator
    BJsales.lead (BJsales)
                            Sales Data with Leading Indicator
    BOD                     Biochemical Oxygen Demand
    CO2                     Carbon Dioxide Uptake in Grass Plants
    ChickWeight             Weight versus age of chicks on different diets

    .....

 Para cargar una base de datos concreta se utiliza el comando
`data(nombre)` (aunque en algunos casos se cargan automáticamente). 
Por ejemplo, la base de datos llamada `cars`
se carga con:


```r
data(cars)
```

Una vez cargado el objeto `cars` (`R` lo carga como un data.frame),
se podrán utilizar las correspondientes sentencias para realizar
resúmenes numéricos y gráficos.


```r
head(cars)  # Primeros valores
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


```r
help(cars)  # Obtención de ayuda
```

    ...

    Speed and Stopping Distances of Cars

    Description:
         The data give the speed of cars and the distances taken to stop.
         Note that the data were recorded in the 1920s.

    ...  

    Format:
         A data frame with 50 observations on 2 variables.
           [,1]  speed  numeric  Speed (mph)            
           [,2]  dist   numeric  Stopping distance (ft) 
          
    Source:
         Ezekiel, M. (1930) _Methods of Correlation Analysis_.  Wiley.
         
    ...  




```r
names(cars)      # Nombres
```

```
## [1] "speed" "dist"
```

```r
cars$speed[1:10] # Primeros 10 valores de velocidad
```

```
##  [1]  4  4  7  7  8  9 10 10 10 11
```

```r
cars$dist[1:10] # Primeros 10 valores de distancia
```

```
##  [1]  2 10  4 22 16 10 18 26 34 17
```

```r
names(cars) <- c("velocidad", "distancia")  # Nombres en español
head(cars)
```

```
##   velocidad distancia
## 1         4         2
## 2         4        10
## 3         7         4
## 4         7        22
## 5         8        16
## 6         9        10
```

```r
summary(cars)   # Resumen estadístico
```

```
##    velocidad      distancia     
##  Min.   : 4.0   Min.   :  2.00  
##  1st Qu.:12.0   1st Qu.: 26.00  
##  Median :15.0   Median : 36.00  
##  Mean   :15.4   Mean   : 42.98  
##  3rd Qu.:19.0   3rd Qu.: 56.00  
##  Max.   :25.0   Max.   :120.00
```


```r
plot(cars)             # Gráfico de dispersión de los datos
hist(cars$velocidad)    # Histograma de velocidad
boxplot(cars$distancia) # Boxplot de distancia
```

### Lectura de archivos de texto

En `R` para leer archivos de texto se suele utilizar la función `read.table()`.

Supóngase, por ejemplo, que en el directorio actual está el fichero
*empleados.txt*. La lectura de este fichero vendría dada por el código:


```r
datos <- read.table(file = "datos/empleados.txt", header = TRUE)
head(datos)
```

```
##   id   sexo   fechnac educ         catlab salario salini tiempemp expprev
## 1  1 Hombre  2/3/1952   15      Directivo   57000  27000       98     144
## 2  2 Hombre 5/23/1958   16 Administrativo   40200  18750       98      36
## 3  3  Mujer 7/26/1929   12 Administrativo   21450  12000       98     381
## 4  4  Mujer 4/15/1947    8 Administrativo   21900  13200       98     190
## 5  5 Hombre  2/9/1955   15 Administrativo   45000  21000       98     138
## 6  6 Hombre 8/22/1958   15 Administrativo   32100  13500       98      67
##   minoria
## 1      No
## 2      No
## 3      No
## 4      No
## 5      No
## 6      No
```
Si el fichero estuviese en el directorio *c:\\datos* bastaría con especificar
`file = "c:/datos/empleados.txt"`.
Nótese también que para la lectura del fichero anterior se ha
establecido el argumento `header=TRUE` para indicar que la primera línea del
fichero contiene los nombres de las variables.

Los argumentos utilizados habitualmente para esta función son:

-   `header`: indica si el fichero tiene cabecera (`header=TRUE`) o no
    (`header=FALSE`). Por defecto toma el valor `header=FALSE`.

-   `sep`: carácter separador de columnas que por defecto es un espacio
    en blanco (`sep=""`). Otras opciones serían: `sep=","` si el separador es
    un “;”, `sep="*"` si el separador es un “\*”, etc.

-   `dec`: carácter utilizado en el fichero para los números decimales.
    Por defecto se establece `dec = "."`. Si los decimales vienen dados
    por “,” se utiliza `dec = ","`

Resumiendo, los (principales) argumentos por defecto de la función
`read.table` son los que se muestran en la siguiente línea:


```r
read.table(file, header = FALSE, sep = "", dec = ".")  
```

Para más detalles sobre esta función véase
`help(read.table)`.

Estan disponibles otras funciones con valores por defecto de los parámetros 
adecuados para otras situaciones. Por ejemplo, para ficheros separados por tabuladores 
se puede utilizar `read.delim()` o `read.delim2()`:

```r
read.delim(file, header = TRUE, sep = "\t", dec = ".")
read.delim2(file, header = TRUE, sep = "\t", dec = ",")
```


### Importación desde SPSS

El programa `R` permite
lectura de ficheros de datos en formato SPSS (extensión *.sav*) sin
necesidad de tener instalado dicho programa en el ordenador. Para ello
se necesita:

-   cargar la librería `foreign`

-   utilizar la función `read.spss`

Por ejemplo:


```r
library(foreign)
datos <- read.spss(file = "datos/Employee data.sav", to.data.frame = TRUE)
head(datos)
```

```
##   id   sexo     fechnac educ         catlab salario salini tiempemp
## 1  1 Hombre 11654150400   15      Directivo   57000  27000       98
## 2  2 Hombre 11852956800   16 Administrativo   40200  18750       98
## 3  3  Mujer 10943337600   12 Administrativo   21450  12000       98
## 4  4  Mujer 11502518400    8 Administrativo   21900  13200       98
## 5  5 Hombre 11749363200   15 Administrativo   45000  21000       98
## 6  6 Hombre 11860819200   15 Administrativo   32100  13500       98
##   expprev minoria
## 1     144      No
## 2      36      No
## 3     381      No
## 4     190      No
## 5     138      No
## 6      67      No
```

**Nota**: Si hay fechas, puede ser recomendable emplear la función `spss.get()` del paquete `Hmisc`.


### Importación desde Excel

Se pueden leer fichero de
Excel (con extensión *.xls*) utilizando por ejemplo funciones de las librerías `openxlsx`, `XLConnect` o `RODBC`.
Sin embargo, puede ser recomendable exportar los datos desde Excel a un archivo
de texto separado por tabuladores (extensión *.csv*).

Por ejemplo, supongamos que queremos leer el fichero *coches.xls*:

-   Desde Excel se selecciona el menú
    `Archivo -> Guardar como -> Guardar como` y en `Tipo` se escoge la opción de
    archivo CSV. De esta forma se guardarán los datos en el archivo
    *coches.csv*.

-   El fichero *coches.csv* es un fichero de texto plano (se puede
    editar con Notepad), con cabecera, las columnas separadas por “;”, y
    siendo “,” el carácter decimal.

-   Por lo tanto, la lectura de este fichero se puede hacer con:

    
    ```r
    datos <- read.table("datos/coches.csv", header = TRUE, sep = ";", dec = ",")
    ```

 Otra posibilidad es utilizar la función `read.csv2`, que es
una adaptación de la función general `read.table` con la siguiente
configuración.


```r
read.csv2(file, header = TRUE, sep = ";", dec = ",")
```
Por lo tanto, la lectura del fichero *coches.csv* se puede hacer de modo
más directo con

```r
datos <- read.csv2("datos/coches.csv")
```

### Exportación de datos

Es evidente que además de
la lectura e importación de datos también será de gran interés la
exportación de datos para que puedan leídos con otros programas. Para
ello, es de gran utilidad la función `write.table`. Esta función es
similar, pero funcionando en sentido inverso, a la ya vista
`read.table`.

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
Para guardar el data.frame `datos` en un fichero de texto se utiliza se
puede utilizar

```r
write.table(datos, file = "datos.txt")
```
Otra posibilidad es utilizar la función

```r
write.csv2(datos, file = "datos.csv")
```
que dará lugar al fichero *datos.csv* importable directamente desde Excel.


Manipulación de datos
---------------------

Una vez cargada una (o varias) bases
de datos hay una series de operaciones que serán de interés para el
tratamiento de datos: 

-   Operaciones con variables: 
    - creación
    - recodificación (e.g. categorización)
    - ...

-   Operaciones con casos:
    - ordenación
    - filtrado de datos
    - ...

A continuación se tratan las operaciones más básicas.

    
### Operaciones con variables

#### Creación (y eliminación) de variables
Consideremos de nuevo la
base de datos `cars` incluida en el paquete `datasets`.

```r
data(cars)
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
##  [1]  4  4  7  7  8  9 10 10 10 11 11 12 12 12 12 13 13 13 13 14 14 14 14
## [24] 15 15 15 16 16 17 17 17 18 18 18 18 19 19 19 20 20 20 20 20 22 23 24
## [47] 24 24 24 25
```

```r
cars[, 1]  # Equivalente
```

```
##  [1]  4  4  7  7  8  9 10 10 10 11 11 12 12 12 12 13 13 13 13 14 14 14 14
## [24] 15 15 15 16 16 17 17 17 18 18 18 18 19 19 19 20 20 20 20 20 22 23 24
## [47] 24 24 24 25
```
Supongamos ahora que queremos transformar la variable original `speed`
(millas por hora) en una nueva variable `velocidad` (kilómetros por
hora) y añadir esta nueva variable al data.frame `cars`.

La transformación que permite pasar millas a kilómetros es
kilómetros=millas/0.62137 que en `R` se hace directamente con:


```r
cars$speed/0.62137
```

 Finalmente, incluimos la nueva variable que llamaremos
`velocidad` en `cars`.

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
`metros=pies/3.2808`.


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
interés

```r
cars[, c(3, 4)]
cars[, c("velocidad", "distancia")]
cars[, -c(1, 2)]
```

Utilizando alguna de las opciones anteriores se obtiene el data.frame
deseado.

```r
coches <- cars[, c("velocidad", "distancia")]
head(coches)
```

```
##   velocidad distancia
## 1  6.437388 0.6096074
## 2  6.437388 3.0480371
## 3 11.265430 1.2192148
## 4 11.265430 6.7056815
## 5 12.874777 4.8768593
## 6 14.484124 3.0480371
```

Finalmente los datos anteriores podrían ser guardados en un fichero
exportable a Excel con el siguiente comando:

```r
write.csv2(coches, file = "coches.csv")
```

### Operaciones con casos

#### Ordenación
Continuemos con el data.frame `cars`. 
Se puede comprobar que los datos disponibles están ordenados por
los valores de `speed`. A continuación haremos la ordenación utilizando
los valores de `dist`. Para ello utilizaremos el conocido como vector de
índices de ordenación.

El vector de índices de 
ordenación establece el orden en que tienen que ser elegidos los
elementos de un vector para obtener la ordenación deseada. Veamos un
ejemplo sencillo:

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

El filtrado de datos consiste en
elegir una submuestra que cumpla determinadas condiciones. Para ello se
puede utilizar la función `subset`.

A continuación están dos ejemplos para comprender su utilización:

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
correspondientes vectores de índices.

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
