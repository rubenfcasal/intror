Estructuras de datos
====================





En los ejemplos que hemos visto hasta ahora los objetos
de `R` almacenaban un único valor cada uno. Sin embargo, las estructuras
de datos que proporciona `R` permiten
almacenar en un mismo objeto varios valores. Las principales estructuras
son:

-   Vectores

-   Matrices y Arrays

-   Data Frames

-   Listas


Vectores
--------
Un vector es un conjunto de valores básicos del mismo tipo. 
La forma más sencilla de crear vectores es a
través de la función `c()` que se usa para combinar (concatenar) valores.

```r
x <- c(3, 5, 7)
x
```

```
## [1] 3 5 7
```


```r
y <- c(8, 9)
y
```

```
## [1] 8 9
```


```r
c(x, y)
```

```
## [1] 3 5 7 8 9
```


```r
z <- c("Hola", "Adios")
z
```

```
## [1] "Hola"  "Adios"
```


### Generación de secuencias 
Existen varias funciones que pemiten obtener secuencias de números

```r
x <- 1:5
x
```

```
## [1] 1 2 3 4 5
```


```r
seq(1, 5, 0.5)
```

```
## [1] 1.0 1.5 2.0 2.5 3.0 3.5 4.0 4.5 5.0
```


```r
seq(from=1, to=5, length=9)
```

```
## [1] 1.0 1.5 2.0 2.5 3.0 3.5 4.0 4.5 5.0
```


```r
rep(1, 5)
```

```
## [1] 1 1 1 1 1
```


### Generación secuencias aleatorias 

A continuación se obtiene una simulación de 10 lanzamientos de un dado

```r
sample(1:6, size=10, replace = T) #lanzamiento de un dado
```

```
##  [1] 1 3 6 2 4 5 2 1 2 6
```

Para simular el lanzamiento de una moneda podemos escribir 

```r
resultado <- c(cara=1,cruz=0) # se le han asignado nombres al objeto
print(resultado)
```

```
## cara cruz 
##    1    0
```

```r
class(resultado)
```

```
## [1] "numeric"
```

```r
attributes(resultado)
```

```
## $names
## [1] "cara" "cruz"
```

```r
names(resultado)
```

```
## [1] "cara" "cruz"
```


```r
lanz <- sample(resultado, size=10, replace = T)
lanz
```

```
## cara cruz cara cara cruz cara cruz cara cruz cruz 
##    1    0    1    1    0    1    0    1    0    0
```

```r
table(lanz)
```

```
## lanz
## 0 1 
## 5 5
```

Otros ejemplos

```r
rnorm(10)  # rnorm(10, mean = 0, sd = 1)
```

```
##  [1] -0.33189114 -0.46768827  0.02812388 -0.49466522  0.18366710
##  [6] -0.75595018 -0.30417405  0.63244979 -1.09631785 -0.02104755
```

```r
runif(15, min = 2, max = 10)
```

```
##  [1] 7.246310 8.178022 9.174676 8.569134 8.143638 9.697063 8.521244
##  [8] 9.273333 3.274513 3.785032 2.246995 5.865423 3.052325 8.220563
## [15] 4.974823
```

El lector puede utilizar la función `help()` para obtener la ayuda de las funciones
anteriores.


### Selección de elementos de un vector 
Para acceder a los elementos de un vector se indica entre corchetes el
correspondiente vector de subíndices (enteros positivos).

```r
x <- seq(-3, 3, 1)
x
```

```
## [1] -3 -2 -1  0  1  2  3
```

```r
x[1] # primer elemento
```

```
## [1] -3
```

```r
ii <- c(1, 5, 7)
x[ii] #posiciones 1, 5 y 7
```

```
## [1] -3  1  3
```

```r
ii <- x>0; ii
```

```
## [1] FALSE FALSE FALSE FALSE  TRUE  TRUE  TRUE
```

```r
x[ii]  # valores positivos
```

```
## [1] 1 2 3
```

```r
ii <- 1:3
x[-ii]  # elementos de x salvo los 3 primeros
```

```
## [1] 0 1 2 3
```


### Ordenación de vectores

```r
x <- c(65, 18, 59, 18, 6, 94, 26)
sort(x)
```

```
## [1]  6 18 18 26 59 65 94
```

```r
sort(x, decreasing = T)
```

```
## [1] 94 65 59 26 18 18  6
```

Otra posibilidad es utilizar un índice de ordenación.

```r
ii <- order(x)
ii  # índice de ordenación
```

```
## [1] 5 2 4 7 3 1 6
```

```r
x[ii]  # valores ordenados
```

```
## [1]  6 18 18 26 59 65 94
```

La función `rev()` devuelve los valores del vector en orden inverso.

```r
rev(x)
```

