#################################################################
# Dockerfile
# Software:         miRdup
# Software Version: 1.4
# Description:      Computational prediction of the localization of microRNAs within their pre-miRNA
# Website:          http://cs.mcgill.ca/~blanchem/mirdup/
# Tags:             Proteomics|Genomics|General
# Provides:         miRdup V1.4
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

# Get Models
RUN mkdir /miRdup/models \
 && cd /miRdup/models \
 && wget --quiet http://www.cs.mcgill.ca/~blanchem/mirdup/model_mirbase.zip -O model_mirbase19.zip \
 && unzip -qq model_mirbase19.zip -d /miRdup/models/ \
 && cp all.model all19.model \
 && rm -f all.model model_mirbase19.zip

RUN cd /miRdup/models \
 && wget --quiet http://www.cs.mcgill.ca/~blanchem/mirdup/model_mammals.zip -O model_mammals.zip \
 && unzip -qq  model_mammals.zip -d /miRdup/models/ \
 && cp mammal.model mammal19.model \
 && rm -f mammal.model model_mammals.zip

RUN cd /miRdup/models \
 && wget --quiet http://www.cs.mcgill.ca/~blanchem/mirdup/model_fishes.zip -O model_fishes.zip \
 && unzip -qq model_fishes.zip -d /miRdup/models/ \
 && cp pisces.model fishes19.model \
 && rm -f pisces.model model_fishes.zip

RUN cd /miRdup/models \
 && wget --quiet http://www.cs.mcgill.ca/~blanchem/mirdup/model_mammals.zip -O model_mammals.zip \
 && unzip -qq model_mammals.zip -d /miRdup/models/ \
 && cp mammal.model mammal19.model \
 && rm -f mammal.model model_mammals.zip

RUN cd /miRdup/models \
 && wget --quiet http://www.cs.mcgill.ca/~blanchem/mirdup/model_nematods.zip -O model_nematods.zip \
 && unzip -qq model_nematods.zip -d /miRdup/models/ \
 && cp nematod.model nematod19.model \
 && rm -f nematod.model model_nematods.zip

RUN cd /miRdup/models \
 && wget --quiet http://www.cs.mcgill.ca/~blanchem/mirdup/model_arthropods.zip -O model_arthropods.zip \
 && unzip -qq model_arthropods.zip -d /miRdup/models/ \
 && cp arthropod.model arthropod19.model \
 && rm -f arthropod.model model_arthropods.zip

RUN cd /miRdup/models \
 && wget --quiet http://www.cs.mcgill.ca/~blanchem/mirdup/model_plants.zip -O model_plants.zip \
 && unzip -qq  model_plants.zip -d /miRdup/models/ \
 && cp plant.model plant19.model \
 && rm -f plant.model model_plants.zip

RUN cd /miRdup/models \
 && wget http://www.cs.mcgill.ca/~blanchem/mirdup/model_mirbase21.zip -O model_mirbase21.zip \
 && unzip -qq model_mirbase21.zip -d /miRdup/models/ \
 && cp all.model all21.model \
 && cp all.modelOutput all21.modelOutput \
 && rm -f all.model all.modelOutput model_mirbase21.zip
 
RUN chmod -R 777 /miRdup

WORKDIR /data
