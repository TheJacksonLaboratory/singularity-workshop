---
title: "10 singularity build rstudio"
teaching: 35
exercises: 0
---

# Disclaimer 

This build of rstudio is for demonstration purposes only on these demo instances. For production purposes please consult your local system admin. 

## Containerizing Rstudio

CITE ROCKER AND RICHARD

```bash
cd /projects/my-lab/10-build-rstudio
```


To start, create a few directories Rstudio server looks for. 

```bash
workdir=/home/student

mkdir -p -m 700 ${workdir}/run ${workdir}/tmp ${workdir}/var/lib/rstudio-server

cat > ${workdir}/database.conf <<END
provider=sqlite
directory=/var/lib/rstudio-server
END
```
To start, let’s create an empty file to use as our recipe file. 
#lets build an Rstudio server and install a few R/Bioconductor pacages into the container. 

```bash
nano rdeseq2.def
```

```bash
Bootstrap: docker
From: rocker/tidyverse:4.2.1

#This def file doesn't seem to build on Sumner with Centos 7.  Richard suggests building on an Ubuntu system but we will stick with Centos 7.

%post

apt update
apt install -y libgeos-dev libglpk-dev # libcurl4-openssl-dev
apt install -y libcurl4-gnutls-dev libxml2-dev cmake  libbz2-dev

apt install -y libxslt1-dev  # install for sandpaper

R -e 'if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")

BiocManager::install("DESeq2")'
```

```bash
sudo time singularity build rdeseq2.sif rdeseq2.def
```

View files inside directory. Notice no R directory.

```bash
ls -lah 
```
Lets set the run time variables and deploy.

```bash
export SINGULARITY_BIND="${workdir}/run:/run,${workdir}/tmp:/tmp,${workdir}/database.conf:/etc/rstudio/database.conf,${workdir}/var/lib/rstudio-server:/var/lib/rstudio-server"
export SINGULARITYENV_USER=$(id -un)
export SINGULARITYENV_PASSWORD=password
singularity exec --cleanenv rdeseq2.sif rserver --www-port 8787 --auth-none=0 --auth-pam-helper-path=pam-helper --auth-stay-signed-in-days=30 --auth-timeout-minutes=0 --server-user  "student"
```

Lets load a package that from the container.

```bash
library('DESeq2')
```

Using the Rstudio file browser look at the R folder that appeared, but nothing is in it.

Lets install something to the user space.

```bash
BiocManager::install("EnhancedVolcano")
```

Now we see the local user install. 


Lets install few fun packages.
```bash
install.packages('knitr', dependencies = TRUE)
```

Lets install few fun packages.
```bash
options(repos = c(
  carpentries = "https://carpentries.r-universe.dev/", 
  CRAN = "https://cran.rstudio.com/"
))
install.packages("sandpaper", dep = TRUE)
```

```bash
library('sandpaper')
sandpaper::create_lesson("~/r-intermediate-penguins")
```

```bash
sandpaper::serve(quiet = FALSE, host = "0.0.0.0", port = "8789")
```

```bash
 servr::daemon_stop()
 ```


 #https://github.com/sachsmc/knit-git-markr-guide/blob/master/knitr/knit.Rmd


```bash
 ```{r setup, include=FALSE}
library(stringr)
library(knitr)
opts_chunk$set(tidy = FALSE)

knit_hooks$set(source = function(x, options){
  if (!is.null(options$verbatim) && options$verbatim){
    opts = gsub(",\\s*verbatim\\s*=\\s*TRUE\\s*", "", options$params.src)
    bef = sprintf('\n\n    ```{r %s}\n', opts, "\n")
    stringr::str_c(
      bef, 
      knitr:::indent_block(paste(x, collapse = '\n'), "    "), 
      "\n    ```\n"
    )
  } else {
    stringr::str_c("\n\n```", tolower(options$engine), "\n", 
      paste(x, collapse = '\n'), "\n```\n\n"
    )
  }
})
```

```{r my-first-chunk, results='asis', verbatim = TRUE} 
## code goes in here
```

Inline code is similar, using single backticks instead. Inline code does not have names or options. For example,  `r noquote("\x60r rnorm(10)\x60")`. 

Here's an example of raw output using the `mtcars` dataset:

```{r mtcars-example, verbatim = TRUE}
lm(mpg ~ hp + wt, data = mtcars)
```

And here's a plot

```{r mt-plot, verbatim = TRUE}
library(ggplot2)
ggplot(mtcars, aes(y = mpg, x = wt, size = hp)) + geom_point() + stat_smooth(method = "lm", se = FALSE)
```

The concept is very simple. Anything you want to do in `R` is incorporated into your document, the results alongside the code. The important details to learn are methods of controlling the output. This means making nice looking tables, decent figures, and formatting inline results. We will cover these topics next. 

Controlling `R` output
========================

### Tables

When outputting tables in knitr, it is important to use the option `results = 'asis'`. There are several options for formatting tables in `R`. The `knitr` package includes a function called `kable` that makes basic **k**nitr t**ables**. There are options to control the number of digits, whether row names are included or not, column alignment, and other options that depend on the output type. 

```{r kable, results = 'asis', verbatim = TRUE}
kable(head(mtcars), digits = 2, align = c(rep("l", 4), rep("c", 4), rep("r", 4)))
```
