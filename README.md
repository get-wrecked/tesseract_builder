# tesseract_builder
This is just a repo for holding the precompiled binaries of tesseract for Windows. Following is the build process.

## Installing prerequisites

- Install the latest version of cmake
- Create a new folder to place the compiled binaries of tesseract and its dependencies. Hereafterwards we will call this folder as `<target folder>`. For example this could be `C:\work\tesseract_binaries`.
- Here are the pre-requisites that we need to install to the above the `<target folder>`

| Name          | Repo link | version  |
|---------------|-----------|----------|
| zlib          | https://github.com/madler/zlib   | v1.3.1   |
| libpng        | https://github.com/pnggroup/libpng   | v1.6.43   |
| libtiff       | https://gitlab.com/libtiff/libtiff.git   | v4.6.0   |
| libjpeg-turbo | https://github.com/libjpeg-turbo/libjpeg-turbo.git  | 3.0.3   |

Compile and install the above dependencies by the following the below steps for each of them seperately. Replace the <Repo link>, <Name> and <version> with the values from the above table

```
git clone <Repo link>
cd <Name>
git checkout <version>
mkdir build64
cd build64
cmake -DCMAKE_INSTALL_PREFIX=<target folder> ..
cmake --build . --config RelWithDebInfo
cmake --install . --config RelWithDebInfo
```

- Add the path `<target folder>\bin` to the PATH environment variable. Open a new command prompt, follow rest of the steps.
- Now checkout and build leptonica, which depends on the above libraries

```
git clone https://github.com/danbloomberg/leptonica
cd leptonica
git checkout 1.84.1
mkdir build
cd build
cmake -DENABLE_GIF=OFF -DENABLE_JPEG=ON -DSW_BUILD=OFF -DENABLE_TIFF=ON -DENABLE_WEBP=OFF -DENABLE_OPENJPEG=OFF -DSTRICT_CONF=ON -DENABLE_PNG=ON -DCMAKE_INSTALL_PREFIX=<target folder> ..
cmake --build . --config RelWithDebInfo
cmake --install . --config RelWithDebInfo
```

- Now checkout and build tesseract, which depends on the above leptonica library

```
git clone https://github.com/get-wrecked/tesseract
cd tesseract
git checkout lstm_multi_thread
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=<target folder> -DSW_BUILD=OFF -DBUILD_TRAINING_TOOLS=OFF -DDISABLE_ARCHIVE=ON -DDISABLE_CURL=ON -DBUILD_SHARED_LIBS=ON ..
cmake --build . --config RelWithDebInfo
cmake --install . --config RelWithDebInfo
```

- Inside the target folder create a folder named `tessdata` and copy this file into it. https://github.com/tesseract-ocr/tessdata_fast/blob/main/eng.traineddata
- Inside the target folder create a folder name `pdb` and copy the following files into it. You can find the below files in one of the subfolders(usually named `RelWithDebInfo`) of the `build` or `build64` folder of the corresponding repos.
```
jpeg62.pdb
libpng16.pdb
tesseract54.pdb
tiff.pdb
turbojpeg.pdb
zlib.pdb
```

- Now zip the contents of the target folder to create a pre-built binary package and upload it as a new release to this repo.
