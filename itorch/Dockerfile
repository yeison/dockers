FROM devyeison/cuda-opencv-boost:7.5


RUN apt-get update && apt-get install -y  \
python			\
python-zmq 		\
python-pip             	\
vim        		\
wget       		\
curl       		\
git  

#Jupyter
RUN apt-get install -yq --no-install-recommends \
    git \
    vim \
    wget \
    build-essential \
    python-dev \
    ca-certificates \
    bzip2 \
    unzip \
    libsm6 \
    pandoc \
    texlive-latex-base \
    texlive-latex-extra \
    texlive-fonts-extra \
    texlive-fonts-recommended \
    sudo \
    && apt-get clean

# Install Tini
RUN wget --quiet https://github.com/krallin/tini/releases/download/v0.6.0/tini && \
    echo "d5ed732199c36a1189320e6c4859f0169e950692f451c03e7854243b95f4234b *tini" | sha256sum -c - && \
    mv tini /usr/local/bin/tini && \
    chmod +x /usr/local/bin/tini

# Configure environment
ENV CONDA_DIR /opt/conda
ENV PATH $CONDA_DIR/bin:$PATH
ENV SHELL /bin/bash
ENV NB_USER jovyan
ENV NB_UID 1000

# Install conda
RUN mkdir -p $CONDA_DIR && \
    echo export PATH=$CONDA_DIR/bin:'$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-3.9.1-Linux-x86_64.sh && \
    echo "6c6b44acdd0bc4229377ee10d52c8ac6160c336d9cdd669db7371aa9344e1ac3 *Miniconda3-3.9.1-Linux-x86_64.sh" | sha256sum -c - && \
    /bin/bash /Miniconda3-3.9.1-Linux-x86_64.sh -f -b -p $CONDA_DIR && \
    rm Miniconda3-3.9.1-Linux-x86_64.sh && \
    $CONDA_DIR/bin/conda install --yes conda==3.14.1

# Install Jupyter notebook
RUN conda install --yes \
    'notebook=4.0*' \
    terminado \
    && conda clean -yt

# Create jovyan user with UID=1000 and in the 'users' group
# Grant ownership over the conda dir and home dir, but stick the group as root.
RUN useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    mkdir /home/$NB_USER/work && \
    mkdir /home/$NB_USER/.jupyter && \
    mkdir /home/$NB_USER/.local && \
    chown -R $NB_USER:users $CONDA_DIR && \
    chown -R $NB_USER:users /home/$NB_USER


# Lua
RUN curl -s https://raw.githubusercontent.com/torch/ezinstall/master/install-deps | bash
RUN git clone https://github.com/torch/distro.git /torch --recursive
RUN cd /torch; ./install.sh
ENV PATH=/torch/install/bin/:$PATH

ENV LUA_HISTORY=/torchdata/.luahistory

RUN echo ". /torch/install/bin/torch-activate" >> /etc/bash.bashrc
RUN /bin/bash --login -c "luarocks install image"
RUN /bin/bash --login -c "luarocks install optim"
RUN /bin/bash --login -c "luarocks list"


# Configure container startup
EXPOSE 8888
WORKDIR /home/$NB_USER/work
#ENTRYPOINT ["tini", "--"]
#CMD ["start-notebook.sh"]

# Add local files as late as possible to avoid cache busting
RUN chown -R $NB_USER:users /home/$NB_USER/.jupyter
RUN cp -r /root/.ipython /home/$NB_USER/
RUN chown $NB_USER /home/$NB_USER/.ipython
RUN jupyter migrate
COPY start-notebook.sh /usr/local/bin/
COPY jupyter_notebook_config.py /home/$NB_USER/.jupyter/
