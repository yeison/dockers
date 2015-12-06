FROM devyeison/cuda-opencv-boost:7.5

RUN apt-get update && apt-get install -y  \
python			\
python-zmq 		\
#ipython    		\
#ipython-notebook    	\
python-pip             	\
vim        		\
wget       		\
curl       		\
git  

RUN pip install jupyter

RUN curl -s https://raw.githubusercontent.com/torch/ezinstall/master/install-deps | bash
RUN git clone https://github.com/torch/distro.git /torch --recursive
RUN cd /torch; ./install.sh
ENV PATH=/torch/install/bin/:$PATH

ENV LUA_HISTORY=/torchdata/.luahistory

RUN echo ". /torch/install/bin/torch-activate" >> /etc/bash.bashrc
RUN /bin/bash --login -c "luarocks install image"
RUN /bin/bash --login -c "luarocks install optim"
RUN /bin/bash --login -c "luarocks list"

