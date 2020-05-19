FROM r-base:3.4.4

MAINTAINER Florian Breitwieser "florian.bw@gmail.com"

## based on https://hub.docker.com/r/rocker/shiny/~/dockerfile/ by Winston Chang

RUN apt-get update && apt-get install -y -t unstable \
    sudo \
    gdebi-core \
    pandoc \
    pandoc-citeproc \
    libcurl4-gnutls-dev \
    libssl-dev/unstable \
    libcairo2-dev/unstable \
    libxt-dev\
    build-essential\
    libcurl4-gnutls-dev \
    libxml2-dev

# Download and install shiny server
RUN wget --no-verbose https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/VERSION -O "version.txt" && \
    VERSION=$(cat version.txt)  && \
    wget --no-verbose "https://s3.amazonaws.com/rstudio-shiny-server-os-build/ubuntu-12.04/x86_64/shiny-server-$VERSION-amd64.deb" -O ss-latest.deb && \
    gdebi -n ss-latest.deb && \
    rm -f version.txt ss-latest.deb

# Special installation for git2r ...
RUN wget https://github.com/ropensci/git2r/archive/master.zip; \
unzip master.zip && mv git2r-master git2r; \
cd git2r; \
make install

RUN R -e "options(repos = c(CRAN = 'http://cran.rstudio.com')); install.packages(c('devtools'));"
RUN R -e "source('https://bioconductor.org/biocLite.R'); biocLite('Rsamtools')"
RUN R -e "install.packages('devtools'); devtools::install_github('hrbrmstr/dtupdate'); install.packages('rappdirs');"
RUN mkdir /export/

ARG INCUBATOR_VER=unknown

RUN R -e "devtools::install_github('natay/pavian', upgrade=c('always'))"
COPY shiny-server.conf  /etc/shiny-server/shiny-server.conf
RUN rm -f /srv/shiny-server/*
RUN R -e "sapply(list.files(system.file('shinyapp',package='pavian'),full.names=TRUE),file.copy,to='/srv/shiny-server/',recursive=TRUE)"

EXPOSE 80

COPY shiny-server.sh /usr/bin/shiny-server.sh

CMD ["/usr/bin/shiny-server.sh"]