```
## [1] 26 94  6 18 59 18 65
```

### Valores perdidos 
Los valore perdidos aparecen normalmente cuando algún dato no ha sido registrado. Este tipo de
valores se registran como `NA` (abreviatura de *Not Available*).

Por ejemplo, supongamos que tenemos registrado las alturas de 5 personas
pero desconocemos la altura de la cuarta persona. El vector sería
registrado como sigue:

```r
altura <- c(165, 178, 184, NA, 175)
altura
```

```
## [1] 165 178 184  NA 175
```

Es importante notar que cualquier operación aritmética sobre un vector
que contiene algún `NA` dará como resultado otro `NA`.

```r
mean(altura)
```

```
## [1] NA
```

Para forzar a `R` a que ignore los valores perdidos se utliza la opción `na.rm = TRUE`.

```r
mean(altura, na.rm = TRUE)
```

```
## [1] 175.5
```


`R` permite gestionar otros tipos de valores especiales:

-   `NaN` (*Not a Number*): es resultado de una indeterminación.

-   `Inf`: `R` representa valores no finitos $\pm \infty$ como `Inf` y `-Inf`.

<br> \vspace*{0.3cm}


```r
5/0  # Infinito
```

```
## [1] Inf
```

```r
log(0)  # -Infinito
```

```
## [1] -Inf
```

```r
0/0  # Not a Number
```

```
## [1] NaN
```


### Vectores no numéricos 
Los vectores pueden ser no numéricos, aunque todas las componentes deben ser del mismo tipo:

```r
a <- c("A Coruña", "Lugo", "Ourense", "Pontevedra")
a
```

```
## [1] "A Coruña"   "Lugo"       "Ourense"    "Pontevedra"
```

```r
letters[1:10]  # primeras 10 letas del abecedario
```

```
##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j"
```

```r
LETTERS[1:10]  # lo mismo en mayúscula
```

```
##  [1] "A" "B" "C" "D" "E" "F" "G" "H" "I" "J"
```

```r
month.name[1:6]  # primeros 6 meses del año en inglés
```

```
## [1] "January"  "February" "March"    "April"    "May"      "June"
```


### Factores 
Los factores se utilizan para representar datos categóricos. Se 
puede pensar en ellos como vectores de enteros en los que cada entero 
tiene asociada una etiqueta (*label*). Los factores son muy importantes 
en la modelización estadística ya que `R`los trata de forma especial.

Utilizar factores con etiquetas es preferible a utilizar enteros porque 
las etiquetas son auto-descriptivas.

Veamos un ejemplo. Supongamos que el vector `sexo` indica el sexo de un
persona codificado como 0 si hombre y 1 si mujer

```r
sexo <- c(0, 1, 1, 1, 0, 0, 1, 0, 1)
sexo
```

```
## [1] 0 1 1 1 0 0 1 0 1
```

```r
table(sexo)
```

```
## sexo
## 0 1 
## 4 5
```

El problema de introducir así los datos es que no queda reflejado la
etiquetación de los mismos. Para ello guardaremos los datos en una
estructura tipo factor:

```r
sexo2 <- factor(sexo, labels = c("hombre", "mujer")); sexo2
```

```
## [1] hombre mujer  mujer  mujer  hombre hombre mujer  hombre mujer 
## Levels: hombre mujer
```

```r
levels(sexo2)  # devuelve los niveles de un factor
```

```
## [1] "hombre" "mujer"
```

```r
unclass(sexo2)  # representación subyacente del factor
```

```
## [1] 1 2 2 2 1 1 2 1 2
## attr(,"levels")
## [1] "hombre" "mujer"
```

```r
table(sexo2)
```

```
## sexo2
## hombre  mujer 
##      4      5
```

Veamos otro ejemplo, en el que inicialmente tenemos datos categóricos. Los 
niveles se toman automáticamente por orden alfabético

```r
respuestas <- factor(c('si', 'si', 'no', 'si', 'si', 'no', 'no'))
respuestas
```

```
## [1] si si no si si no no
## Levels: no si
```

Si deseásemos otro orden (lo cual puede ser importante en algunos casos, por ejemplo 
para representaciones gráficas), habría que indicarlo expresamente

```r
respuestas <- factor(c('si', 'si', 'no', 'si', 'si', 'no', 'no'), levels = c('si', 'no'))
respuestas
```

```
## [1] si si no si si no no
## Levels: si no
```


Matrices y arrays
-----------------

### Matrices 
Las *matrices* son la extensión natural de los vectores a dos dimensiones. 
Su generalización a más dimensiones se llama *array*. 

Las matrices se pueden crear concatenando vectores
con las funciones `cbind` o `rbind`:

```r
x <- c(3, 7, 1, 8, 4)
y <- c(7, 5, 2, 1, 0)
cbind(x, y)  # por columnas
```

