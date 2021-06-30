# Contributing to R - Participant Instructions useR2021 Tutorial
Gabriel Becker and Martin Maechler

## Code of Conduct

The [useR2021 conference Code of Conduct](https://user2021.r-project.org/participation/coc/)
 applies to all aspects of this tutorial and will be enforced throughout.  By joining the
 tutorial session you agree to abide by the code of conduct at all times in
 both the main tutorial call and all break-out 'rooms'. 

Please bring any violations to the code of conduct to the immediate
attention of Gabriel, Martin, or one of the helpers in attendence. 

## Zoom

The tutorial - like all of the live aspects of the useR!2021 virtual conference - will take place on **Zoom**. 

Portions of this tutorial will be participatory so a **working zoom-compatible microphone** and a **location where you can speak loud enough to be understood** are required. 

A working camera and internet connection stable enough to transmit video are strongly preferred, if possible.

## Recording

Part or all of this tutorial will be recorded and made available to the wider R community. By joining and remaining in the session you agree to have this tutorial recorded and the resulting video made publicly available.



## Docker and Rocker

For for the practical portions of this tutorial, participants will use use Eddelbuettel and Boettiger's
 [rocker](https://www.rocker-project.org/) project to step into older versions of R to explore real bugs which have since been fixed.

**Prior to the start of the session** please have docker installed and the `rocker/r-ver:3.2.2` docker image pulled and tested.

Participants unable to run docker on the device they will be joining the session from will be placed
within breakout groups where at a least one participant does have the setup working, and can
share their screen, but working along locally is still preferable.

__Note__ you can *also* use `podman` instead of `docker`, notably on
Fedora/Redhat/... Linux systems where it is well supported.  
In the terminal, simply replace the word `docker` by `podman`.

With Docker installed, doing 

```
docker pull rocker/r-ver:3.2.2
```

Should be sufficient to retrieve the rocker image we will use. Once that is done,

```
docker run -i rocker/r-ver:3.2.2
```

Will be sufficient to start an interactive R session within that docker image. When yo do this, you should see


```
<your prompt>$ docker run -i rocker/r-ver:3.2.2

R version 3.2.2 (2015-08-14) -- "Fire Safety"
Copyright (C) 2015 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

```

and you should be able to run code, etc within that (quite old) version of R. We will use this fact to explore the bugs we've chosen as our case studies.


## Supplemental/Preparatory Reading

[Accessing R Sources - Uwe Ligges](https://www.r-project.org/doc/Rnews/Rnews_2006-4_Ligges_AccessSource.pdf), an excerpt from
[R News 2006-4](https://cran.r-project.org/doc/Rnews/Rnews_2006-4.pdf);
R Help Desk, p.43--45.


[Parsing Rd Files - Duncan Murdoch](https://developer.r-project.org/parseRd.pdf) (Rd Syntax Specification section)

