FROM ubuntu:20.04

LABEL maintainer="Suneel Dondapati <dsuneel1@gmail.com>"

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

USER root
# Neovim configuration (SU)
RUN apt-get update --fix-missing && \
    apt-get install -y wget git curl neovim tmux && \
    curl -sL https://deb.nodesource.com/setup_12.x | bash - && \
    apt-get install -y nodejs && \
    apt-get clean && \
    useradd --create-home --shell /bin/bash suneel

ENV HOME /home/suneel
ENV DF /home/suneel/dotfiles

# Miniconda installation
ENV PATH /opt/conda/bin:$PATH
ENV PYTHON_VERSION=3.7
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    /opt/conda/bin/conda install -y python=$PYTHON_VERSION && \
    /opt/conda/bin/conda clean -tipsy && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc

USER suneel
# Neovim configuration
SHELL ["/bin/bash", "-c"]
RUN git clone https://github.com/suneel123/dotfiles $DF && \
    mkdir -p $HOME/.config/nvim && \
    cp -r $DF/nvim/* $HOME/.config/nvim/ && \
    nvim -u $HOME/.config/nvim/init.vim -i NONE -c 'PlugInstall' -c 'qa' && \
    mv $HOME/.config/nvim/coc-extensions.sh /tmp/ && \
    chmod +x /tmp/coc-extensions.sh && \
    /bin/bash /tmp/coc-extensions.sh && \
    pip install pynvim

