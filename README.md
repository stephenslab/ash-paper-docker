# ash-paper-docker

Docker image for reproducing the simulation, analysis,
and the paper of the [Adaptive Shrinkage](https://github.com/stephens999/ash/) project.

## Installation

* [Install Docker](https://docs.docker.com/installation/).

* Download the `ash-paper-docker` image:

        docker pull stephenslab/ash-paper-docker:master

## Reproduce the simulation studies, analysis, and paper

        cd $HOME && mkdir output
        docker run -v $HOME/output:/home/rstudio/output --name ash stephenslab/ash-paper-docker:master make output
        docker run -v $HOME/output:/home/rstudio/output --name ash stephenslab/ash-paper-docker:master make analysis && cp -r analysis/ output/analysis/
        docker run -v $HOME/output:/home/rstudio/output --name ash stephenslab/ash-paper-docker:master make paper && cp -r paper/ output/paper/

In the host system, the output of the simulation studies will be accessible from the `~/output` directory, the analysis will be in `~/output/analysis`, the paper will be in `~/output/paper`.

> Note: the simulation may take more than 12 hours to finish.

## Reproduce the analysis interactively

After the simulation is finished (the `~/output` directory contains the result):

        docker run -d -p 8787:8787 -v $HOME/output:/home/rstudio/output --name ash stephenslab/ash-paper-docker

Open [http://localhost:8787](http://localhost:8787), use `rstudio`/`rstudio`
to log in to the RStudio Server. Compile and interact with the R Markdown
documents in the `analysis/` directory.

## Clean Up

* Stop and remove the running container `ash`:

        docker stop ash
        docker rm ash

* Remove the `ash-paper-docker` image:

        docker rmi stephenslab/ash-paper-docker:master
