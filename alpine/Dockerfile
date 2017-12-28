FROM alpine:latest
MAINTAINER Fabiano Menegidio <fabiano.menegidio@biology.bio.br>

############### Environment config
ENV PASS dugong
ENV USER dugong
ENV HOME /headless


RUN apk update && apk add --no-cache curl git vim bzip2 sudo \

    && mkdir -p $HOME/data \
    && mkdir -p $HOME/.ve \
    && mkdir -p $HOME/.workspace \
    && ln -s $HOME/.ve $HOME/data/.ve && ln -s $HOME/.workspace $HOME/data/.workspace \
    && chown -R $USER:$USER $HOME

USER $USER

RUN echo "############### Welcome to Dugong Bioinformatics ###############" \
    && echo "############### USER: dugong - PASS: dugong ###############"
    
VOLUME ["$HOME/data"]
CMD ["/bin/bash"]