```
##      x y
## [1,] 3 7
## [2,] 7 5
## [3,] 1 2
## [4,] 8 1
## [5,] 4 0
```

```r
rbind(x, y)  # por filas
```

```
##   [,1] [,2] [,3] [,4] [,5]
## x    3    7    1    8    4
## y    7    5    2    1    0
```

 Una matriz se puede crear con la función `matrix` donde el parámetro
`nrow` indica el número de filas y `ncol` el número de columnas. 
Por defecto, los valores se colocan por columnas.

```r
matrix(1:8, nrow = 2, ncol = 4)  # equivalente a matrix(1:8, nrow=2)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    3    5    7
## [2,]    2    4    6    8
```

Los nombres de los parámetros se pueden acortar siempre y cuando no haya 
ambigüedad, por lo que es habitual escribir

```r
matrix(1:8, nr = 2, nc = 4)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    3    5    7
## [2,]    2    4    6    8
```

Si queremos indicar que los valores se escriban por filas

```r
matrix(1:8, nr = 2, byrow = TRUE)
```

```
##      [,1] [,2] [,3] [,4]
## [1,]    1    2    3    4
## [2,]    5    6    7    8
```


### Nombres en matrices
Se pueden dar nombres a las filas y columnas de una matriz.

```r
x <- matrix(c(1, 2, 3, 11, 12, 13), nrow = 2, byrow = TRUE)
x
```

```
##      [,1] [,2] [,3]
## [1,]    1    2    3
## [2,]   11   12   13
```

```r
rownames(x) <- c("fila 1", "fila 2")
colnames(x) <- c("col 1", "col 2", "col 3")
x 
```

```
##        col 1 col 2 col 3
## fila 1     1     2     3
## fila 2    11    12    13
```

Obtenemos el mismo resultado si escribimos

```r
colnames(x) <- paste("col", 1:ncol(x), sep=" ")
```
    
Internamente, las matrices son vectores con un atributo especial: la *dimensión*.

```r
dim(x)
```

```
## [1] 2 3
```

```r
attributes(x)
```

```
## $dim
## [1] 2 3
## 
## $dimnames
## $dimnames[[1]]
## [1] "fila 1" "fila 2"
## 
## $dimnames[[2]]
## [1] "col 1" "col 2" "col 3"
```


### Acceso a los elementos de una matriz 
El acceso a los elementos de una matriz se realiza de forma análoga al acceso ya
comentado para los vectores.

```r
x <- matrix(1:6, 2, 3); x
```

```
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
```

```r
x[1, 1]
```

```
## [1] 1
```

```r
x[2, 2]
```

```
## [1] 4
```

```r
x[2, ]  # segunda fila
```

```
## [1] 2 4 6
```

```r
x[ ,2]  # segunda columna
```

```
## [1] 3 4
```

```r
x[1, 1:2]  # primera fila, columnas 1ª y 2ª 
```

```
## [1] 1 3
```


### Ordenación por filas y columnas 
En ocasiones, interesará ordenar los elementos de una matriz por los valores de una
determinada columna o fila.

Por ejemplo, supongamos la matriz

```r
x <- c(79, 100, 116, 121, 52, 134, 123, 109, 80, 107, 66, 118)
x <- matrix(x, ncol=4, byrow=T); x
```

```
##      [,1] [,2] [,3] [,4]
## [1,]   79  100  116  121
## [2,]   52  134  123  109
## [3,]   80  107   66  118
```

La matriz ordenada por los valores de la primera columna viene dada por

```r
ii <- order(x[ ,1])
x[ii, ]  # ordenación columna 1
```

```
##      [,1] [,2] [,3] [,4]
## [1,]   52  134  123  109
## [2,]   79  100  116  121
## [3,]   80  107   66  118
```

De igual modo, si queremos ordenar por los valores de la cuarta columna:

```r
ii <- order(x[ ,4]); x[ii, ]  # ordenación columna 4
```

```
##      [,1] [,2] [,3] [,4]
## [1,]   52  134  123  109
## [2,]   80  107   66  118
## [3,]   79  100  116  121
```


### Operaciones con Matrices y Arrays 
A continuación se muestran algunas funciones que se pueden emplear con
matrices

Función  |  Descripción
-------  |  -----------
`dim(),nrow(),ncol()`  |  número de filas y/o columnas
`diag()`  |  diagonal de una matrix
`*`    |  multiplicación elemento a elemento
`%*%`  |  multiplicación matricial de matrices
`cbind(),rbind() `| encadenamiento de columnas o filas
`t()`  |  transpuesta
`solve(A)`  |  inversa de la matriz A
`solve(A,b)`  |  solución del sistema de ecuaciones $Ax=b$
`qr()`  |  descomposición de Cholesky
`eigen()`  |  autovalores y autovectores
`svd()`  |  descomposición singular


