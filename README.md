# tesseract_builder
This is just a repo for holding the precompiled binaries of tesseract for Windows. These binaries are built using vcpkg tool
`./vcpkg/vcpkg install tesseract:x64-windows`.

### OpenMP build
**Don't follow the steps below. This is just for FYI. Just use the default build**

For building tesseract with OpenMP enabled, do the following one line change to `ports/tesseract/portfile.cmake` file inside the vcpkg repo.

```
C:\work\vcpkg>git diff
diff --git a/ports/tesseract/portfile.cmake b/ports/tesseract/portfile.cmake
index 091a0d26b..080aa728f 100644
--- a/ports/tesseract/portfile.cmake
+++ b/ports/tesseract/portfile.cmake
@@ -30,6 +30,7 @@ vcpkg_cmake_configure(
         -DLeptonica_DIR=YES
         -DSW_BUILD=OFF
         -DLEPT_TIFF_RESULT=ON
+        -DOPENMP_BUILD=ON
     MAYBE_UNUSED_VARIABLES
         CMAKE_DISABLE_FIND_PACKAGE_OpenCL
 )
```

In my experiments, OpenMP enabled version, with `tessedit_parallelize` set `4` uses 2x to 3x more CPU resource for almost same detection speed. Hence decided against using OpenMP enabled version.
