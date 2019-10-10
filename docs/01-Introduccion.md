Introducción
============





El entorno estadístico `R` puede ser una herramienta de gran
utilidad a lo largo de todo el proceso de obtención de información
a partir de datos (normalmente con el objetivo final de ayudar a tomar decisiones). 

\begin{figure}[!htb]

{\centering \includegraphics[width=0.7\linewidth]{figuras/esquema2} 

}

\caption{Etapas del proceso}(\#fig:esquema)
\end{figure}

## El lenguaje y entorno estadístico `R`

`R` es un lenguaje de programación desarrollado específicamente para el
análisis estadístico y la visualización de datos.

- El lenguaje `R` es interpretado (similar a Matlab o Phyton) pero orientado al
  análisis estadístico (fórmulas modelos, factores,...).

    - derivado del S (Laboratorios Bell).

- `R` es un **Software Libre** bajo las condiciones de licencia GPL de
    GNU, con código fuente de libre acceso.

    - Además de permitir crear **nuevas funciones**, 
      se pueden examinar y modificar las ya existentes.

- Multiplataforma, 
  disponible para los sistemas operativos más populares (Linux, Windows, MacOS X, ...).


### Principales características

Se pueden destacar las siguientes características del entorno `R`:

- Dispone de numerosos complementos (librerías, paquetes) que cubren “literalmente”
  todos los campos del análisis de datos.

- Repositorios: 
    
    - [CRAN](https://cran.r-project.org) (9705, 14972, ...)
    
    - [Bioconductor](https://www.bioconductor.org) (1289, 1741, ...), 
    
    - [GitHub](https://github.com/trending/r?since=monthly), ...    

- Existe una comunidad de usuarios (programadores) muy dinámica
  (multitud de paquetes adicionales).

- Muy bien documentado y con numerosos foros de ayuda.

- Puntos débiles (a priori): velocidad, memoria, ...

Aunque inicialmente fue un lenguaje desarrollado por estadísticos
para estadísticos:

\begin{figure}[!htb]

{\centering \includegraphics[width=0.4\linewidth]{figuras/rexer2016} 

}

\caption{Rexer Data Miner Survey 2007-2015}(\#fig:rexer)
\end{figure}

Hoy en día es muy popular:

\begin{figure}[!htb]

{\centering \includegraphics[width=0.35\linewidth]{figuras/IEEE-top-programming-languages-of-2019} 

}

\caption{[IEEE Spectrum](https://spectrum.ieee.org) Top Programming Languages, 2019}(\#fig:ieee)
\end{figure}

`R` destaca especialmente en:

- Representaciones gráficas.

- Métodos estadísticos “avanzados”:

    - *Data Science*: *Statistical Learning*, *Data Mining*, 
      *Machine Learning*, *Business Intelligence*, ...

    - Datos funcionales.

    - Estadística espacial.

    - ...

- Análisis de datos “complejos”:

    - Big Data.

    - Lenguaje natural (*Text Mining*).

    - Análisis de redes.

    -   ...

### Interfaces gráficas 

El programa `R`
utiliza una **interfaz de comandos** donde se teclean las instrucciones
que se pretenden ejecutar (ver Figura \@ref(fig:consola)).

Por ejemplo, para obtener una secuencia de números desde el 1 hasta el
10, se utilizará la sentencia:


```r
1:10
```
obteniéndose el resultado

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

En el Apéndice \@ref(instalacion) se detallan los pasos para la instalación de `R`,
y en el Apéndice \@ref(interfaces) los de otras interfaces gráficas.


Entorno de trabajo
------------------

### Ventana de Consola 

Al abrir el programa `R`, tal y como ya
se ha visto, aparece la siguiente ventana de consola para trabajar de
modo interactivo en modo comando (ver Figura \@ref(fig:consola)).


\begin{figure}[!htb]

{\centering \includegraphics[width=0.7\linewidth]{figuras/consola} 

}

\caption{Consola de `R` en Windows (modo MDI).}(\#fig:consola)
\end{figure}


En la ventana de consola cada línea en que el usuario puede
introducir información se inicia con el carácter `>` que pone el sistema
`R`.

-   Para ejecutar las instrucciones que están en una línea, se pulsa la
    tecla de Retorno (y por defecto se imprime el resultado).
    
    
    ```r
    2+2
    ```
    
    ```
    ## [1] 4
    ```
    
    ```r
    1+2*4
    ```
    
    ```
    ## [1] 9
    ```
    
-   Se pueden escribir varias instrucción en una misma línea
    separándolas por ";"

    
    ```r
    2+2;1+2*4
    ```
    
    ```
    ## [1] 4
    ```
    
    ```
    ## [1] 9
    ```

-   Se pueden recuperar líneas de instrucciones introducidas
    anteriormente pulsando la tecla con la flecha ascendente del
    teclado, a fin de re-ejecutarlas o modificarlas.

### Ventana de Script 

La ventana consola ejecuta de
forma automática cada línea de comando. Sin embargo, suele interesar
guardar un conjunto de instrucciones en un único archivo de texto para
formar lo que se conoce como un *script*. Las instrucciones del script
se copian y pegan en la ventana de comandos para ser ejecutadas.

Para crear un fichero de script se selecciona el submenú *Nuevo script*
dentro del menú *Archivo*. Otra posibilidad es utilizar directamente la
combinación de teclas Ctrl+N.


\begin{figure}[!htb]

{\centering \includegraphics[width=0.7\linewidth]{figuras/script} 

}

\caption{Ventanas de la consola y de *script* en Windows (modo MDI).}(\#fig:script)
\end{figure}


Para guardar este tipo de fichero se utiliza directamente el menú *Archivo > Guardar como...* 
y se elige a continuación la ubicación en el disco duro del ordenador.

De igual modo para abrir un script existente se hace a través del menú *Archivo > Abrir script...*.

En el Apéndice \@ref(links) se incluyen enlaces a numerosos recursos para el aprendizaje de R,
incluyendo manuales y libros, además de otros recursos para la obtención de ayuda.


### Ayuda dentro del programa
 
Como ya se ha comentado con anterioridad,
hay manuales de `R` alos que se puede acceder a través del menú
*Ayuda > Manuales (en PDF)*
    
Puede empezarse utilizando `help.start()` o `demo()`.

Todas las funciones de `R` tienen su documentación integrada en
el programa. Para obtener la ayuda de una determinada función se
utilizará `help (función)` o de forma equivalente `?función`.

Por ejemplo, la ayuda de la función `rnorm` (utilizada para la
generación de datos normales) se obtiene con el código


```r
help(rnorm)
?rnorm
```

En muchas ocasiones no se conoce el nombre exacto de la función de la
que queremos obtener la documentación. En estos casos, la función
`help.search()` realiza búsquedas en la documentación en todos los
paquetes instalados, estén cargados o no.

Por ejemplo, si no conocemos la función que permite calcular la mediana
de un conjunto de datos, se puede utilizar


```r
help.search("median")
```

Para más detalles véase `?help.search`


Librerías
---------

### Paquetes 

Al iniciar el programa `R` se cargan
por defecto una serie de librerías básicas con las que se pueden
realizar una gran cantidad de operaciones básicas. Estas librerías
conforman el llamado **paquete base**.

En otras ocasiones es necesario cargar otras librerías distintas a las
anteriores. Esto se hace a través de los llamados paquetes (packages)
que pueden ser descargados directamente de la web

<http://cran.r-project.org/web/packages/>

o directamente a través del menú `Paquetes`.


### Instalación {#instalacion-pkg}

La instalación de un paquete, bajo el sistema
operativo Windows, se puede hacer de varias formas:

-   Desde el menú *Paquetes > Instalar paquete(s)...*

-   Desde la ventana de consola utilizando la instrucción

    
    ```r
    install.packages("nombre del paquete")
    ```

Este proceso sólo es necesario realizarlo la primera vez que se utilice
el paquete.

### Carga

Para utilizar un paquete ya instalado será necesario cargarlo, lo cual se puede hacer de
varias formas:

-   Desde el menú `Paquetes > Cargar paquete(s)...`

-   Por consola, utilizando `library(nombre del paquete)`

Esta operación será necesario realizarla cada vez que se inicie una
sesión de R.

Finalmente, la ayuda de un paquete se puede obtener con la sentencia

```r
library(help = "nombre del paquete") 
```


Una primera sesión
------------------

### Inicio de una sesión 
 El programa `R` (bajo
Windows) se arranca al hacer un doble-click sobre el icono del programa.
Entonces aparecerá la ventana de consola donde se escribirán los
comandos y los resultados serán mostrados inmediatamente por pantalla.

Veamos algún ejemplo:

```r
3+5
```

```
## [1] 8
```

```r
sqrt(16) # raiz cuadrada de 16
```

```
## [1] 4
```

```r
pi # R reconoce el número pi
```

```
## [1] 3.141593
```

Nótese que en los comandos se pueden hacer comentarios utilizando el
símbolo `#`.

Los resultados obtenidos pueden guardarse en objetos. Por
ejemplo, al escribir

```r
a <- 3 + 5
```
el resultado de la suma se guarda en `a`. Se puede comprobar si la
asignación se ha realizado correctamente escribiendo el nombre del
objeto

```r
a
```

```
## [1] 8
```
La asignación anterior se puede hacer del siguiente modo
ejemplo, al escribir

```r
a <- 3 + 5; a
```

```
## [1] 8
```

**Nota**: Habitualmente no habrá diferencia entre la utilización de las
asignaciones hechas con `=` y `<-` (aunque nosotros emplearemos el segundo). 
Las diferencias aparecen a nivel
de programación y se tratarán en el Capítulo \@ref(programacion).

 Veamos ahora un ejemplo un poco más avanzado. Con el
siguiente código

-   Se obtienen 200 datos simulados siguiendo una distribución gaussiana
    de media 105 y desviación típica 2

-   Se hace un resumen estadístico de los valores obtenidos

-   Se hace el correspondiente histograma y gráfico de cajas

    
    ```r
    x <- rnorm(n = 200, mean = 105, sd = 2) #datos normales
    summary(x) # resumen estadístico
    ```
    
    ```
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   99.69  103.57  104.86  104.81  106.07  109.66
    ```
    
    ```r
    hist(x) # histograma
    ```
    
    
    
    \begin{center}\includegraphics[width=0.7\linewidth]{01-Introduccion_files/figure-latex/unnamed-chunk-14-1} \end{center}
    
    ```r
    boxplot(x) # gráfico de cajas
    ```
    
    
    
    \begin{center}\includegraphics[width=0.7\linewidth]{01-Introduccion_files/figure-latex/unnamed-chunk-14-2} \end{center}


Es importante señalar que `R` diferencia entre mayúsculas y
minúsculas, de modo que los objetos `a` y `A` serán diferentes.

```r
a <- 1:10 # secuencia de números
A <- "casa"
a
```

```
##  [1]  1  2  3  4  5  6  7  8  9 10
```

```r
A
```

```
## [1] "casa"
```

Objetos básicos
---------------
 
`R` es un lenguaje **orientado a
objetos** lo que significa que las variables, datos, funciones,
resultados, etc., se guardan en la memoria del ordenador en forma de
*objetos* con un nombre específico.

Los principales tipos de valores básicos de `R` son:

-   numéricos,

-   cadenas de caracteres, y

-   lógicos

### Objetos numéricos 

Los valores numéricos adoptan
la notación habitual en informática: punto decimal, notacion científica, ...

```r
pi
```

```
## [1] 3.141593
```

```r
1e3
```

```
## [1] 1000
```

Con este tipo de objetos se pueden hacer operaciones aritméticas
utilizando el operador correspondiente.

```r
a <- 3.4
b <- 4.5
a * b
```

```
## [1] 15.3
```

```r
a / b
```

```
## [1] 0.7555556
```

```r
a + b
```

```
## [1] 7.9
```

```r
min(a, b)
```

```
## [1] 3.4
```


### Objetos tipo carácter 

Las cadenas de caracteres
se introducen delimitadas por comillas ("nombre") o por apóstrofos
('nombre').

```r
a <- "casa grande"
a
```

```
## [1] "casa grande"
```

```r
a <- 'casa grande'
a
```

```
## [1] "casa grande"
```

```r
a <- 'casa "grande"'
a
```

```
## [1] "casa \"grande\""
```


### Objetos lógicos

Los objetos lógicos sólo pueden
tomar dos valores `TRUE` (numéricamente toma el valor 1) y `FALSE`
(valor 0).

```r
A <- TRUE
B <- FALSE
A
```

```
## [1] TRUE
```

```r
B
```

```
## [1] FALSE
```

```r
# valores numéricos
as.numeric(A)
```

```
## [1] 1
```

```r
as.numeric(B)
```

```
## [1] 0
```


### Operadores lógicos

Existen varios operadores en
`R` que devuelven un valor de tipo lógico. Veamos algún ejemplo

```r
a <- 2
b <- 3
a == b  # compara a y b
```

```
## [1] FALSE
```

```r
a == a  # compara a y a
```

```
## [1] TRUE
```

```r
a < b
```

```
## [1] TRUE
```

```r
b < a
```

```
## [1] FALSE
```

```r
! (b < a) # ! niega la condición
```

```
## [1] TRUE
```

```r
2**2 == 2^2
```

```
## [1] TRUE
```

```r
3*2 == 3^2
```

```
## [1] FALSE
```


Nótese la diferencia entre `=` (asignación) y `==` (operador lógico)

```r
2 == 3
```

```
## [1] FALSE
```

```r
# 2 = 3 # produce un error:
# Error en 2 = 3 : lado izquierdo de la asignación inválida (do_set)
```

Se pueden encadenar varias condiciones lógicas utiilizando
los operadores `&` (y lógico) y `|` (o lógico).

```r
TRUE & TRUE
```

```
## [1] TRUE
```

```r
TRUE | TRUE
```

```
## [1] TRUE
```

```r
TRUE & FALSE
```

```
## [1] FALSE
```

```r
TRUE | FALSE
```

```
## [1] TRUE
```

```r
2 < 3 & 3 < 1
```

```
## [1] FALSE
```

```r
2 < 3 | 3 < 1
```

```
## [1] TRUE
```


Área de trabajo
---------------

Como ya se ha comentado
con anterioridad es posible guardar los comandos que se han utilizado en
una sesión en ficheros llamados **script**. En ocasiones interesará
además guardar todos los objetos que han sido generados a lo largo de
una sesión de trabajo.


El **Workspace** o **Área de Trabajo** es el entorno en el que se puede
guardar todo el trabajo realizado en una sesión. De este modo, la
próxima vez que se inicie el programa, al cargar dicho entorno, se podrá
acceder a lo objetos almacenados en él.

En primer lugar, para saber los objetos que tenemos en memoria se
utiliza la función `ls`. Por ejemplo, supongamos que acabamos de iniciar
una sesión de `R` y hemos escrito

```r
a <- 1:10
b <- log(50)
```


Entonces al utilizar `ls` se obtendrá la siguiente lista de objetos en
memoria

```r
ls()
```

```
## [1] "a" "b"
```

 También es posible borrar objetos a través de la función
`rm`

```r
rm(b)
ls()
```

```
## [1] "a"
```

Para borrar todos los objetos en memoria se puede utilizar
`rm(list=ls())`

```r
rm(list = ls())
```

```
## character(0)
```
`character(0)` (lista vacía)  significa que no hay objetos en memoria.

### Guardar y cargar resultados

Para guardar
el área de trabajo (Workspace) con todos los objetos de memoria (es
decir, los que figuran al utilizar `ls()`) se utiliza la función
`save.image(nombre archivo)`.

```r
rm(list = ls()) # primero borramos toda la mamoria
x <- 20
y <- 34
z <- "casa"
save.image(file = "prueba.RData") # guarda area de trabajo en prueba.RData
```


La función `save` permite guardar los objetos seleccionados.

```r
save(x, y, file = "prueba2.RData") # guarda los objetos x e y
```

Para cargar una ára de trabajo ya exitente se utiliza la función
`load()`.

```r
load("prueba2.RData") # carga área de trabajo
```

### Directorio de trabajo 

Por defecto `R` utiliza
una carpeta de trabajo donde guardará toda la información. Dicha carpeta
se puede obtener con la función

```r
getwd() 
```

```
## [1] "d:/"
```

El directorio de trabajo se puede cambiar utilizando `setwd(carpeta)`.
Por ejemplo, para cambiar el directorio de trabajo a `c:\datos`,
se utiliza el comando

```r
setwd("c:/datos")
# Importante la barra utilizada
# NO funciona setwd("c:\datos")
```

