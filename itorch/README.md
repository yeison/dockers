# To run itorch at localhost:9999 

On Linux:

```mkdir -p $HOME/jupyter_workdir && docker run -p 9999:8888 -v $HOME/jupyter_workdir:/home/jovyan/work -it devyeison/jupyteritorch```

This will also create a directory ```jupyter_workdir```.  The directory will be used as the itorch working directory.  It will be loaded as a volume on the docker container.


The docker image contains the following:

-jupyter

-itorch

-torch7

-cuda

-opencv

-boost

Please see the Dockerfile for more details:

https://github.com/yeison/dockers/blob/master/itorch/Dockerfile

known issues:  there might be an issue with running more than one container at a time.
