# JustTest
Only for my test

# ObjGL: Object Graphics Library

ObjGL (Object Graphics Library) is an open-source C++ library for device and display agnostic graphics rendering. The API provides a software framework to render graphics for a wide range of 2D and 3D display technologies.  

## Resources

* Homepage: <http://fovi3d.com>

## Building

### Required Dependencies
* <a href="https://cmake.org/">CMake (3.8.2 or newer)</a>
* <a href="http://www.doxygen.org/">Doxygen (1.8.14) </a> (Optional)
* <a href="https://github.com/muflihun/easyloggingpp">Easylogging++ (9.89)</a>
* <a href="https://github.com/glfw/glfw">GLFW (3.2.1)</a>
* <a href="https://github.com/g-truc/glm">GLM (0.9.8.5)</a>
* <a href="https://github.com/zeromq/libzmq">ZeroMQ (4.2.3)</a>
  * <a href="https://github.com/zeromq/cppzmq">cppzmq (4.2.3)</a>
* <a href="https://github.com/opencv/opencv">OpenCV (3.4.3)</a>
* <a href="https://github.com/nigels-com/glew">GLEW (2.1.0)</a> (Windows only)
* <a href="https://developer.microsoft.com/en-us/windows/downloads/windows-10-sdk">Windows 10 SDK (10.0.17134.0 or newer)</a> (Windows only)

### Options
The following options can be used to configure a specific build using CMake.
* BUILD_DOCUMENTATION (Default OFF): Build documentation using Doxygen. (Note: Doxygen is included as a project in the solution file of Visual Studio. Before building the INSTALL project, the Doxygen project must be built first.)

## Contributing

Please read the [contribution guidelines](http://fovi3d.com) before starting work on a pull request.
