# PGR framework

This a CMake port of the framework used in the BI-PGR course taught on CTU FIT.

# Dependencies

This framework depends on the following libraries:

 - [DevIL](http://openil.sourceforge.net)
 - [AntTweakBar](http://anttweakbar.sourceforge.net/doc/)
 - [Assimp](http://www.assimp.org)
 - [FreeGLUT](http://freeglut.sourceforge.net)

It attempts to ship their Linux binaries, but they're definitely not guaranteed to work. If they don't, install the libraries using you distributions package manager and change the following line in `CMakeLists.txt`

```cmake
set(USE_PACKAGE_MANAGER OFF)
```

to instead say

```cmake
set(USE_PACKAGE_MANAGER ON)
```

Note that on some (especially Debian-based) distributions, the package manager might fail to supply appropriate CMake modules.

Those will be added to this repository shortly, or you can hunt them down online.

# macOS

On macOS, the native GLUT implementation supplied by Apple is used instead of FreeGLUT, because FreeGLUT is dependent on X11. This means you might need to do some changes.

The inclusion of platform-specific GLUT headers should be taken care of by the `pgr.h` header file.

To get a Core OpenGL context, surround the FreeGLUT context initialization with an `ifdef` and use the Apple-specific `GLUT_3_2_CORE_PROFILE` display flag.

```c
#ifndef __APPLE__
  glutInitContextVersion(pgr::OGL_VER_MAJOR, pgr::OGL_VER_MINOR);
  glutInitContextFlags(GLUT_FORWARD_COMPATIBLE);

  glutInitDisplayMode(GLUT_RGB | GLUT_DOUBLE | GLUT_DEPTH);
#else
  glutInitDisplayMode(GLUT_3_2_CORE_PROFILE | GLUT_RGB | GLUT_DOUBLE | GLUT_DEPTH);
#endif
```

Similarly, any calls to `glutLeaveMainLoop` should be replaced by

```c
#ifndef __APPLE__
  glutLeaveMainLoop();
#else
  exit(0);
#endif
```

# Issues

In case of any issues, please create an issue or contact me on <sebelji1@fit.cvut.cz>.
