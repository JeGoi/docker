#################################################################
# Dockerfile
# Software:         embassy-domsearch
# Software Version: 1:0.1.650-1
# Description:      Extra EMBOSS commands to search for protein domains
# Website:          http://emboss.sourceforge.net/what/
# Tags:             Proteomics|Genomics|General
# Provides:         emboss 6.6.0-1 && embassy-domsearch 1:0.1.650-1
#################################################################

# Base docker image
FROM ubuntu:14.04
MAINTAINER test@armadillo.uqam.ca

# Install EMBOSS amd embassy-domsearch package
RUN apt-get update && apt-get install -y \
    emboss=6.6.0-1 \
    embassy-domsearch=1:0.1.650-1 \
 && apt-get clean \
 && apt-get purge \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /data
