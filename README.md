# ash-paper-docker

Docker image for reproducing the simulation, analysis,
and paper of the [Adaptive Shrinkage](https://github.com/stephens999/ash/)
project.

## Installation

1. [Install Docker](https://docs.docker.com/installation/).

2. Pull the Docker image:

        docker pull stephenslab/ash-paper-docker:master

## Reproduce the simulation studies, analysis, and paper

1. Create a directory named `output` in the host system to store the output:

        cd $HOME && mkdir output

2. Run simulation:

        docker run -v $HOME/output:/home/rstudio/output --name ash1 stephenslab/ash-paper-docker:master make output

3. Run analysis:

        docker run -v $HOME/output:/home/rstudio/output --name ash2 stephenslab/ash-paper-docker:master make analysis && cp -r analysis/ output/analysis/

4. Compile paper:

        docker run -v $HOME/output:/home/rstudio/output --name ash3 stephenslab/ash-paper-docker:master make paper && cp -r paper/ output/paper/

In the host system, the output of the simulation studies will be accessible
from the `~/output` directory, the analysis will be in `~/output/analysis`,
the paper will be in `~/output/paper`.

> Note: the simulation takes about **10** hours to finish on a MacBook Pro Late 2013 (i5 2.3 GHz).
> Running analysis and compiling paper only take minutes to finish.

## Reproduce the analysis interactively

After the simulation is finished (the `~/output` directory will
contain a ~6GB output), run:

        docker run -d -p 8787:8787 -v $HOME/output:/home/rstudio/output --name ash4 stephenslab/ash-paper-docker:master

Open [http://localhost:8787](http://localhost:8787), use `rstudio`/`rstudio`
to log in to the RStudio Server. Compile and interact with the R Markdown
documents in the `analysis/` directory.

## For Mac and Windows Docker Users

Since Mac and Windows currently use a Linux VM with 2GB memory to run Docker
by default, the analysis step may run out of memory.

To adjust the VM memory size on Mac:

        boot2docker stop
        VBoxManage modifyvm boot2docker-vm --memory 6144
        boot2docker start

On Windows:

        /c/Program\ Files/Oracle/VirtualBox/VBoxManage.exe modifyvm boot2docker-vm --memory 6144

Or simply use the VirtualBox GUI to increase the memory size:
open VirtualBox, select "boot2docker-vm", click "settings", select "system",
adjust the memory size (~6GB will be sufficient).

## Clean Up

1. Stop and remove the containers:

        docker stop ash1 ash2 ash3 ash4
        docker rm ash1 ash2 ash3 ash4

2. Remove the Docker image:

        docker rmi stephenslab/ash-paper-docker:master

3. Remove the `~/output` directory.
