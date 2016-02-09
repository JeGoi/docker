#################################################################
# Dockerfile
# Software:         TAPIR
# Software Version: 1.2
# Description:      Computational prediction of the localization of microRNAs within their pre-miRNA
# Website:          http://bioinformatics.psb.ugent.be/webtools/tapir/
# Tags:             Proteomics|Genomics|General
# Provides:         miRdup V1.2
# Dependencies :    vienna and bioperl
#################################################################

# Base docker image
FROM jego/vienna
MAINTAINER test@armadillo.uqam

# Install TAPIR and dependencies
RUN apt-get clean 

# Prepare installation
RUN apt-get --quiet update \
 && apt-get -qqy install \
    wget \
    bioperl \
    tar \
 && apt-get clean \
 && apt-get purge \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Create rep
RUN mkdir /TAPIR /data

# Get TAPIR
RUN cd /TAPIR \
 && wget --quiet http://bioinformatics.psb.ugent.be/webtools/tapir/tapir-1.2.tar.gz -O tapir-1.2.tar.gz \
 && tar -xzf tapir-1.2.tar.gz \
 && rm -f tapir-1.2.tar.gz \
 export PATH="${PATH}:/TAPIR/tapir-1.2"

RUN chmod -x -R /TAPIR

WORKDIR /TAPIR
