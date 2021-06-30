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

The tutorial - like all of the live aspects of the useR!2021 virtual
conference - will take place on **Zoom**. Unlike most sessions in the
conference, the tutorial will be a zoom meeting, not a webinar. As
such it cannot be attended via web browser. **Please ensure you have
an up-to-date Zoom client installed on your device prior to the start
of the tutorial.**

Portions of this tutorial will be participatory so a **working zoom-compatible microphone** and a **location where you can speak loud enough to be understood** are required. 

A working camera and internet connection stable enough to transmit video are strongly preferred, if possible.

## Recording

Part or all of this tutorial will be recorded and made available to the wider R community. By joining and remaining in the session you agree to have this tutorial and your participation in it recorded and the resulting video made publicly available.

# Preparing for the Session

This tutorial will assume certain baseline knowledge about how R works in order to effectively use the short amount of time we have. 

Please read or otherwise be familiar with the subjects covered in the following:

## Navigating the R soources:
[Accessing R Sources - Uwe Ligges](https://www.r-project.org/doc/Rnews/Rnews_2006-4_Ligges_AccessSource.pdf), an excerpt from
[R News 2006-4](https://cran.r-project.org/doc/Rnews/Rnews_2006-4.pdf); R Help Desk, p.43--45.

## R documentation syntax (not roxygen2)
[Parsing Rd Files - Duncan Murdoch](https://developer.r-project.org/parseRd.pdf) (Rd Syntax Specification section)

## Search Paths and NAMESPACEs

[Search Paths](https://cran.r-project.org/doc/manuals/r-release/R-ints.html#Search-paths) (and the following Namespaces section)

[Package Namespaces](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Package-namespaces)


## (Optional but helpful) Building R from Source and the C API

[Learning the C API](https://cran.r-project.org/doc/manuals/r-release/R-ints.html#R-Internal-Structures)

[Building R from sources](https://cran.r-project.org/doc/manuals/r-release/R-admin.html)


## (Optional but helpful) Regular Expressions (not stringi/stringr)

[Regular Expressions](https://cheatography.com/davechild/cheat-sheets/regular-expressions/)


# Docker and Rocker

For for the practical portions of this tutorial, participants will use use Eddelbuettel and Boettiger's
 [rocker](https://www.rocker-project.org/) project to step into older versions of R to explore real bugs which have since been fixed.

**Prior to the start of the session** please have docker installed and a rocker image version with R 3.3.2 pulled and tested (`rocker/r-ver:3.3.2` or `rocker/rstudio:3.3.2`).

Participants unable to run docker on the device they will be joining the session from will be placed
within breakout groups where at a least one participant does have the setup working, and can
share their screen, but working along locally is still preferable.

__Note__ you can *also* use `podman` instead of `docker`, notably on
Fedora/Redhat/... Linux systems where it is well supported.  
In the terminal, simply replace the word `docker` by `podman`.

If using the rstudio image, please read and understand the insturctions for using the container ( [https://hub.docker.com/r/rocker/rstudio](https://hub.docker.com/r/rocker/rstudio)) and come ready and able to use the container.

If for some reason you choose not to use the rstudio based images, ensure your container has some other progam usable as an IDE (e.g., emacs with ess) installed.

Configure a volume for your container, using the instructions: [https://docs.docker.com/storage/volumes/](https://docs.docker.com/storage/volumes/)

Use that volume when running the container, and download and untar the R 3.3.2 source tarball ( [https://cloud.r-project.org/src/base/R-3/R-3.3.2.tar.gz](https://cloud.r-project.org/src/base/R-3/R-3.3.2.tar.gz) ) onto it. We will explore and use portions of these sources in the practical portions of the tutorial.

To get an interactive shell in your docker container (rather than starting R or Rstudio server), you can do

`docker run -it rocker/rstudio:3.3.2 bash`







