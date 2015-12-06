FROM devyeison/torch7:latest

ENV LUA_HISTORY=/torchdata/.luahistory
RUN echo ". /torch/install/bin/torch-activate" >> /etc/bash.bashrc
RUN /bin/bash --login -c "luarocks install image"
RUN /bin/bash --login -c "luarocks install optim"
RUN /bin/bash --login -c "luarocks list"
