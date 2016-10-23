#################################################################
# Dockerfile
# Software:         miRcheck
# Software Version: 
# Description:      miRcheck and various perl scripts useful in identfying plant miRNA genes.
# Website:          http://bartellab.wi.mit.edu/softwareDocs/
# Files:            http://bartellab.wi.mit.edu/softwareDocs/miRcheck.tar
# Tags:             Proteomics|Genomics|General
# Provides:         miRcheck
#
# Run :             perl ~/script_you_need [options]
# Run :             
# If needed, copy new models in model folder
# Copy:             
# Modifications:    Few files have been modified during installation to add option or to update files with current OS.
#################################################################

# Base docker image
FROM ubuntu:14.04
MAINTAINER test@armadillo.uqam
ARG DEBIAN_FRONTEND=noninteractive

# Install miRcheck and dependencies
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

# Update miRcheck lib localization
RUN cd ~/miRcheck \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' evaluate_miRNA_candidates.pl \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' extract_einverted_20mers.pl \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' fold_inverted_repeats.pl \
 && sed -i 's/\/home2\/mjonesrh\/_miRNA_search\/_scripts\///g' fold_inverted_repeats.pl \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' retrieve_genomic_regions.pl \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' run_einverted.pl \
 && sed -i 's/\/home2\/mjonesrh\/exe\/linuxexe\/RNAfold_original/RNAfold/g' miRcheck.pm \
 && sed -i 's/\/home2\/mjonesrh/\/root/g' test_mircheck.pl

# Add options in extract_einverted_20mers.pl
RUN cd ~/miRcheck \
  && sed -i '26i \
while (@ARGV){\n\
    $thisarg = shift @ARGV;\n\
    if ($thisarg eq "-unpair" ) {$MAX_UNPAIR=shift @ARGV;} \n\
    if ($thisarg eq "-star_unpair" ) {$MAX_STAR_UNPAIR=shift @ARGV;} \n\
    if ($thisarg eq "-size_diff" ) {$MAX_SIZEDIFFERENCE=shift @ARGV;} \n\
    if ($thisarg eq "-mir_bulge") {$MAX_MIR_GAP=shift @ARGV;} \n\
    if ($thisarg eq "-star_bulge") {$MAX_STAR_GAP=shift @ARGV;} \n\
    if ($thisarg eq "-fback_min") {$MIN_FBACK_SIZE=shift @ARGV;} \n\
    if ($thisarg eq "-ass") {$MAX_MIR_AS_BULGE=shift @ARGV;} \n\
    if ($thisarg eq "-min_unpair") {$MIN_UNPAIR=shift @ARGV;} \n\
    if ($thisarg eq "-bp_ext") {$BP_EXTENSION=shift @ARGV;} \n\
    if ($thisarg eq "-max_one_nt") {$MAX_ONE_NT=shift @ARGV;} \n\
    if ($thisarg eq "-max_two_nt") {$MAX_TWO_NT=shift @ARGV;} \n\
    if ($thisarg eq "-block") {$BLOCK =shift @ARGV;} \n\
} \n\n\
'  extract_einverted_20mers.pl 

# Add options in fold_inverted_repeats.pl
RUN cd ~/miRcheck \
  && sed -i '14i \
while (@ARGV){\n\
    $thisarg = shift @ARGV;\n\
    if ($thisarg eq "-flanking_nt" ) {$FLANKING_NT=shift @ARGV;} \n\
} \n\n\
'  fold_inverted_repeats.pl

# Add options in run_einverted.pl
RUN cd ~/miRcheck \
  && sed -i '28i \
\n\
while (@ARGV){\n\
    $thisarg = shift @ARGV;\n\
    if ($thisarg eq "-win" ) {$WIN=shift @ARGV;} \n\
    if ($thisarg eq "-step" ) {$step=shift @ARGV;} \n\
    if ($thisarg eq "-min_pct" ) {$MIN_PCT=shift @ARGV;} \n\
    if ($thisarg eq "-min_arm") {$MIN_ARM=shift @ARGV;} \n\
    if ($thisarg eq "-gap") {$GAP=shift @ARGV;} \n\
    if ($thisarg eq "-tresh") {$THRESH=shift @ARGV;} \n\
    if ($thisarg eq "-m") {$M=shift @ARGV;} \n\
    if ($thisarg eq "-mm") {$MM=shift @ARGV;} \n\
    if ($thisarg eq "-dist") {$DIST=shift @ARGV;} \n\
} \n\n\
'  run_einverted.pl

# Add options in run_patscan.pl
RUN cd ~/miRcheck \
  && sed -i".bak" '8,10d' run_patscan.pl \
  && sed -i 's/ARGV\[5\]/ARGV\[2\]/' run_patscan.pl \
  && sed -i".bak" '10d' run_patscan.pl \
  && sed -i '10i \
$mismatches = 0; \n\
$deletions  = 0; \n\
$insertions = 0; \n\
\n\
while (@ARGV){\n\
    $thisarg = shift @ARGV;\n\
    if ($thisarg eq "-mismatches" ) {$mismatches=shift @ARGV;} \n\
    if ($thisarg eq "-deletions" )  {$deletions =shift @ARGV;} \n\
    if ($thisarg eq "-insertions" ) {$insertions=shift @ARGV;} \n\
} \n\n\
if (not($outfile)){print "usage: simple_patscan.pl query.fa database.fa outfile -mismatches 0 -deletions 0 -insertions 0 \\n";exit;} \n\
\n\
'  run_patscan.pl

# Create rep
RUN mkdir ~/data

RUN cd ~ \
 && chmod -R 777 ./miRcheck

WORKDIR /root/data
