# ash-paper-docker

Docker image for reproducing the simulation, analysis,
and the paper of the [Adaptive Shrinkage](https://github.com/stephens999/ash/) project.

## Installation

* [Install Docker](https://docs.docker.com/installation/).

* Download the `ash-paper-docker` image:

        docker pull stephenslab/ash-paper-docker:master

## Reproduce the simulation studies, analysis, and paper

First, let's create a directory named `output` in the host system to store the output data:

        cd $HOME && mkdir output

Run simulation:

        docker run -v $HOME/output:/home/rstudio/output --name ash1 stephenslab/ash-paper-docker:master make output

Run analysis:

        docker run -v $HOME/output:/home/rstudio/output --name ash2 stephenslab/ash-paper-docker:master make analysis && cp -r analysis/ output/analysis/

Compile paper:

        docker run -v $HOME/output:/home/rstudio/output --name ash3 stephenslab/ash-paper-docker:master make paper && cp -r paper/ output/paper/

In the host system, the output of the simulation studies will be accessible from the `~/output` directory, the analysis will be in `~/output/analysis`, the paper will be in `~/output/paper`.

> Note: the simulation takes about 10 hours to finish on a MacBook Pro Late 2013 (i5 2.3 GHz).

## Reproduce the analysis interactively

After the simulation is finished (the `~/output` directory contains the data):

        docker run -d -p 8787:8787 -v $HOME/output:/home/rstudio/output --name ash4 stephenslab/ash-paper-docker:master

Open [http://localhost:8787](http://localhost:8787), use `rstudio`/`rstudio`
to log in to the RStudio Server. Compile and interact with the R Markdown
documents in the `analysis/` directory.

## Clean Up

* Stop and remove the running containers:

        docker stop ash1 ash2 ash3 ash4
        docker rm ash1 ash2 ash3 ash4

* Remove the `ash-paper-docker` image:

        docker rmi stephenslab/ash-paper-docker:master
