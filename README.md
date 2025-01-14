## Why should you use these docker containers?

* If you do not want to install every software needed for the Polyploid Training Workshop
* If you want to run every software via RStudio in your web browser
* If you want to use a cluster to run the analysis

All images will run on top of your OS kernel, so they are cross-platform enabled, do not need any configuration, and are ready to go.

## What will you find in this repository?

* [Docker installation instructions](#installing-docker)

* Images to run self-contained containers with the following software:

    * [SuperMASSA/VCF2SM](https://github.com/guilherme-pereira/vcf2sm)
    * [polyRAD](https://github.com/lvclark/polyRAD)
    * [fitPoly](https://cran.r-project.org/web/packages/fitPoly/index.html)
    * [MAPpoly](https://cran.r-project.org/web/packages/mappoly/index.html)
    * [polymapR](https://cran.r-project.org/web/packages/polymapR/index.html)
    * [PolyOrigin](https://github.com/chaozhi/PolyOrigin.jl)
    * [QTLpoly](https://github.com/guilherme-pereira/QTLpoly)
    * [polyqtlR](https://cran.r-project.org/web/packages/polyqtlR/index.html)
    * [diaQTL](https://github.com/jendelman/diaQTL)
    * [GWASpoly](https://github.com/jendelman/GWASpoly)

## Installing Docker

**Linux users:** use the following instructions to [Install Docker on Ubuntu](https://docs.docker.com/engine/install/ubuntu/), or find your distribution [here](https://docs.docker.com/engine/install/).

**Mac users:** download the [Docker Desktop Installer for Mac](https://desktop.docker.com/mac/stable/Docker.dmg) and follow the [Installation instructions for Mac](https://docs.docker.com/docker-for-mac/install/).

**Windows users:** first, [install and configure Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/install-win10#manual-installation-steps) (steps 1-5 only). Then, download the [Docker Desktop Installer for Windows](https://desktop.docker.com/win/stable/Docker%20Desktop%20Installer.exe) and run it. Once the installation finishes, you will be able to open a Windows Powershell terminal and run the commands under the `Running instructions` sessions below. If you have any problem, please follow the [Installation instructions for Windows](https://docs.docker.com/docker-for-windows/install/) or ask for help on our Slack Channel.

**Cluster users**: most computing clusters already have Docker or Singularity installed - you can jump to the next session, download, and run the images. If your cluster does not have any image rendering system, please contact your system administrator. This [information](https://singularity.lbl.gov/install-request) may help you to elaborate a request.

Docker requires administrator permissions to run. If you do not have these permissions, you can use [Singularity](https://singularity.lbl.gov/). [Here are](https://singularity.lbl.gov/docs-docker) some instructions about how to use Docker images with Singularity.

## Docker useful commands

After installing docker here are some useful commands:

```
# Dowload images and run containers
docker pull                               # Get image from a registry
docker run                                # run a command in a new container
docker run -it -v                         # run in an interactive mode and transferring directory to the container environment

# Delete container
docker ps                                 # list all containers available on your computer, search the container you want to delete
docker stop <cointainer_id>               # before delete you should stop it
docker rm <cointainer_id>                 # delete container using the same ID

# Delete image
docker images                             # list all images available on your computer, search the image ID that you want to delete
docker rmi <image_id>                     # delete the image using the same ID

# Delete all images and containers
docker stop $(docker ps -a -q)            # stop all running containers
docker rm $(docker ps -a -q)              # remove all stopped containers
docker rmi $(docker images -q)            # remove all images
```

## Genotype calling software

* Docker Hub image: [cristaniguti/poly_genocalls](https://hub.docker.com/repository/docker/cristaniguti/poly_genocalls)

<ins>**Image contents**</ins>

* Ubuntu 20.04
* R 4.0.3
* RStudio 1.3
* Python 2.7
* SuperMASSA
* VCF2SM
* polyRAD
* fitPoly

<ins>**Running instructions**</ins>

**Download the container image:** open a terminal and run: `docker pull cristaniguti/poly_genocalls`

Then start the image by running the following commands (feel free to choose another port different than 8787):

```
# Running VCF2SM (Linux/Mac)
docker run -v $(pwd):/home/user/ cristaniguti/poly_genocalls python /scripts/VCF2SM.py -i /opt/subset.vcf -o /home/user/example_poly.vcf -S /scripts/SuperMASSA.py -I hw -a RA/AA -r 1:84 -d 15 -D 500 -M 4:6 -f 4 -p 0.80 -n 0.90 -c 0.75 -t 1

# Running VCF2SM (Windows)
docker run -v ${pwd}:/home/user/ cristaniguti/poly_genocalls python /scripts/VCF2SM.py -i /opt/subset.vcf -o /home/user/example_poly.vcf -S /scripts/SuperMASSA.py -I hw -a RA/AA -r 1:84 -d 15 -D 500 -M 4:6 -f 4 -p 0.80 -n 0.90 -c 0.75 -t 1

# Access the RStudio (Linux/Mac)
docker run -p 8787:8787 -v $(pwd):/home/rstudio/ -e DISABLE_AUTH=true cristaniguti/poly_genocalls

# Access the RStudio (Windows)
docker run -p 8787:8787 -v ${pwd}:/home/rstudio/ -e DISABLE_AUTH=true cristaniguti/poly_genocalls
```

Keep this terminal open and access `http://localhost:8787/` or `http://127.0.0.1:8787/` in your web browser. You can also run the VCF2SM using the RStudio terminal, but do not forget the path of the scripts inside the container: `/scripts/SuperMASSA.py` and `/scripts/VCF2SM.py`. If you want to load files or folders into the container, simply run the commands above from a directory containing them (you can use commands such as `cd` and `cd ..` to navigate through directories).

**Warning:** container images can be large depending on their content. If you do not plan to use the images soon and want to free up some storage space, please see [Docker useful commands session](#docker-useful-commands) to remove them.

## Linkage Mapping and Haplotype Reconstruction

* Docker Hub image: [cristaniguti/poly_haplo](https://hub.docker.com/repository/docker/cristaniguti/poly_haplo)


<ins>**Image contents**</ins>

* Ubuntu 20.04
* R 4.0.3
* RStudio 1.3
* Julia 1.5.3
* MAPpoly
* polymapR
* PolyOrigin


<ins>**Running instructions**</ins>

**Download the container image:** open a terminal and run: `docker pull cristaniguti/poly_haplo`

Then start the image by running the following commands (feel free to choose another port different than 8787):

```
# Running Julia (Linux/Mac)
docker run -it -v $(pwd):/opt cristaniguti/poly_haplo /scripts/./julia

# Running Julia (Windows)
docker run -it -v ${pwd}:/opt cristaniguti/poly_haplo /scripts/./julia

# Access the RStudio (Linux/Mac)
docker run -p 8787:8787 -v $(pwd):/home/rstudio/ -e DISABLE_AUTH=true cristaniguti/poly_haplo

# Access the RStudio (Windows)
docker run -p 8787:8787 -v ${pwd}:/home/rstudio/ -e DISABLE_AUTH=true cristaniguti/poly_haplo
```

Keep this terminal open and access `http://localhost:8787/` or `http://127.0.0.1:8787/` in your web browser. You can also run the Julia using the RStudio terminal, but do not forget the path of the script inside the container: `/scripts/julia`. If you want to load files or folders into the container, simply run the commands above from a directory containing them (you can use commands such as `cd` and `cd ..` to navigate through directories).

With RStudio, try to load any of the included R packages:

```
# Loading MAPpoly
library(mappoly)
plot(solcap.err.map[[1]])
```

![image](https://user-images.githubusercontent.com/7572527/103160220-dc411100-47b0-11eb-806d-e3ed7f2c0d78.png)

If you see the same plot shown above, you are ready to go! We recommend that you start by following the vignettes and examples for each package.

**Warning:** container images can be large depending on their content. If you do not plan to use the images soon and want to free up some storage space, please see [Docker useful commands session](#docker-useful-commands) to remove them.

## QTL mapping

* Docker Hub image: [cristaniguti/poly_qtl](https://hub.docker.com/repository/docker/cristaniguti/poly_qtl)

<ins>**Image contents**</ins>

* Ubuntu 20.04
* R 4.0.3
* RStudio 1.3
* QTLpoly
* diaQTL
* GWASpoly

<ins>**Running instructions**</ins>

**Download the container image:** open a terminal and run: `docker pull cristaniguti/poly_qtl`

Then start the image by running the following commands (feel free to choose another port different than 8787):

```
# Access the RStudio (Linux/Mac)
docker run -p 8787:8787 -v $(pwd):/home/rstudio/ -e DISABLE_AUTH=true cristaniguti/poly_qtl

# Access the RStudio (Windows)
docker run -p 8787:8787 -v ${pwd}:/home/rstudio/ -e DISABLE_AUTH=true cristaniguti/poly_qtl
```

Keep this terminal open and access `http://localhost:8787/` or `http://127.0.0.1:8787/` in your web browser. If you want to load files or folders into the container, simply run the commands above from a directory containing them (you can use commands such as `cd` and `cd ..` to navigate through directories).

Inside RStudio, try to load any of the included packages:

```
# Loading QTLpoly
library(qtlpoly)
load(pheno)
head(pheno)
```

```
##              T32        T17        T45
## Ind_1 -0.1698446 -0.9332320 -1.2259338
## Ind_2  2.5319356  0.1997378 -1.8004184
## Ind_3  1.3669074  1.0584794 -0.7980037
## Ind_4  0.7955652 -1.7186921  1.5834176
## Ind_5  1.3168502 -0.7119421  1.3099067
## Ind_6 -0.8778211 -0.2339543 -0.6323779
```

If you see the output shown above, you are ready to go! We recommend that you start by following the vignettes and examples for each package.

**Warning:** container images can be large depending on their content. If you do not plan to use the images soon and want to free up some storage space, please see [Docker useful commands session](#docker-useful-commands) to remove them.

## Help us to improve

If you find any problem or want to add more software to the images, or even create a new one, feel free to send us messages, open issues, or pull requests. 
