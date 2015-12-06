FROM ubuntu:14.04

RUN apt-get update && apt-get install -y  \
ipython  \
vim      \
wget     \
curl     \
git  

RUN curl -s https://raw.githubusercontent.com/torch/ezinstall/master/install-deps | bash
RUN git clone https://github.com/torch/distro.git /torch --recursive
RUN cd /torch; ./install.sh
