#===============================================================================
#
#         FILE: Dockerfile
#    DEV USAGE: docker run -it -v /Users/Alec/Documents/Bioinformatics/MDV_Project/p0100_music/data:/mount_data --name music1 --rm ubuntu:latest
#        USAGE: docker image build -t genome_music . # local image build
#
#  DESCRIPTION:  This Dockerfile will build MuSiC2 (Mutational Significance in Cancer version 2)
# REQUIREMENTS:  ---
#        NOTES:  MuSiC2 Github: https://github.com/ding-lab/MuSiC2
#                MuSiC resources: https://www.biostars.org/p/62793/
#                joinx Github: https://github.com/genome/joinx
#                calcRoiCovg Github: https://github.com/Beifang/calcRoiCovg
#       AUTHOR:  Alec Steep, alec.steep@gmail.com
#  AFFILIATION:  Michigan State University (MSU), East Lansing, MI, United States
#				         USDA ARS Avian Disease and Oncology Lab (ADOL), East Lansing, MI, United States
#				         Technical University of Munich (TUM), Weihenstephan, Germany
#      VERSION:  1.1
#      CREATED:  2019.06.26
#     REVISION:  2019.07.16
#===============================================================================

# Hardcode this version
FROM ubuntu:18.04
# Pull the Ubuntu OS image

# EDIT: Hardcode all these versions 
RUN apt-get update \
    && apt-get install -y sudo=1.8.21p2-3ubuntu1 \
    && sudo apt-get install -y build-essential=12.4ubuntu1 \
    git=1:2.17.1-1ubuntu0.4 \
    cmake \
    curl=7.58.0-2ubuntu3.7 \
    cpanminus=1.7043-1 \
    libbz2-dev \
    libgtest-dev=1.8.0-6 \
    libbam-dev=0.1.19-2 \
    zlib1g-dev=1:1.2.11.dfsg-0ubuntu2 \
    wget=1.19.4-1ubuntu2.2 \
    libncurses-dev \
    liblzma-dev=5.2.2-1.3 \
    vim=2:8.0.1453-1ubuntu1.1 \
    r-base:3.4.4-1ubuntu1
# Update the apt-get in the Ubuntu image
# Install the MuSiC2 dependencies
# Note: use 'apt-cache policy <package-name>' to determine which package is installed

RUN mkdir apps \
    && cd apps \
    && wget https://sourceforge.net/projects/samtools/files/samtools/0.1.19/samtools-0.1.19.tar.bz2 \
    && tar jxf samtools-0.1.19.tar.bz2 \
    && rm samtools-0.1.19.tar.bz2 \
    && cd samtools-0.1.19 \
    && make \
    && export SAMTOOLS_ROOT="/apps/samtools-0.1.19" \
    && sudo cp samtools /usr/local/bin/
# Install Samtools and place in bin

ENV SAMTOOLS_ROOT /apps/samtools-0.1.19
# Set environmental variable for calcRoiCovg

WORKDIR /apps
# Change back to app directory

RUN git clone https://github.com/Beifang/calcRoiCovg.git \
    && cd calcRoiCovg \
    && make \
    && sudo mv calcRoiCovg /usr/local/bin/
# Install calcRoiCovg

WORKDIR /apps
# Change back to app directory

RUN wget https://github.com/arq5x/bedtools2/archive/v2.27.1.tar.gz \
    && tar -zxvf v2.27.1.tar.gz \
    && rm v2.27.1.tar.gz \
    && cd bedtools2-2.27.1 \
    && make \
    && sudo mv ./bin/* /usr/local/bin/
# Install bedtools

WORKDIR /apps
# Change back to app directory

RUN git clone --recursive https://github.com/genome/joinx.git \
    && rm /apps/joinx/src/lib/io/InputStream.cpp \
    && rm /apps/joinx/src/lib/io/InputStream.hpp \
    && rm /apps/joinx/src/lib/io/StreamLineSource.cpp \
    && rm /apps/joinx/test/lib/fileformats/TestVcfEntry.cpp \
    && cd /usr/local \
    && git clone https://github.com/steepale/Docker_MuSiC2.git \
    && mv /usr/local/Docker_MuSiC2/adjusted_files/InputStream.cpp /apps/joinx/src/lib/io/InputStream.cpp \
    && mv /usr/local/Docker_MuSiC2/adjusted_files/InputStream.hpp /apps/joinx/src/lib/io/InputStream.hpp \
    && mv /usr/local/Docker_MuSiC2/adjusted_files/StreamLineSource.cpp /apps/joinx/src/lib/io/StreamLineSource.cpp \
    && mv /usr/local/Docker_MuSiC2/adjusted_files/TestVcfEntry.cpp /apps/joinx/test/lib/fileformats/TestVcfEntry.cpp \
    && rm -r /usr/local/Docker_MuSiC2 \
    && cd /apps/joinx \
    && mkdir build \
    && cd build \
    && cmake .. \
    && make deps \
    && make \
    && sudo make install
# Install joinx: remove problematic files (at least on Ubuntu), clone github directory, copy in adjusted files, and install joinx.
#  Recommendation from: https://github.com/genome/joinx/commit/ba66b5cd790d1798ce797a934df0483330429aac

WORKDIR /apps
# Change back to app directory

RUN sudo cpanm Test::Most@0.35 \
    && sudo cpanm Statistics::Descriptive@3.0702 \
    && sudo cpanm Statistics::Distributions@1.02 \
    && sudo cpanm Bit::Vector@7.4
# Install perl modules
# Note: determine version with cpan -D <module-name>

RUN git clone https://github.com/ding-lab/MuSiC2 \
    && cd MuSiC2 \
    && sudo cpanm MuSiC2-0.2.tar.gz \
    && rm /apps/MuSiC2/MuSiC2-0.2.tar.gz
# Install MuSiC2 and log build stdout and stderr

RUN mkdir /var/log/music2 \
    && touch /var/log/music2/access.log \
    && touch /var/log/music2/error.log \
    && ln -sf /dev/stdout /var/log/music2/access.log \
	&& ln -sf /dev/stderr /var/log/music2/error.log
# forward request and error logs to docker log collector (from nginx official image)

WORKDIR /

CMD ["/bin/bash"]
# This is the CMD command from the Ubuntu:18.04 image we FROM'ed