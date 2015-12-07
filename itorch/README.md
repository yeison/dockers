To run itorch on port 9999 :

```mkdir jupyter_workdir && \ 
docker run -p 9999:8888 -v $HOME/jupyter_workdir:/home/jovyan/work -it devyeison/jupyteritorch```



This docker image contains the following:

-jupyter

-itorch

-torch7

-cuda

-opencv

-boost

Please see the Dockerfile for more details:

https://github.com/yeison/dockers/blob/master/itorch/Dockerfile

known issues:  there might be an issue with running more than one container at a time.
