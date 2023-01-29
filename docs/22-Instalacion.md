# (APPENDIX) Apendices {-} 

# Instalación de R {#instalacion}




<!-- 
---
title: "Instalación de R"
author: "Estadística"
date: "Grado en Inteligencia Artificial (UDC)"
output: 
  bookdown::html_document2:
    toc: yes 
    # mathjax: local            # copia local de MathJax, hay que establecer:
    # self_contained: false     # las dependencias se guardan en ficheros externos 
  bookdown::pdf_document2:
    keep_tex: yes
    toc: yes 
---

bookdown::preview_chapter("22-Instalacion.Rmd")
knitr::purl("22-Instalacion.Rmd", documentation = 2)
knitr::spin("22-Instalacion.R",knit = FALSE)
-->

En la web del proyecto R ([www.r-project.org](http://www.r-project.org)) está disponible mucha información sobre este entorno estadístico.

\begin{figure}[!htb]

{\centering \includegraphics[width=0.45\linewidth]{figuras/rproject} \includegraphics[width=0.45\linewidth]{figuras/cran} 

}

\caption{Web de [R-project](https://r-project.org) y [CRAN](https://cran.r-project.org).}(\#fig:rproject)
\end{figure}

Las descargas se realizan a través de la web del CRAN (The Comprehensive R Archive Network), con múltiples [mirrors](https://cran.r-project.org/mirrors.html):

-  *Oficina de Software Libre (A Coruña)* (CIXUG): [ftp.cixug.es/CRAN](http://ftp.cixug.es/CRAN/).
-  *Spanish National Research Network (Madrid)* (RedIRIS): [cran.es.r-project.org](http://cran.es.r-project.org/).


## Instalación de R en Windows {#windows}

Seleccionando [Download R for Windows](http://ftp.cixug.es/CRAN/bin/windows/) y posteriormente [base](http://ftp.cixug.es/CRAN/bin/windows/base/) accedemos al enlace con el instalador de R para Windows (actualmente de la versión [4.2.2](http://ftp.cixug.es/CRAN/bin/windows/base/R-4.2.2-win.exe)).

\begin{figure}[!htb]

{\centering \includegraphics[width=0.5\linewidth]{figuras/R351} 

}

\caption{Web de [descarga de R para Windows](http://ftp.cixug.es/CRAN/bin/windows).}(\#fig:rdownload)
\end{figure}

### Asistente de instalación

Durante el proceso de instalación la recomendación (para evitar posibles problemas) es seleccionar ventanas simples SDI en lugar de múltiples ventanas MDI (hay que utilizar *Opciones de configuración*).

\begin{figure}[!htb]

{\centering \includegraphics[width=0.9\linewidth]{figuras/asistente} 

}

\caption{Pasos del asistente para instalación de R en Windows.}(\#fig:asistente)
\end{figure}

Una vez terminada la instalación, al abrir R aparece la ventana de la consola (simula una ventana de comandos de Unix) que permite ejecutar comandos de R.

Por defecto se instalan un conjunto de paquetes base de R (que se cargan automáticamente al iniciarlo) y un conjunto de [paquetes recomendados](https://cran.r-project.org/src/contrib/4.1.2/Recommended) (que se pueden cargar empleando el comando `library()`),
pero hay disponibles miles de paquetes que cubren literalmente todos los campos del análisis de datos. 
Ver por ejemplo:

- [CRAN: Packages](https://cran.r-project.org/web/packages/index.html)

- [CRAN: Task Views](https://cran.r-project.org/web/views)


### Instalación de paquetes {#paquetes-win}

Después de la instalación de R suele ser necesario instalar paquetes adicionales (puede ser recomendable *Ejecutar como administrador* R para evitar problemas de permiso de escritura en la carpeta *C:\\Program Files\\R\\R-X.Y.Z\\library*, o cambiar previamente los permisos de esta carpeta como se indica [aquí](#library)).



Para ejecutar los ejemplos mostrados en el libro sería necesario tener instalados los siguientes paquetes:
[`lattice`](https://CRAN.R-project.org/package=lattice), [`ggplot2`](https://CRAN.R-project.org/package=ggplot2), [`foreign`](https://CRAN.R-project.org/package=foreign), [`car`](https://CRAN.R-project.org/package=car), [`leaps`](https://CRAN.R-project.org/package=leaps), [`MASS`](https://CRAN.R-project.org/package=MASS), [`RcmdrMisc`](https://CRAN.R-project.org/package=RcmdrMisc), [`lmtest`](https://CRAN.R-project.org/package=lmtest), [`glmnet`](https://CRAN.R-project.org/package=glmnet), [`mgcv`](https://CRAN.R-project.org/package=mgcv), [`rmarkdown`](https://CRAN.R-project.org/package=rmarkdown), [`knitr`](https://CRAN.R-project.org/package=knitr), [`dplyr`](https://CRAN.R-project.org/package=dplyr), [`tidyr`](https://CRAN.R-project.org/package=tidyr).
Por ejemplo mediante los siguientes comandos:

```r
pkgs <- c("lattice", "ggplot2", "foreign", "car", "leaps", "MASS", "RcmdrMisc", 
          "lmtest", "glmnet", "mgcv", "rmarkdown", "knitr", "dplyr", "tidyr")
install.packages(setdiff(pkgs, installed.packages()[,"Package"]), dependencies = TRUE)
```
(puede que haya que seleccionar el repositorio de descarga, e.g. *Spain (Madrid)*).

El código anterior no reinstala los paquetes ya instalados, por lo que podrían aparecer problemas debidos a incompatibilidades entre versiones (aunque no suele ocurrir, salvo que nuestra instalación de R esté muy desactualizada). 
Si es el caso, en lugar de la última línea se puede ejecutar: 

```r
install.packages(pkgs, dependencies = TRUE) # Instala todos...
```


#### Cambiar los permisos de la carpeta *library* (opcional) {#library}

Para evitar problemas con la instalación de paquetes en Windows (y evitar también que los paquetes se instalen en *Documentos\\R\\win-library\\X.Y*) se puede dar permiso de *control total* a los usuarios del equipo en el subdirectorio *library* de la instalación de R.
Para ello, pulsar con el botón derecho en esta carpeta (e.g. *C:\\Program Files\\R\\R-4.2.2\\library*), seleccionar *Propiedades > Seguridad > Editar*, seleccionar los *Usuarios* del equipo, marcar *Control total* y *Aplicar*.

\begin{figure}[!htb]

{\centering \includegraphics[width=0.45\linewidth]{figuras/propiedades} \includegraphics[width=0.45\linewidth]{figuras/permisos} 

}

\caption{Pasos en Windows para cambiar permisos en la carpeta library.}(\#fig:permisos-library)
\end{figure}

### Instalación de RStudio Desktop {#ide}

Aunque la consola de R dispone de un editor básico de código (script), puede ser recomendable trabajar con un editor de comandos más cómodo y flexible.
El entorno de desarrollo (*Integrated Development Environment*, IDE) recomendado es [RStudio](https://posit.co/products/open-source/rstudio).
Está disponible para la mayoría de plataformas^[También hay una versión para servidores: [RStudio Server](https://posit.co/download/rstudio-server)] e integra una gran cantidad de herramientas, que permiten desde la generación de informes, hasta la gestión de distintos tipos de proyectos, depuración de código, control de versiones, etc.
También es compatible con otros lenguajes, incluido [Python](https://support.posit.co/hc/en-us/articles/1500007929061-Using-Python-with-the-RStudio-IDE).

Una vez instalado R, para instalar RStudio Desktop basta con descargar el correspondiente archivo de instalación de [*https://posit.co/download/rstudio-desktop*](https://posit.co/download/rstudio-desktop) y seguir las instrucciones.

Este entorno se describe en la Sección \@ref(rstudio).


#### Configuración adicional de RStudio (opcional) {#op-rstudio-win}

En lugar de emplear los visores de gráficos, ayuda y navegador web integrados, nos puede interesar que los gráficos se muestren en ventanas independientes y las páginas web en el navegador del equipo.
Esto se puede conseguir modificando los archivos de configuración (en el directorio *C:\\Program Files\\RStudio\\R* en Windows y  */Applications/RStudio.app/Contents/Resources/R* en Linux), que normalmente habrá que editar como administrador.

Por defecto los gráficos generados desde RStudio se mostrarán en la pestaña *Plots* panel inferior derecho y por ejemplo puede aparecer errores si el área gráfica es demasiado pequeña.
Para utilizar el dispositivo gráfico de R habría que modificar las siguientes líneas de *C:\\Program Files\\RStudio\\R\\Tools.R*:


```r
# set our graphics device as the default and cause it to be created/set
.rs.addFunction( "initGraphicsDevice", function()
{
   # options(device="RStudioGD")
   # grDevices::deviceIsInteractive("RStudioGD")
  grDevices::deviceIsInteractive()
})
```

El visor integrado de RStudio no resulta muy cómodo para navegar por la ayuda de las funciones (por ejemplo no permite hacer zoom o abrir múltiples ventanas).
Para utilizar en su lugar el navegador del equipo habría que comentar las siguientes líneas de *C:\\Program Files\\RStudio\\R\\Options.R*:


```r
# # custom browseURL implementation.
# .rs.setOption("browser", function(url)
# {
   # .Call("rs_browseURL", url, PACKAGE = "(embedding)")
# })
```



## Instalación de R en Ubuntu/Devian {#ubuntu}

Instalar dependencias:

```sh
sudo apt install libcurl4-gnutls-dev libgit2-dev libxml2-dev libssl-dev 
```

Si aparecen problemas asegurarse de que los repositorios *universe* y *multiverse* están disponibles:

```sh
sudo add-apt-repository universe
sudo add-apt-repository multiverse
sudo apt update
```

Se puede instalar R desde estos repositorios, pero normalmente no será la versión más actualizada y no lo recomendaría.


### Instalación de R desde [CRAN](https://cran.r-project.org/bin/linux/ubuntu)

Añadir la llave de firma GPG, añadir el repositorio CRAN a la lista de fuentes (para ver la versión de ubuntu se puede ejecutar `lsb_release -a`, el siguiente código ya la obtiene directamente) e instalar R:

```sh
# Cambiar a root (alternativamente añadir `sudo` al principio de los comandos)
sudo -i
# update indices
apt update -qq
# install two helper packages we need
apt install --no-install-recommends software-properties-common dirmngr
# add the signing key (by Michael Rutter) for these repos
# To verify key, run gpg --show-keys /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc 
# Fingerprint: 298A3A825C0D65DFD57CBB651716619E084DAB9
wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | sudo tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc
# add the R 4.0 repo from CRAN -- adjust 'focal' to 'groovy' or 'bionic' as needed
add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/"
apt-get update
apt-get install r-base r-base-dev
logout
```

### Instalación de devtools y demás paquetes

Ejecutar R en modo administrador para que los paquetes que se instalen estén disponibles para todos los usuarios
```sh
sudo -i R
```

Ejecutar en la consola de R
```r
install.packages('devtools', dependencies = TRUE)
```

Si aparecen problemas mirar [stackoverflow - Problems installing the devtools package](http://stackoverflow.com/a/20924082).

Instalar el resto de paquetes como se muestra en la sección de [Windows](#paquetes-win).


### Ayuda html

Si queremos la ayuda html (en un entorno gráfico con un navegador web instalado):

```sh
echo "options(help_type='html')" | sudo tee -a /etc/R/Rprofile.site
```

### Actualizar R

```sh
sudo apt-get update
sudo apt-get upgrade
```

### Instalacion de RStudio Desktop

Antes de nada, puede ser recomendable crear un directorio donde descargar el instalador:

```sh
mkdir installr
cd installr
```

Buscar la versión actualizada de RStudio en [Download RStudio](https://posit.co/download/rstudio-desktop) correspondiente a la versión de Ubuntu, en este caso emplearemos "https://download1.rstudio.org/electron/bionic/amd64/rstudio-2022.12.0-353-amd64.deb",
para Ubuntu 18+/Debian 10.

```sh
sudo apt-get install gdebi-core
wget https://download1.rstudio.org/electron/bionic/amd64/rstudio-2022.12.0-353-amd64.deb
sudo gdebi -n rstudio-2022.12.0-353-amd64.deb
```

Al igual que como se mostró para el caso de [Windows](#op-rstudio-win), nos puede interesar modificar la configuración de de RStudio para mostrar los gráficos en ventanas independientes y las páginas web en el navegador del equipo.
En este caso se procedería de forma idéntica, modificando los archivos de configuración en */Applications/RStudio.app/Contents/Resources/R*, editándolos como administrador.


## Instalación en Mac OS X {#macosx}

Instalar R de <http://cran.es.r‐project.org/bin/macosx> siguiendo los pasos habituales.

Para instalar R-Commander (<https://socialsciences.mcmaster.ca/jfox/Misc/Rcmdr/installation-notes.html>) es necesario disponer de las librerías gráficas X11, como a partir de OS X Lion ya no están instaladas por defecto en el sistema, hay que instalar las librerías Open Source XQuartz <https://www.xquartz.org>. 

Finalmente, para instalar `Rcmdr` ejecutar en la consola de R: 

```r
install.packages("Rcmdr", dependencies = TRUE)
```

