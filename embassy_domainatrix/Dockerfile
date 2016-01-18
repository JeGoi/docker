#################################################################
# Dockerfile
# Software:         embassy-domainatrix
# Software Version: 0.1.650-1
# Description:      The DOMAINATRIX programs were developed by Jon Ison and colleagues at MRC HGMP for their protein domain research. They are included as an EMBASSY package as a work in progress. 
# Website:          http://emboss.sourceforge.net/what/
# Tags:             Proteomics|Genomics|General
# Provides:         emboss 6.6.0-1 embassy-domainatrix 0.1.650-1
#################################################################

# Base docker image
FROM ubuntu:14.04
MAINTAINER test@armadillo.uqam.ca

# Install EMBOSS amd embassy-domainatrix package
RUN apt-get update && apt-get install -y \
    emboss=6.6.0-1 \
    embassy-domainatrix=0.1.650-1 \
 && apt-get clean \
 && apt-get purge \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /data
