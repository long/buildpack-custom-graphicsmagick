## Configuring a Heroku App

In order to use this buildpack in addition to a standard buildpack such
as node, we need to use the multi buildpack

### New App

    heroku apps:create build-gm -buildpack https://github.com/ddollar/heroku-buildpack-multi.git

### Update an Existing App

    heroku config:set BUILDPACK_URL=https://github.com/ddollar/heroku-buildpack-multi.git

### Buildpack configuration file

A buildpack configuration file .buildpacks needs to be added to the project

.buildpacks

    https://github.com/long/buildpack-custom-graphicsmagick.git
    https://github.com/heroku/heroku-buildpack-nodejs.git

## Build

Libraries and executables for GraphicsMagick and supporting libraries
are built using the scripts located under the **build/scripts** directory.

Each build script is self-contained and are run separately to build the
necessary library components.

The build results are installed in a clean directory and packaged up into
an archive that is stored in S3 and will be used for the custom buildpack
installation.

### Build Source

Build source is located in S3

s3://buildpacks.rayburst.com/custom-graphicsmagick/src

### Build Resutls

Build results are located in S3

s3://buildpacks.rayburst.com/custom-graphicsmagick/vendor

### Build Order

1. zlib
2. libpng  
   requires zlib 
3. tiff  
   requires zlib 
4. yasm
5. libjpeg-turbo  
   requires yasm, zlib
6. lcms2  
   requires libjpeg-turbo, tiff, zlib
7. gm  
   requries libpng, tiff, libjpeg-turbo, lcms2, zlib

### Versions

zlib 1.2.8  
libpng 1.5.17  
tiff 4.0.3  
yasm 1.2.0  
libjpeg-turbo 1.3.1  
lcms2 2.4  
GraphicsMagick 1.3.18

# Building on Heroku

This project should be installed on Heroku using the null buildpack in order 
to pre-build the libraries and executables

    heroku apps:create build-gm -buildpack http://github.com/ryandotsmith/null-buildpack.git
    heroku run bash

    build/build_all /tmp/vendor_build /tmp/vendor \
    https://s3.amazonaws.com/buildpacks.rayburst.com/custom-graphicsmagick/src \
    buildpacks.rayburst.com custom-graphicsmagick/vendor \
    [AWS KEY] [AWS SECRET]


