# [https://github.com/steepale/Docker_MuSiC2](https://github.com/steepale/Docker_MuSiC2)

## Image created by: [Alec Steep](https://github.com/steepale)

This is a Git repo for a dockerized MuSiC2 image. The image has been designed to be reproducible and user friendly. I am an independent bioinformatician, who created this image to analyze somatic mutation hotspots in Marek's disease lymphomas. See the [Docker Hub page](https://cloud.docker.com/repository/docker/steepale/music2_20190708_v1.0) for instructions on how to use this image.

## Image Info

This image uses latest Ubuntu image and installs dependencies for [MuSiC2](https://github.com/ding-lab/MuSiC2), some of which were adjusted for proper installation. For instance, [files for joinx installation](https://github.com/steepale/Docker_MuSiC2/tree/master/adjusted_files) were altered based on [this joinx issue](https://github.com/genome/joinx/commit/ba66b5cd790d1798ce797a934df0483330429aac).
These include:
    -InputStream.cpp
    -InputStream.hpp
    -StreamLineSource.cpp
    -TestVcfEntry.cpp



