#################################################################
# Dockerfile
# Software:         miRdup
# Software Version: 1.4
# Description:      Computational prediction of the localization of microRNAs within their pre-miRNA
# Website:          http://cs.mcgill.ca/~blanchem/mirdup/
# Tags:             Proteomics|Genomics|General
# Provides:         miRdup V1.4
# WARNING :         YOUR.model needs to be in /miRdup/YOUR.model or it will not work
#                   Use the sharing folder /data and cp command to transfert the model before using miRdup.
# Use :             java -jar miRdup.jar -predict -i ./testFiles/prediction.infile.fasta -d all19.model -f /data/results/outfile -r ./ViennaRNA/bin/
#################################################################

# Base docker image
FROM ubuntu:latest
MAINTAINER test@armadillo.uqam

# Install miRdup and dependencies
RUN apt-get clean 

# Prepare installation
RUN apt-get --quiet update >> ~/install.log \
 && apt-get -qqy install \
    wget \
    build-essential \
    unzip \
    default-jre \
    default-jdk \
 && apt-get clean \
 && apt-get purge \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Get miRdup
RUN cd ~ \
 && wget --quiet http://cs.mcgill.ca/~blanchem/mirdup/miRdup_1.4.zip -O miRdup_1.4.zip \
 && mkdir /miRdup \
 && unzip -qq miRdup_1.4.zip -d /miRdup \
 && rm -f miRdup_1.4.zip

# Get vienna
RUN cd /miRdup \
 && wget --quiet http://www.tbi.univie.ac.at/RNA/download.php?id=viennarna-2.2.0 -O ViennaRNA-2.2.0.tar.gz \
 && tar -xf ViennaRNA-2.2.0.tar.gz >> ~/zip.log \
 && cd ViennaRNA-2.2.0 \
 && ./configure --quiet --prefix=/miRdup/ViennaRNA --without-swig --without-perl --without-python \
 && make --quiet \
 && make install --quiet \
 && cd .. \
 && rm -Rf ./ViennaRNA-2.2.0 \
 && rm -f ViennaRNA-2.2.0.tar.gz

RUN chmod -R 777 /miRdup

WORKDIR /data
