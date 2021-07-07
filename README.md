# Contributing to R - Participant Instructions useR! 2021 Tutorial
Gabriel Becker and Martin Maechler

## Code of Conduct

The [useR! 2021 conference Code of Conduct](https://user2021.r-project.org/participation/coc/)
 applies to all aspects of this tutorial and will be enforced throughout.  By joining the
 tutorial session you agree to abide by the code of conduct at all times in
 both the main tutorial call and all break-out 'rooms'. 

Please bring any violations to the code of conduct to the immediate
attention of Gabriel, Martin, or one of the helpers in attendence. 

## Zoom

The tutorial - like all of the live aspects of the useR! 2021 virtual
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

## Navigating the R sources:
[Accessing R Sources - Uwe Ligges](https://www.r-project.org/doc/Rnews/Rnews_2006-4_Ligges_AccessSource.pdf), an excerpt from
[R News 2006-4](https://cran.r-project.org/doc/Rnews/Rnews_2006-4.pdf); R Help Desk, p.43--45.

## R documentation ('Rd') syntax (not roxygen2)
[Writing R Documentation](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Writing-R-documentation-files) (Rd Syntax for package authors)

[Parsing Rd Files - Duncan Murdoch](https://developer.r-project.org/parseRd.pdf) (Rd Syntax Specification section)

## S3 Methods
Help pages [UseMethod](https://stat.ethz.ch/R-manual/R-patched/library/base/html/UseMethod.html) and [methods](https://stat.ethz.ch/R-manual/R-patched/library/base/html/methods.html)

[(S3) Object oriented Programming](https://cran.r-project.org/doc/manuals/r-release/R-lang.html#Object_002doriented-programming)

## Search Paths and NAMESPACEs

[Search Paths](https://cran.r-project.org/doc/manuals/r-release/R-ints.html#Search-paths) (and the following Namespaces section)

[Package Namespaces](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Package-namespaces)


## (Optional but helpful) Building R from Source and the C API

[Learning the C API](https://cran.r-project.org/doc/manuals/r-release/R-ints.html#R-Internal-Structures)

[Building R from sources](https://cran.r-project.org/doc/manuals/r-release/R-admin.html)


## (Optional but helpful) Regular Expressions (not stringi/stringr)

[Regular Expressions](https://cheatography.com/davechild/cheat-sheets/regular-expressions/)

R help pages [regex](https://stat.ethz.ch/R-manual/R-patched/library/base/html/regex.html) and [grep](https://stat.ethz.ch/R-manual/R-patched/library/base/html/grep.html)

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

If using the rstudio image, please read and understand the instructions for using the container ( [https://hub.docker.com/r/rocker/rstudio](https://hub.docker.com/r/rocker/rstudio)) and come ready and able to use the container.

If for some reason you choose not to use the rstudio based images, ensure your container has some other progam usable as an IDE (e.g., emacs with ess) installed.

Configure a volume for your container, like so (see full documentation for volumes here: [https://docs.docker.com/storage/volumes/](https://docs.docker.com/storage/volumes/))


```
docker volume create r-source
```

Ensure your volume was created using `docker volume ls`

```
Gabriels-MacBook-Pro:rtables_paper gabrielbecker$ docker volume ls
DRIVER    VOLUME NAME
<snip>
local     r-source
```

Then start a shell in your docker container with the volume mounted

```
docker run -it --mount src=r-source,target=/r-source rocker/rstudio:3.3.2 bash
root@788c85efe291:/# 
```

By doing `ls` we can see that our mounted volume is there:

```
root@788c85efe291:/# ls
bin  boot  dev	etc  home  init  lib  lib64  media  mnt  opt  proc  root  r-source  run  sbin  srv  sys  tmp  usr  var
```

Then navigate to that directory and use `wget` and `tar` to download and untar the R 3.3.2 source tarball ( [https://cloud.r-project.org/src/base/R-3/R-3.3.2.tar.gz](https://cloud.r-project.org/src/base/R-3/R-3.3.2.tar.gz) ) onto it. 

```
root@93dde2224f94:/r-source# wget https://cloud.r-project.org/src/base/R-3/R-3.3.2.tar.gz
--2021-07-02 17:20:01--  https://cloud.r-project.org/src/base/R-3/R-3.3.2.tar.gz
Resolving cloud.r-project.org (cloud.r-project.org)... 204.246.191.121, 204.246.191.87, 204.246.191.77, ...
Connecting to cloud.r-project.org (cloud.r-project.org)|204.246.191.121|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 29440670 (28M) [application/x-gzip]
Saving to: ‘R-3.3.2.tar.gz’

R-3.3.2.tar.gz                                                100%[================================================================================================================================================>]  28.08M  9.03MB/s   in 3.1s   

2021-07-02 17:20:04 (9.03 MB/s) - ‘R-3.3.2.tar.gz’ saved [29440670/29440670]

root@93dde2224f94:/r-source# tar -xvf R-3.3.2.tar.gz 
R-3.3.2/
R-3.3.2/ChangeLog
R-3.3.2/config.site
R-3.3.2/configure
<snip>
```

We will explore and use portions of these sources in the practical portions of the tutorial.

Ensure these files are persistent by closing the docker container (e.g. via `exit`, and invoking it again to ensure the files are still there:

```
root@788c85efe291:/# exit
exit
Gabriels-MacBook-Pro:rtables_paper gabrielbecker$ cd ~
Gabriels-MacBook-Pro:~ gabrielbecker$ docker run -it --mount src=r-source,target=/r-source rocker/rstudio:3.3.2 bash
root@15a8d340686a:/# cd r-source/
root@15a8d340686a:/r-source# ls
R-3.3.2  R-3.3.2.tar.gz
```

When invoking the Rstudio container as described at  [https://hub.docker.com/r/rocker/rstudio](https://hub.docker.com/r/rocker/rstudio)) be sure to remember to add ` --mount src=r-source,target=/r-source` to the invocation.



# Practicum 2 - Bugs to Choose From
## Present in 3.3.2

### `as.person` not handling multiple emails

```
joe <- person("Joe", "Schmo", email = c("first@site.com", "second@uni.edu"))
str(joe$email)
##  chr [1:2] "first@site.com" "second@uni.edu"

joe_text <- format(joe)
print(joe_text)
## [1] "Joe Schmo <first@site.com, second@uni.edu>"

joe_new <- as.person(joe_text)
str(joe_new$email)
##  chr "first@site.com, second@uni.edu"
```


### `is.ratetablle` inconsistent between `verbose=TRUE` and `verbose=FALSE`

```
library(survival)
library(relsurv)
data("slopop")
is.ratetable(slopop)
# [1] TRUE
is.ratetable(slopop, verbose = TRUE)
# [1] "wrong length for cutpoints 3"
```

### `diff` on `difftime` objects losing units

```
d <- as.POSIXct("2016-06-08 14:21", tz="US/Pacific") + as.difftime(2^(-2:8), units="mins")
str(d)
# POSIXct[1:11], format: "2016-06-08 14:21:15" "2016-06-08 14:21:30" ...
str(diff(d))
#Class 'difftime'  atomic [1:10] 15 30 60 120 240 480 960 1920 3840 7680
#  ..- attr(*, "units")= chr "secs"
str(diff(diff(d)))
#Class 'difftime'  num [1:9] 15 30 60 120 240 480 960 1920 3840
```

### `addmargins()` fails if supplied functions are not defined in the `stats` namespace or parent environment thereof

```
local({
    
    mB <- structure(c(16, 26, 27, 20, 24, 20, 19, 25, 40, 46, 46, 45), 
    .Dim = c(4L,  3L), 
    .Dimnames = list(Sea = c("Black", "Dead", "Red",  "White"), 
                     Bee = c("Buzz", "Hum", "Total")), 
    class = c("table", "matrix"))
    
    sqsm <- function(x) sum(x)^2/100

    addmargins(mB, 1, list(list(All = sum, N = sqsm)))

})
```

### Subsetting data.frame with factor column does not strip additional column class

```
data(iris)

lapply(iris, class)
Species2 <- iris$Species
Sepal.Length2 <- iris$Sepal.Length

class(Species2) <- c("some_class", class(Species2))
class(Sepal.Length2) <- c("some_class", class(Sepal.Length2))

attr(Species2, "some_attr") <- "some_attr_val"
attr(Sepal.Length2, "some_attr") <- "some_attr_val"

iris$Species2 <- Species2
iris$Sepal.Length2 <- Sepal.Length2

lapply(iris, class)
lapply(iris, attributes)

iris2 <- iris[c(2:5), ] # row-wise subsetting
lapply(iris2, class)    # Species2: class c("some_class", "factor"), Sepal.Length2 class stripped to numeric
lapply(iris2, attributes) # all (incl. Species2): "some_attr" is stripped
```


## In Latest Release/Unresolved

### data.frame subsetting issue listed above

### `debugcall` fails after loading `mgcv` or `survival`

```
library("mgcv")  # or "survival", or just loadNamespace("Matrix")
f <- factor(1:10)
debugcall(summary(f))
```

### What `substring(., last=*)` should default to

- Search for "Should last default to" / "for substring()" in the 
  [R-devel list archives](https://stat.ethz.ch/pipermail/r-devel/2021-June/)
- Look at the first 2--3 mails; what do you think? how would you solve it?
