#################################################################
# Dockerfile
# Software:         miRcheck
# Software Version: 
# Description:      Computational prediction of the localization of microRNAs within their pre-miRNA
# Website:          http://cs.mcgill.ca/~blanchem/mirdup/
# Tags:             Proteomics|Genomics|General
# Provides:         miRdup V1.4
#
# Run :             java -Xms500m -Xmx3500m -jar miRdup.jar -k all -r /usr/bin/
# Run :             java -jar miRdup.jar -predict -i [PATH/TO/INPUT]/infile.fasta -d [PATH/TO/MODEL]/model.model -f [PATH/TO/OUTPUT/REP]/outfile -r /usr/bin/
# If needed, copy new models in model folder
# Copy:             find /miRdup/ -maxdepth 1 -type f -name '*.model' -exec cp {} [PATH/TO/MODEL] \;
#################################################################

# Base docker image
FROM ubuntu:14.04
MAINTAINER test@armadillo.uqam
ARG DEBIAN_FRONTEND=noninteractive

# Install miRdup and dependencies
RUN apt-get clean 

# Prepare installation
RUN apt-get -qq update \
 && apt-get -qqy install \
    --no-install-recommends \
    software-properties-common \
    wget \
    g++ \
    emboss=6.6.0-1 \
 && apt-get clean \
 && apt-get purge \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install vienna for fold_inverted_repeats.pl
RUN apt-add-repository -y ppa:j-4/vienna-rna  > /dev/null 2>&1 \
 && apt-get -qq update \
 && apt-get -qqy install \
    vienna-rna \
 && apt-get clean \
 && apt-get purge \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Get miRcheck
RUN cd ~ \
 && wget --quiet http://bartellab.wi.mit.edu/softwareDocs/miRcheck.tar \
 && tar -xf miRcheck.tar \
 && mv ./_miRcheck_scripts ./miRcheck \
 && rm -f miRcheck.tar

# Install scan_for_matches
RUN cd ~ \
 && wget --quiet http://www.theseed.org/servers/downloads/scan_for_matches.tgz \
 && tar -xzf scan_for_matches.tgz \
 && cd scan_for_matches \
 && gcc -O -o scan_for_matches  ggpunit.c scan_for_matches.c \
 && chmod +x  scan_for_matches \
 && chmod 777  scan_for_matches \
 && ln -sv scan_for_matches /usr/bin/scan_for_matches \
 && rm -f ../scan_for_matches.tgz
 
USER root
ENV PATH="$PATH:/root/scan_for_matches"

# Change miRcheck lib localization
RUN cd ~/miRcheck \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' evaluate_miRNA_candidates.pl \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' extract_einverted_20mers.pl \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' fold_inverted_repeats.pl \
 && sed -i 's/\/home2\/mjonesrh\/_miRNA_search\/_scripts\///g' fold_inverted_repeats.pl \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' retrieve_genomic_regions.pl \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' run_einverted.pl \
 && sed -i 's/\/home2\/mjonesrh\/exe\/linuxexe\/RNAfold_original/RNAfold/g' miRcheck.pm \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' test_mircheck.pl

# Add options in perl files
RUN cd ~/miRcheck \
  && sed -i '26i \
while (@ARGV){\n \
    $thisarg = shift @ARGV;\n \
    if ($thisarg eq "-unpair" ) {$MAX_UNPAIR=shift @ARGV;}\n \
    if ($thisarg eq "-star_unpair" ) {$MAX_STAR_UNPAIR=shift @ARGV;}\n \
    if ($thisarg eq "-size_diff" ) {$MAX_SIZEDIFFERENCE=shift @ARGV;}\n \
    if ($thisarg eq "-mir_bulge") {$MAX_MIR_GAP=shift @ARGV;}\n \
    if ($thisarg eq "-star_bulge") {$MAX_STAR_GAP=shift @ARGV;}\n \
    if ($thisarg eq "-fback_min") {$MIN_FBACK_SIZE=shift @ARGV;}\n \
    if ($thisarg eq "-ass") {$MAX_MIR_AS_BULGE=shift @ARGV;}\n \
    if ($thisarg eq "-min_unpair") {$MIN_UNPAIR=shift @ARGV;}\n \
    if ($thisarg eq "-bp_ext") {$BP_EXTENSION=shift @ARGV;}\n \
    if ($thisarg eq "-max_one_nt") {$MAX_ONE_NT=shift @ARGV;}\n \
    if ($thisarg eq "-max_two_nt") {$MAX_TWO_NT=shift @ARGV;}\n \
}\n\n \
'  extract_einverted_20mers.pl 

# Create rep
RUN mkdir ~/data

RUN cd ~ \
 && chmod -R 777 ./miRcheck

WORKDIR ~/data
