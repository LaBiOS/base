FROM ubuntu:latest
MAINTAINER Fabiano Menegidio <fabiano.menegidio@biology.bio.br>

############### Environment config
ENV DEBIAN_FRONTEND noninteractive
ENV PASS dugong
ENV USER dugong
ENV HOME /headless

RUN apt-get update && apt-get install -y --allow-unauthenticated git curl vim bzip2 sudo build-essential \
    && apt-get clean && apt-get autoremove && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && echo "User: $USER Pass: $PASS" \
    && useradd -d $HOME --shell /bin/bash --user-group --groups adm,sudo $USER \
    && echo "$USER:$PASS" | chpasswd \
    && echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    && mkdir -p $HOME/data \
    && mkdir -p $HOME/.ve \
    && mkdir -p $HOME/.workspace \
    && ln -s $HOME/.ve $HOME/data/.ve && ln -s $HOME/.workspace $HOME/data/.workspace \
    && chown -R $USER:$USER $HOME \
    && curl -fsSL "https://github.com/krallin/tini/releases/download/v0.16.1/tini" \
    && mv tini /usr/local/bin/tini && chmod +x /usr/local/bin/tini
    
USER $USER

RUN echo "############### Welcome to Dugong Bioinformatics ###############" \
    && echo "############### USER: dugong - PASS: dugong ###############"

ENTRYPOINT ["tini", "--"]
VOLUME ["$HOME/data"]
CMD ["/bin/bash"]
