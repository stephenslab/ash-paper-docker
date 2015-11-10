# ash-paper-docker

`ash-paper-docker` provides a Docker image for reproducing the simulation, analysis,
and the paper of the [ash](https://github.com/stephens999/ash/) project,
It is also a production-ready environment for running [ashr](https://github.com/stephens999/ashr/).

## Installation

* [Install Docker](https://docs.docker.com/installation/).

* Download the `ash-paper-docker` image and run a container named `ash`:

        docker pull stephenslab/ash-paper-docker

## Reproduce the simulation studies, analysis, and paper

Run `make` in the container to generate the simulation results, analysis, paper, and make them available in a folder `output` accessible from the host:

        cd /home/user/
        mkdir output
        docker run -v /home/rstudio/output:/home/user/output --name ash stephenslab/ash-paper-docker make output
        docker run -v /home/rstudio/output:/home/user/output --name ash stephenslab/ash-paper-docker make analysis && cp -r analysis/ output/analysis/
        docker run -v /home/rstudio/output:/home/user/output --name ash stephenslab/ash-paper-docker make paper && cp -r paper/ output/paper/

> Note 1: replace `user` with your system user name.

> Note 2: running this will take several hours.

## Reproduce the analysis interactively

        docker run -d -p 8787:8787 -v /home/rstudio/output:/home/user/output --name ash stephenslab/ash-paper-docker

* Open [http://localhost:8787](http://localhost:8787), use `rstudio`/`rstudio`
to log in to the RStudio Server. Compile and interact with the R Markdown
documents in the `analysis/` directory.

## Clean Up

* To stop and remove the running container `ash`:

        docker stop ash
        docker rm ash

* To remove the `ash-paper-docker` image:

        docker rmi stephenslab/ash-paper-docker
