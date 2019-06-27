#===============================================================================
#
#         FILE: Dockerfile
#    DEV USAGE: docker run -it -v /Users/Alec/Documents/Bioinformatics/MDV_Project/p0100_music/data:/mount_data --name music1 --rm ubuntu:latest
#        USAGE: docker image build -t genome_music . # local image build
#
#  DESCRIPTION:  This Dockerfile will build MuSiC2 (Mutational Significance in Cancer version 2)
# REQUIREMENTS:  ---
#        NOTES:  MuSiC2 Github: https://github.com/ding-lab/MuSiC2
#                joinx Github: https://github.com/genome/joinx
#                calcRoiCovg Github: https://github.com/Beifang/calcRoiCovg
#       AUTHOR:  Alec Steep, alec.steep@gmail.com
#  AFFILIATION:  Michigan State University (MSU), East Lansing, MI, United States
#				         USDA ARS Avian Disease and Oncology Lab (ADOL), East Lansing, MI, United States
#				         Technical University of Munich (TUM), Weihenstephan, Germany
#      VERSION:  1.0
#      CREATED:  2019.06.26
#     REVISION:  ---
#===============================================================================

FROM ubuntu:latest
# Pull the Ubuntu OS image

RUN apt-get update \
    && apt-get install -y sudo \
    && sudo apt-get install -y build-essential \
    git \
    cmake \
    curl \
    cpanminus \
    libbz2-dev \
    libgtest-dev \
    libbam-dev \
    zlib1g-dev \
    wget \
    libncurses-dev \
    liblzma-dev
# Update the apt-get in the Ubuntu image
# Install the MuSiC2 dependencies

RUN mkdir apps \
    && cd apps \
    && wget https://sourceforge.net/projects/samtools/files/samtools/0.1.19/samtools-0.1.19.tar.bz2 \
    && tar jxf samtools-0.1.19.tar.bz2 \
    && rm samtools-0.1.19.tar.bz2 \
    && cd samtools-0.1.19 \
    && make \
    && export SAMTOOLS_DIR=$PWD \
    && export SAMTOOLS_ROOT=$PWD \
    && sudo mv samtools /usr/local/bin/ \
    && cd ..
# Install Samtools and place in bin

RUN git clone https://github.com/Beifang/calcRoiCovg.git \
    && cd calcRoiCovg \
    && make \
    && sudo mv calcRoiCovg /usr/local/bin/ \
    && cd ..
# Install calcRoiCovg

RUN wget https://github.com/arq5x/bedtools2/archive/v2.27.1.tar.gz \
    && tar -zxvf v2.27.1.tar.gz \
    && rm v2.27.1.tar.gz \
    && cd bedtools2-2.27.1 \
    && make \
    && sudo mv ./bin/* /usr/local/bin/ \
    && cd ..
# Install bedtools

RUN git clone --recursive https://github.com/genome/joinx.git \
    && cd joinx \
    && mkdir build \
    && cd build \
    && cmake .. \
    && make deps \
    && make \
    && sudo make install \
    && cd ../..
# Install joinx


bool StreamLineSource::getline(std::string& line) {
    std::getline(_in, line);
    return true;
}
# Need to add code that will automatically update joinx documents
# Recommended from: https://github.com/genome/joinx/commit/ba66b5cd790d1798ce797a934df0483330429aac

RUN sudo cpanm Test::Most \
    && sudo cpanm Statistics::Descriptive \
    && sudo cpanm Statistics::Distributions \
    && sudo cpanm Bit::Vector
# Install perl modules

RUN git clone https://github.com/ding-lab/MuSiC2 \
    && cd MuSiC2 \
    && sudo cpanm MuSiC2-0.2.tar.gz
# Install MuSiC2



# smg test example
# Download hg19 for the test example
mkdir reference
wget http://hgdownload.cse.ucsc.edu/goldenPath/hg19/bigZips/hg19.fa.gz ./reference

# Make the directory
mkdir music2_smg_running
cd music2_smg_running
mkdir logs
mkdir logs/calc_covg

music2 bmr calc-covg \
--roi-file ../example/smg/example.roi_file \
--reference-sequence ../reference_dir/ucsc.hg19.fa \
--bam-list ./example/smg/example.bam_list \
--output-dir . \
--cmd-list-file example.run-coverage-command

bash example.run-coverage-command

music2 bmr calc-covg \
--roi-file ./example/smg/example.roi_file \
--reference-sequence /reference_dir/ucsc.hg19.fa \
--bam-list ./example/smg/example.bam_list \
--output-dir .

music2 bmr calc-bmr \
--roi-file ./example/smg/example.roi_file \
--reference-sequence /reference_dir/ucsc.hg19.fa \
--bam-list ./example/smg/example.bam_list \
--maf-file ./example/smg/example.input.maf \
--output-dir . \
--show-skipped

music2 smg \
--gene-mr-file gene_mrs \
--output-file smgs \
--max-fdr 0.05 \
--processors 1