Introduction {#intro}
============

## Why ObjGL ?  ####

ObjGL (Object Graphics Library) is an open-source C++ library for device and display agnostic graphics rendering. 

The ObjGL API provides a software framework to render graphics for a wide range of 2D and 3D display technologies.  The display agnostic nature of ObjGL implies that there may be many different display types from simple 2D displays to AR/VR, light-field, volumetric, tensor and other complex 3D displays, thus enabling a true heteregenous display environment.

ObjGL abstracts platform or device specific definitions of graphics primitives such as geometry and textures from the underlying GPU or other hardware.  This enables a single definition for all graphics devices and platforms.  ObjGL uses factory design pattern to abstract the device specific implementations from the developer using an abstract base library 'ObjGLFactory'.  Under the hood of the abstract library, derived classes independently implement the machine/platform specific constructs and the application developer is free to choose which Factory(OpenGL 2.1, OpenGL 4.6, DirectX 11, Vulkan and so on) to use. 

### ObjGL Device Agnosticism

@htmlonly <style>div.image img[src="../Images/DeviceAgnosticity.png"]{width:40px;}</style> \endhtmlonly
@image html "../Images/DeviceAgnosticity.png" 

ObjGL operates in a client-server architecture model.  ObjGL host application acts as the server and a component called 'RenExec' acts as the render client.  The host application sits on a server machine that issues cache and render commands to the clients.  The 'RenExec' component receives the cache and render commands and parses them into appropriate ObjGL core elements.  The 'RenExec' is also responsible for establishing the context with the display that it renders to.


## API Concepts

### ObjGL Namespace

All the classes and functions in ObjGL are placed in the namespace ObjGL.  Hence to use any ObjGL functionality, `ObjGL::` specifier or `using namespace ObjGL` must be used.


@ref introArchitecture

@ref introInstrSet



