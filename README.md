# Base Linux

Dockerfile base for creating Docker projects by LaBiOS:

- Dockerfile base for the construction of Dugong Bioinformatics.
- Dockerfile base for the construction of SCIENV.

---

## Dockerfile Alpine:

- Compressed Size: 74 MB

```
FROM alpine:latest
MAINTAINER Fabiano Menegidio <fabiano.menegidio@biology.bio.br>

############### Environment config
ENV PASS dugong
ENV USER dugong
ENV HOME /headless
ENV UID 1000

RUN apk update && apk add --no-cache curl git vim bzip2 sudo bash build-base \    
    && curl "https://raw.githubusercontent.com/sgerrand/alpine-pkg-glibc/master/sgerrand.rsa.pub" -o /etc/apk/keys/sgerrand.rsa.pub \
    && curl -L "https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.26-r0/glibc-2.26-r0.apk" -o glibc.apk \
    && apk add glibc.apk \
    && curl -L "https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.26-r0/glibc-bin-2.26-r0.apk" -o glibc-bin.apk \
    && apk add glibc-bin.apk \
    && /usr/glibc-compat/sbin/ldconfig /lib /usr/glibc/usr/lib \
    && rm -rf glibc*apk /var/cache/apk/* \
    && adduser -s /bin/bash -u $UID -h $HOME -D $USER \
    && echo "$USER:$PASS" | chpasswd \
    && echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
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
```

---

## Dockerfile Ubuntu:

- Compressed Size: 149 MB

```
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
    && chown -R $USER:$USER $HOME    
    
USER $USER

RUN echo "############### Welcome to Dugong Bioinformatics ###############" \
    && echo "############### USER: dugong - PASS: dugong ###############"
    
VOLUME ["$HOME/data"]
CMD ["/bin/bash"]
```