### Ejemplos

```r
x <- matrix(1:6, ncol = 3)
x
```

```
##      [,1] [,2] [,3]
## [1,]    1    3    5
## [2,]    2    4    6
```

```r
t(x)  # matriz transpuesta
```

```
##      [,1] [,2]
## [1,]    1    2
## [2,]    3    4
## [3,]    5    6
```

```r
dim(x)  # dimensiones de la matriz
```

```
## [1] 2 3
```


### Inversión de una matriz

```r
A <- matrix(c(2, 4, 0, 2), nrow = 2); A
```

```
##      [,1] [,2]
## [1,]    2    0
## [2,]    4    2
```

```r
B <- solve(A)
B  # inversa
```

```
##      [,1] [,2]
## [1,]  0.5  0.0
## [2,] -1.0  0.5
```

```r
A %*% B  # comprobamos que está bien
```

```
##      [,1] [,2]
## [1,]    1    0
## [2,]    0    1
```


Data frames
-----------

Los `data.fames` (*marcos de datos*) son el objeto más habitual para el
almacenamiento de datos. En este tipo de objetos cada individuo de la muestra
se corresponde con una fila y cada una de las variables con una columna.
Para la creación de estas estructuras se utiliza la función
`data.frame()`.

Este tipo de estructuras son en apariencia muy similares a las matrices, con la
ventaja de que permiten que los valores de las distintas columnas sean de tipos
diferentes. Por ejemplo, supongamos que tenemos registrados los siguientes valores

```r
Producto <- c("Zumo", "Queso", "Yogourt")
Seccion <- c("Bebidas", "Lácteos", "Lácteos")
Unidades <- c(2, 1, 10)
```

Los valores anteriores se podrían guardar en una única matriz

```r
x <- cbind(Producto, Seccion, Unidades)
class(x)
```

```
## [1] "matrix"
```

```r
x
```

```
##      Producto  Seccion   Unidades
## [1,] "Zumo"    "Bebidas" "2"     
## [2,] "Queso"   "Lácteos" "1"     
## [3,] "Yogourt" "Lácteos" "10"
```

Sin embargo, el resultado anterior no es satisfactorio ya que todos
los valores se han transformado en caracteres. Una solución mejor es
utilizar un `data.frame`, con lo cual se mantiene el tipo original de las variables.

```r
lista.compra <- data.frame(Producto, Seccion, Unidades)
class(lista.compra)
```

```
## [1] "data.frame"
```

```r
lista.compra
```

```
##   Producto Seccion Unidades
## 1     Zumo Bebidas        2
## 2    Queso Lácteos        1
## 3  Yogourt Lácteos       10
```

A continuación se muestran ejemplos que ilustran la manera de acceder
a los valores de un data.frame.

```r
lista.compra$Unidades
```

```
## [1]  2  1 10
```

```r
lista.compra[ ,3]  # de manera equivalente
```

```
## [1]  2  1 10
```

```r
lista.compra$Seccion
```

```
## [1] Bebidas Lácteos Lácteos
## Levels: Bebidas Lácteos
```

```r
lista.compra$Unidades[1:2]  # primeros dos valores de Unidades
```

```
## [1] 2 1
```

```r
lista.compra[2,]  # segunda fila
```

```
##   Producto Seccion Unidades
## 2    Queso Lácteos        1
```

La función `summary()` permite hacer un resumen estadístico de las
variables (columnas) del data.frame.

```r
summary(lista.compra)
```

```
##     Producto    Seccion     Unidades     
##  Queso  :1   Bebidas:1   Min.   : 1.000  
##  Yogourt:1   Lácteos:2   1st Qu.: 1.500  
##  Zumo   :1               Median : 2.000  
##                          Mean   : 4.333  
##                          3rd Qu.: 6.000  
##                          Max.   :10.000
```


Listas
------

Las listas son colecciones ordenadas de cualquier tipo de objetos (en `R` las
listas son un tipo especial de vectores). Así, mientras que los elementos de 
los vectores, matrices y arrays deben ser del mismo tipo, en el caso de las 
listas se pueden tener elementos de tipos distintos.

```r
x <- c(1, 2, 3, 4)
y <- c("Hombre", "Mujer")
z <- matrix(1:12, ncol = 4)
datos <- list(A=x, B=y, C=z)
datos
```

```
## $A
## [1] 1 2 3 4
## 
## $B
## [1] "Hombre" "Mujer" 
## 
## $C
##      [,1] [,2] [,3] [,4]
## [1,]    1    4    7   10
## [2,]    2    5    8   11
## [3,]    3    6    9   12
```
