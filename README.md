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