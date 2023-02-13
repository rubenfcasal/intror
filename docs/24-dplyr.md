
# Manipulación de datos con `dplyr` {#dplyr}

Working draft...

En este apéndice se realiza una breve introducción al paquete  [`dplyr`](https://dplyr.tidyverse.org/index.html). 
Para mas información, ver por ejemplo la 'vignette' del paquete   
[Introduction to dplyr](https://cran.rstudio.com/web/packages/dplyr/vignettes/dplyr.html),
o el Capítulo [5 Data transformation](http://r4ds.had.co.nz/transform.html) del libro 
[R for Data Science](http://r4ds.had.co.nz)^[Otra alternativa (más rápida) es emplear
[data.table](https://rdatatable.gitlab.io/data.table).].


## El paquete **dplyr** {#dplyr-pkg}


```r
library(dplyr)
```

[`dplyr`](https://dplyr.tidyverse.org/index.html) 
permite sustituir funciones base de R (como `split()`, `subset()`, 
`apply()`, `sapply()`, `lapply()`, `tapply()` y `aggregate()`)
mediante una "gramática" más sencilla para la manipulación de datos:

- `select()` seleccionar variables/columnas (también `rename()`).

- `mutate()` crear variables/columnas (también `transmute()`).

- `filter()` seleccionar casos/filas (también `slice()`).

- `arrange()`  ordenar o organizar casos/filas.

- `summarise()` resumir valores.

- `group_by()` permite operaciones por grupo empleando el concepto
"dividir-aplicar-combinar" (`ungroup()` elimina el agrupamiento).

Puede trabajar con conjuntos de datos en distintos formatos:
     
- `data.frame`, `tibble`, `data.table`...

- bases de datos relacionales (lenguaje SQL); [dbplyr](https://dbplyr.tidyverse.org).

- bases de datos *Hadoop*: [`sparklyr`](https://spark.rstudio.com).
    

En lugar de operar sobre vectores como las funciones base,
opera sobre objetos de este tipo (solo nos centraremos en `data.frame`).

### Datos de ejemplo

El fichero *empleados.RData* contiene datos de empleados de un banco.
Supongamos por ejemplo que estamos interesados en estudiar si hay
discriminación por cuestión de sexo o raza.




## Operaciones con variables (columnas) {#dplyr-variables}

### Seleccionar variables con **select()**


```r
emplea2 <- select(empleados, id, sexo, minoria, tiempemp, salini, salario)
head(emplea2)
```

```
##   id   sexo minoria tiempemp salini salario
## 1  1 Hombre      No       98  27000   57000
## 2  2 Hombre      No       98  18750   40200
## 3  3  Mujer      No       98  12000   21450
## 4  4  Mujer      No       98  13200   21900
## 5  5 Hombre      No       98  21000   45000
## 6  6 Hombre      No       98  13500   32100
```

Se puede cambiar el nombre (ver también *?rename()*)


```r
head(select(empleados, sexo, noblanca = minoria, salario))
```

```
##     sexo noblanca salario
## 1 Hombre       No   57000
## 2 Hombre       No   40200
## 3  Mujer       No   21450
## 4  Mujer       No   21900
## 5 Hombre       No   45000
## 6 Hombre       No   32100
```

Se pueden emplear los nombres de variables como índices:


```r
head(select(empleados, sexo:salario))
```

```
##     sexo    fechnac educ         catlab salario
## 1 Hombre 1952-02-03   15      Directivo   57000
## 2 Hombre 1958-05-23   16 Administrativo   40200
## 3  Mujer 1929-07-26   12 Administrativo   21450
## 4  Mujer 1947-04-15    8 Administrativo   21900
## 5 Hombre 1955-02-09   15 Administrativo   45000
## 6 Hombre 1958-08-22   15 Administrativo   32100
```

```r
head(select(empleados, -(sexo:salario)))
```

```
##   id salini tiempemp expprev minoria     sexoraza
## 1  1  27000       98     144      No Blanca varón
## 2  2  18750       98      36      No Blanca varón
## 3  3  12000       98     381      No Blanca mujer
## 4  4  13200       98     190      No Blanca mujer
## 5  5  21000       98     138      No Blanca varón
## 6  6  13500       98      67      No Blanca varón
```

Hay opciones para considerar distintos criterios: `starts_with()`, `ends_with()`, 
`contains()`, `matches()`, `one_of()` (ver `?select`).


```r
head(select(empleados, starts_with("s")))
```

```
##     sexo salario salini     sexoraza
## 1 Hombre   57000  27000 Blanca varón
## 2 Hombre   40200  18750 Blanca varón
## 3  Mujer   21450  12000 Blanca mujer
## 4  Mujer   21900  13200 Blanca mujer
## 5 Hombre   45000  21000 Blanca varón
## 6 Hombre   32100  13500 Blanca varón
```

### Generar nuevas variables con **mutate()**


```r
head(mutate(emplea2, incsal = salario - salini, tsal = incsal/tiempemp ))
```

```
##   id   sexo minoria tiempemp salini salario incsal      tsal
## 1  1 Hombre      No       98  27000   57000  30000 306.12245
## 2  2 Hombre      No       98  18750   40200  21450 218.87755
## 3  3  Mujer      No       98  12000   21450   9450  96.42857
## 4  4  Mujer      No       98  13200   21900   8700  88.77551
## 5  5 Hombre      No       98  21000   45000  24000 244.89796
## 6  6 Hombre      No       98  13500   32100  18600 189.79592
```


## Operaciones con casos (filas) {#dplyr-casos}

### Seleccionar casos con **filter()**


```r
head(filter(emplea2, sexo == "Mujer", minoria == "Sí"))
```

```
##   id  sexo minoria tiempemp salini salario
## 1 14 Mujer      Sí       98  16800   35100
## 2 23 Mujer      Sí       97  11100   24000
## 3 24 Mujer      Sí       97   9000   16950
## 4 25 Mujer      Sí       97   9000   21150
## 5 40 Mujer      Sí       96   9000   19200
## 6 41 Mujer      Sí       96  11550   23550
```

### Organizar casos con **arrange()**


```r
head(arrange(emplea2, salario))
```

```
##    id  sexo minoria tiempemp salini salario
## 1 378 Mujer      No       70  10200   15750
## 2 338 Mujer      No       74  10200   15900
## 3  90 Mujer      No       92   9750   16200
## 4 224 Mujer      No       82  10200   16200
## 5 411 Mujer      No       68  10200   16200
## 6 448 Mujer      Sí       66  10200   16350
```

```r
head(arrange(emplea2, desc(salini), salario))
```

```
##    id   sexo minoria tiempemp salini salario
## 1  29 Hombre      No       96  79980  135000
## 2 343 Hombre      No       73  60000  103500
## 3 205 Hombre      No       83  52500   66750
## 4 160 Hombre      No       86  47490   66000
## 5 431 Hombre      No       66  45000   86250
## 6  32 Hombre      No       96  45000  110625
```

### Resumir valores con **summarise()**


```r
summarise(empleados, sal.med = mean(salario), n = n())
```

```
##    sal.med   n
## 1 34419.57 474
```

### Agrupar casos con **group_by()**


```r
summarise(group_by(empleados, sexo, minoria), sal.med = mean(salario), n = n())
```

```
## `summarise()` has grouped output by 'sexo'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 4 x 4
## # Groups:   sexo [2]
##   sexo   minoria sal.med     n
##   <fct>  <fct>     <dbl> <int>
## 1 Hombre No       44475.   194
## 2 Hombre Sí       32246.    64
## 3 Mujer  No       26707.   176
## 4 Mujer  Sí       23062.    40
```


## Operador *pipe* **%>%** (tubería, redirección) {#dplyr-pipe}

Este operador le permite canalizar la salida de una función a la entrada de otra función. 
`segundo(primero(datos))` se traduce en `datos %>% primero %>% segundo`
(lectura de funciones de izquierda a derecha).

Ejemplos:


```r
empleados %>%  filter(catlab == "Directivo") %>%
          group_by(sexo, minoria) %>%
          summarise(sal.med = mean(salario), n = n())
```

```
## `summarise()` has grouped output by 'sexo'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 3 x 4
## # Groups:   sexo [2]
##   sexo   minoria sal.med     n
##   <fct>  <fct>     <dbl> <int>
## 1 Hombre No       65684.    70
## 2 Hombre Sí       76038.     4
## 3 Mujer  No       47214.    10
```

```r
empleados %>% select(sexo, catlab, salario) %>%
          filter(catlab != "Seguridad") %>%
          group_by(catlab) %>%
          mutate(saldif = salario - mean(salario)) %>%
          ungroup() %>%
          boxplot(saldif ~ sexo*droplevels(catlab), data = .)
abline(h = 0, lty = 2)
```

![](24-dplyr_files/figure-latex/unnamed-chunk-12-1.pdf)<!-- --> 

## Operaciones con tablas de datos {# dplyr-join}

Se emplean funciones `xxx_join()` (ver la documentación del paquete 
[Join two tbls together](https://dplyr.tidyverse.org/reference/join.html),
o la vignette [Two-table verbs](https://dplyr.tidyverse.org/articles/two-table.html)):

- `inner_join()`: devuelve las filas de `x` que tienen valores coincidentes en `y`, 
  y todas las columnas de `x` e `y`. Si hay varias coincidencias entre `x` e `y`, 
  se devuelven todas las combinaciones.
  
- `left_join()`: devuelve todas las filas de `x` y todas las columnas de `x` e `y`. 
  Las filas de `x` sin correspondencia en `y` contendrán `NA` en las nuevas columnas. 
  Si hay varias coincidencias entre `x` e `y`, se devuelven todas las combinaciones
  (duplicando las filas).

    `right_join()` hace lo contrario, devuelve todas las filas de `y`.
    
    `full_join()` devuelve todas las filas de `x` e `y` (duplicando o asignando `NA` si es necesario).

- `semi_join()`: devuelve las filas de `x` que tienen valores coincidentes en `y`, 
  manteniendo sólo las columnas de `x` (al contrario que `inner_join()` no duplica filas).
  
    `anti_join()` hace lo contrario, devuelve las filas sin correspondencia. 

El parámetro `by` determina las variables clave para las correspondencias.
Si no se establece se considerarán todas las que tengan el mismo nombre en ambas tablas.
Se puede establecer a un vector de nombres coincidentes y en caso de que los nombres sean distintos a un vector con nombres de la forma `c("clave_x" = "clave_y")`.

Adicionalmente, si las tablas `x` e `y` tienen las mismas variables, se pueden combinar las observaciones con operaciones de conjuntos:

- `intersect(x, y)`: observaciones en `x` y en `y`.

- `union(x, y)`: observaciones en `x` o `y` no duplicadas.

- `setdiff(x, y)`: observaciones en `x` pero no en `y`.


## Bases de datos con dplyr {#dbplyr}

Para poder usar tablas en bases de datos relacionales con `dplyr` hay que emplear el paquete [dbplyr](https://dbplyr.tidyverse.org) (convierte automáticamente el código de dplyr en consultas SQL).

Algunos enlaces:

- [Best Practices in Working with Databases](https://solutions.posit.co/connections/db)

- [Introduction to dbplyr](https://dbplyr.tidyverse.org/articles/dbplyr.html)

- [Data Carpentry](https://datacarpentry.org/R-ecology-lesson/index.html):
  [SQL databases and R](https://datacarpentry.org/R-ecology-lesson/05-r-and-databases.html), 

- [R and Data – When Should we Use Relational Databases? ](https://intellixus.com/2018/06/29/r-and-data-when-should-we-use-relational-databases)


### Ejemplos

Como ejemplo emplearemos la base de datos de [SQLite Sample Database Tutorial](https://www.sqlitetutorial.net/sqlite-sample-database/), almacenada en el archivo [*chinook.db*](datos/chinook.db).


```r
# install.packages('dbplyr')
library(dplyr)
library(dbplyr)
```

En primer lugar hay que conectar la base de datos:

```r
chinook <- DBI::dbConnect(RSQLite::SQLite(), "datos/chinook.db")
```

Podemos listar las tablas:

```r
src_dbi(chinook)
```

```
## src:  sqlite 3.36.0 [D:\OneDrive - Universidade da Coruña\__Actual\__IA\_intror\datos\chinook.db]
## tbls: albums, artists, customers, employees, genres, invoice_items, invoices,
##   media_types, playlist_track, playlists, sqlite_sequence, sqlite_stat1, tracks
```

Para enlazar una tabla:

```r
invoices <- tbl(chinook, "invoices")
invoices
```

```
## # Source:   table<invoices> [?? x 9]
## # Database: sqlite 3.36.0 [D:\OneDrive - Universidade da
## #   Coruña\__Actual\__IA\_intror\datos\chinook.db]
##    InvoiceId CustomerId InvoiceD~1 Billi~2 Billi~3 Billi~4 Billi~5 Billi~6 Total
##        <int>      <int> <chr>      <chr>   <chr>   <chr>   <chr>   <chr>   <dbl>
##  1         1          2 2009-01-0~ Theodo~ Stuttg~ <NA>    Germany 70174    1.98
##  2         2          4 2009-01-0~ Ullevå~ Oslo    <NA>    Norway  0171     3.96
##  3         3          8 2009-01-0~ Grétry~ Brusse~ <NA>    Belgium 1000     5.94
##  4         4         14 2009-01-0~ 8210 1~ Edmont~ AB      Canada  T6G 2C7  8.91
##  5         5         23 2009-01-1~ 69 Sal~ Boston  MA      USA     2113    13.9 
##  6         6         37 2009-01-1~ Berger~ Frankf~ <NA>    Germany 60316    0.99
##  7         7         38 2009-02-0~ Barbar~ Berlin  <NA>    Germany 10779    1.98
##  8         8         40 2009-02-0~ 8, Rue~ Paris   <NA>    France  75002    1.98
##  9         9         42 2009-02-0~ 9, Pla~ Bordea~ <NA>    France  33000    3.96
## 10        10         46 2009-02-0~ 3 Chat~ Dublin  Dublin  Ireland <NA>     5.94
## # ... with more rows, and abbreviated variable names 1: InvoiceDate,
## #   2: BillingAddress, 3: BillingCity, 4: BillingState, 5: BillingCountry,
## #   6: BillingPostalCode
```

Ojo `[?? x 9]`: de momento no conoce el número de filas.

```r
nrow(invoices)
```

```
## [1] NA
```

Podemos mostrar la consulta SQL correspondiente a una operación:

```r
show_query(head(invoices))
```

```
## <SQL>
## SELECT *
## FROM `invoices`
## LIMIT 6
```

```r
str(head(invoices))
```

```
## List of 2
##  $ src:List of 2
##   ..$ con  :Formal class 'SQLiteConnection' [package "RSQLite"] with 8 slots
##   .. .. ..@ ptr                :<externalptr> 
##   .. .. ..@ dbname             : chr "D:\\OneDrive - Universidade da Coruña\\__Actual\\__IA\\_intror\\datos\\chinook.db"
##   .. .. ..@ loadable.extensions: logi TRUE
##   .. .. ..@ flags              : int 70
##   .. .. ..@ vfs                : chr ""
##   .. .. ..@ ref                :<environment: 0x00000000195e1d60> 
##   .. .. ..@ bigint             : chr "integer64"
##   .. .. ..@ extended_types     : logi FALSE
##   ..$ disco: NULL
##   ..- attr(*, "class")= chr [1:4] "src_SQLiteConnection" "src_dbi" "src_sql" "src"
##  $ ops:List of 4
##   ..$ name: chr "head"
##   ..$ x   :List of 2
##   .. ..$ x   : 'ident' chr "invoices"
##   .. ..$ vars: chr [1:9] "InvoiceId" "CustomerId" "InvoiceDate" "BillingAddress" ...
##   .. ..- attr(*, "class")= chr [1:3] "op_base_remote" "op_base" "op"
##   ..$ dots: list()
##   ..$ args:List of 1
##   .. ..$ n: num 6
##   ..- attr(*, "class")= chr [1:3] "op_head" "op_single" "op"
##  - attr(*, "class")= chr [1:5] "tbl_SQLiteConnection" "tbl_dbi" "tbl_sql" "tbl_lazy" ...
```

Al trabajar con bases de datos, dplyr intenta ser lo más vago posible:

-  No exporta datos a R a menos que se pida explícitamente (`colect()`).

-  Retrasa cualquier operación lo máximo posible: 
   agrupa todo lo que se desea hacer y luego hace una única petición a la base de datos.
   

```r
invoices %>% head %>% collect
```

```
## # A tibble: 6 x 9
##   InvoiceId CustomerId InvoiceDate Billi~1 Billi~2 Billi~3 Billi~4 Billi~5 Total
##       <int>      <int> <chr>       <chr>   <chr>   <chr>   <chr>   <chr>   <dbl>
## 1         1          2 2009-01-01~ Theodo~ Stuttg~ <NA>    Germany 70174    1.98
## 2         2          4 2009-01-02~ Ullevå~ Oslo    <NA>    Norway  0171     3.96
## 3         3          8 2009-01-03~ Grétry~ Brusse~ <NA>    Belgium 1000     5.94
## 4         4         14 2009-01-06~ 8210 1~ Edmont~ AB      Canada  T6G 2C7  8.91
## 5         5         23 2009-01-11~ 69 Sal~ Boston  MA      USA     2113    13.9 
## 6         6         37 2009-01-19~ Berger~ Frankf~ <NA>    Germany 60316    0.99
## # ... with abbreviated variable names 1: BillingAddress, 2: BillingCity,
## #   3: BillingState, 4: BillingCountry, 5: BillingPostalCode
```

```r
invoices %>% count # número de filas
```

```
## # Source:   lazy query [?? x 1]
## # Database: sqlite 3.36.0 [D:\OneDrive - Universidade da
## #   Coruña\__Actual\__IA\_intror\datos\chinook.db]
##       n
##   <int>
## 1   412
```

Por ejemplo, para obtener el importe mínimo, máximo y la media de las facturas:


```r
res <- invoices %>% summarise(min = min(Total, na.rm = TRUE), 
                        max = max(Total, na.rm = TRUE), med = mean(Total, na.rm = TRUE))
show_query(res)
```

```
## <SQL>
## SELECT MIN(`Total`) AS `min`, MAX(`Total`) AS `max`, AVG(`Total`) AS `med`
## FROM `invoices`
```

```r
res  %>% collect
```

```
## # A tibble: 1 x 3
##     min   max   med
##   <dbl> <dbl> <dbl>
## 1  0.99  25.9  5.65
```

Para obtener el total de las facturas de cada uno de los países:


```r
res <- invoices %>% group_by(BillingCountry) %>% 
          summarise(n = n(), total = sum(Total, na.rm = TRUE))
show_query(res)
```

```
## <SQL>
## SELECT `BillingCountry`, COUNT(*) AS `n`, SUM(`Total`) AS `total`
## FROM `invoices`
## GROUP BY `BillingCountry`
```

```r
res  %>% collect
```

```
## # A tibble: 24 x 3
##    BillingCountry     n total
##    <chr>          <int> <dbl>
##  1 Argentina          7  37.6
##  2 Australia          7  37.6
##  3 Austria            7  42.6
##  4 Belgium            7  37.6
##  5 Brazil            35 190. 
##  6 Canada            56 304. 
##  7 Chile              7  46.6
##  8 Czech Republic    14  90.2
##  9 Denmark            7  37.6
## 10 Finland            7  41.6
## # ... with 14 more rows
```

Para obtener el listado de países junto con su facturación media, ordenado alfabéticamente por país:


```r
res <- invoices %>% group_by(BillingCountry) %>% 
          summarise(n = n(), med = mean(Total, na.rm = TRUE)) %>%
          arrange(BillingCountry)
show_query(res)
```

```
## <SQL>
## SELECT `BillingCountry`, COUNT(*) AS `n`, AVG(`Total`) AS `med`
## FROM `invoices`
## GROUP BY `BillingCountry`
## ORDER BY `BillingCountry`
```

```r
res  %>% collect
```

```
## # A tibble: 24 x 3
##    BillingCountry     n   med
##    <chr>          <int> <dbl>
##  1 Argentina          7  5.37
##  2 Australia          7  5.37
##  3 Austria            7  6.09
##  4 Belgium            7  5.37
##  5 Brazil            35  5.43
##  6 Canada            56  5.43
##  7 Chile              7  6.66
##  8 Czech Republic    14  6.45
##  9 Denmark            7  5.37
## 10 Finland            7  5.95
## # ... with 14 more rows
```

Si el resultado lo queremos en orden decreciente por importe de facturación media:


```r
invoices %>% group_by(BillingCountry) %>% 
          summarise(n = n(), med = mean(Total, na.rm = TRUE)) %>%
          arrange(desc(med)) %>% collect
```

```
## # A tibble: 24 x 3
##    BillingCountry     n   med
##    <chr>          <int> <dbl>
##  1 Chile              7  6.66
##  2 Ireland            7  6.52
##  3 Hungary            7  6.52
##  4 Czech Republic    14  6.45
##  5 Austria            7  6.09
##  6 Finland            7  5.95
##  7 Netherlands        7  5.80
##  8 India             13  5.79
##  9 USA               91  5.75
## 10 Norway             7  5.66
## # ... with 14 more rows
```

Para obtener un listado con Nombre y Apellidos de cliente y el importe de cada una de sus facturas (Hint: WHERE customer.CustomerID=invoices.CustomerID):


```r
customers <- tbl(chinook, "customers")
tbl_vars(customers) 
```

```
## <dplyr:::vars>
##  [1] "CustomerId"   "FirstName"    "LastName"     "Company"      "Address"     
##  [6] "City"         "State"        "Country"      "PostalCode"   "Phone"       
## [11] "Fax"          "Email"        "SupportRepId"
```

```r
res <- customers %>% inner_join(invoices, by = "CustomerId") %>% select(FirstName, LastName, Country, Total) 
show_query(res)
```

```
## <SQL>
## SELECT `FirstName`, `LastName`, `Country`, `Total`
## FROM (SELECT `LHS`.`CustomerId` AS `CustomerId`, `FirstName`, `LastName`, `Company`, `Address`, `City`, `State`, `Country`, `PostalCode`, `Phone`, `Fax`, `Email`, `SupportRepId`, `InvoiceId`, `InvoiceDate`, `BillingAddress`, `BillingCity`, `BillingState`, `BillingCountry`, `BillingPostalCode`, `Total`
## FROM `customers` AS `LHS`
## INNER JOIN `invoices` AS `RHS`
## ON (`LHS`.`CustomerId` = `RHS`.`CustomerId`)
## )
```

```r
res  %>% collect
```

```
## # A tibble: 412 x 4
##    FirstName LastName  Country Total
##    <chr>     <chr>     <chr>   <dbl>
##  1 Luís      Gonçalves Brazil   3.98
##  2 Luís      Gonçalves Brazil   3.96
##  3 Luís      Gonçalves Brazil   5.94
##  4 Luís      Gonçalves Brazil   0.99
##  5 Luís      Gonçalves Brazil   1.98
##  6 Luís      Gonçalves Brazil  13.9 
##  7 Luís      Gonçalves Brazil   8.91
##  8 Leonie    Köhler    Germany  1.98
##  9 Leonie    Köhler    Germany 13.9 
## 10 Leonie    Köhler    Germany  8.91
## # ... with 402 more rows
```

Para obtener el porcentaje de canciones que son vídeos:


```r
tracks <- tbl(chinook, "tracks")
head(tracks) 
```

```
## # Source:   lazy query [?? x 9]
## # Database: sqlite 3.36.0 [D:\OneDrive - Universidade da
## #   Coruña\__Actual\__IA\_intror\datos\chinook.db]
##   TrackId Name            AlbumId Media~1 GenreId Compo~2 Milli~3  Bytes UnitP~4
##     <int> <chr>             <int>   <int>   <int> <chr>     <int>  <int>   <dbl>
## 1       1 For Those Abou~       1       1       1 Angus ~  343719 1.12e7    0.99
## 2       2 Balls to the W~       2       2       1 <NA>     342562 5.51e6    0.99
## 3       3 Fast As a Shark       3       2       1 F. Bal~  230619 3.99e6    0.99
## 4       4 Restless and W~       3       2       1 F. Bal~  252051 4.33e6    0.99
## 5       5 Princess of th~       3       2       1 Deaffy~  375418 6.29e6    0.99
## 6       6 Put The Finger~       1       1       1 Angus ~  205662 6.71e6    0.99
## # ... with abbreviated variable names 1: MediaTypeId, 2: Composer,
## #   3: Milliseconds, 4: UnitPrice
```

```r
tracks %>% group_by(MediaTypeId) %>% 
    summarise(n = n()) %>% collect %>% mutate(freq = n / sum(n))
```

```
## # A tibble: 5 x 3
##   MediaTypeId     n    freq
##         <int> <int>   <dbl>
## 1           1  3034 0.866  
## 2           2   237 0.0677 
## 3           3   214 0.0611 
## 4           4     7 0.00200
## 5           5    11 0.00314
```

```r
media_types <- tbl(chinook, "media_types")
head(media_types)
```

```
## # Source:   lazy query [?? x 2]
## # Database: sqlite 3.36.0 [D:\OneDrive - Universidade da
## #   Coruña\__Actual\__IA\_intror\datos\chinook.db]
##   MediaTypeId Name                       
##         <int> <chr>                      
## 1           1 MPEG audio file            
## 2           2 Protected AAC audio file   
## 3           3 Protected MPEG-4 video file
## 4           4 Purchased AAC audio file   
## 5           5 AAC audio file
```

```r
tracks %>% inner_join(media_types, by = "MediaTypeId") %>% count(Name.y) %>% 
    collect %>% mutate(freq = n / sum(n)) %>% filter(grepl('video', Name.y))
```

```
## # A tibble: 1 x 3
##   Name.y                          n   freq
##   <chr>                       <int>  <dbl>
## 1 Protected MPEG-4 video file   214 0.0611
```

Para listar los 10 mejores clientes (aquellos a los que se les ha facturado más cantidad) indicando Nombre, Apellidos, Pais y el importe total de su facturación:


```r
customers %>% inner_join(invoices, by = "CustomerId") %>% group_by(CustomerId) %>% 
    summarise(FirstName, LastName, country, total = sum(Total, na.rm = TRUE)) %>%  
    arrange(desc(total)) %>% head(10) %>% collect
```

```
## # A tibble: 10 x 5
##    CustomerId FirstName LastName   Country        total
##         <int> <chr>     <chr>      <chr>          <dbl>
##  1          6 Helena    Holý       Czech Republic  49.6
##  2         26 Richard   Cunningham USA             47.6
##  3         57 Luis      Rojas      Chile           46.6
##  4         45 Ladislav  Kovács     Hungary         45.6
##  5         46 Hugh      O'Reilly   Ireland         45.6
##  6         28 Julia     Barnett    USA             43.6
##  7         24 Frank     Ralston    USA             43.6
##  8         37 Fynn      Zimmermann Germany         43.6
##  9          7 Astrid    Gruber     Austria         42.6
## 10         25 Victor    Stevens    USA             42.6
```

Para listar los géneros musicales por orden decreciente de popularidad (definida la popularidad como el número de canciones de ese género), indicando el porcentaje de las canciones de ese género:


```r
tracks %>% inner_join(tbl(chinook, "genres"), by = "GenreId") %>% count(Name.y) %>% 
    arrange(desc(n)) %>% collect %>% mutate(freq = n / sum(n))
```

```
## # A tibble: 25 x 3
##    Name.y                 n   freq
##    <chr>              <int>  <dbl>
##  1 Rock                1297 0.370 
##  2 Latin                579 0.165 
##  3 Metal                374 0.107 
##  4 Alternative & Punk   332 0.0948
##  5 Jazz                 130 0.0371
##  6 TV Shows              93 0.0265
##  7 Blues                 81 0.0231
##  8 Classical             74 0.0211
##  9 Drama                 64 0.0183
## 10 R&B/Soul              61 0.0174
## # ... with 15 more rows
```

Para listar los 10 artistas con mayor número de canciones de forma descendente según el número de canciones:


```r
tracks %>% inner_join(tbl(chinook, "albums"), by = "AlbumId") %>% 
    inner_join(tbl(chinook, "artists"), by = "ArtistId") %>% 
    count(Name.y) %>% arrange(desc(n)) %>% collect
```

```
## # A tibble: 204 x 2
##    Name.y              n
##    <chr>           <int>
##  1 Iron Maiden       213
##  2 U2                135
##  3 Led Zeppelin      114
##  4 Metallica         112
##  5 Lost               92
##  6 Deep Purple        92
##  7 Pearl Jam          67
##  8 Lenny Kravitz      57
##  9 Various Artists    56
## 10 The Office         53
## # ... with 194 more rows
```

Al finalizar hay que desconectar la base de datos:


```r
DBI::dbDisconnect(chinook)            
```




