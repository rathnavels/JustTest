Tutorial - ObjGL Host {#tutorial01host}
============================


This is a simple ObjGL host application tutorial!  This creates an ObjGL context, caches a triangle, texture, and material,
and renders it onto a GLFW window.  Please refer the <B> Tutorial01Host </B> project in the SDK as you follow this tutorial.

The class Triangle and member functions used in this tutorial can be found here - @ref tutorial01h.

<hr>

## ObjGL Factory and Server Configuration

Each host application must have an ObjGL::Interface component which is the fundamental entity that manages the host rendering and client messaging system.

@code{.cpp}
ObjGL::Interface         objGL; 
@endcode

In a generic ObjGL host application, we begin with configuring the ObjGL factory, which sets the rendering device/version for the host.  This is usually done through a .json config file which is passed on as a command line argument.

### A Sample Host Configuration JSON File

@code{.json}
{
  "Server": {
    "Address": "*",
    "Port": "5557"
  },
  "OpenGL": {
    "Version":"3.3",
	"Pipeline": "fixed",
  }
}
@endcode

As it can be seen from the above .json config file, the host computer's server configuration as well can be set with the configure method.

@code{.cpp}
objGL.configure(cfp);
@endcode

The argument 'cfp' points to the location of the config file.  Based on the factory and server information provided in the config file, appropriate initialization parameters are set.  

<hr>

## Initialization

Following the configuration, the ObjGL::Factory and ObjGL::Server must be initialized.  This is done by

@code{.cpp}
objGL.init();
@endcode

Under the hood, this initializes the choosen factory and the server, also sets in motion the server component in a new thread.

<hr>

## Enabling Forwarding

Afer initialization and before actual caching of any scene components, ObjGL::Forwarding must be enabled.  This makes sure all the caching or rendering operation done by the host application, triggers a server action that would compose a cache or render message to be broadcasted to the clients.  

@code{.cpp}
objGL.enable(ObjGL::Forwarding);
@endcode

This enables the ObjGL::Forwarding flag, so the clients can receive the appropriate messages containing the commands which the client in turn uses to cache or render.  This flag can also be disabled by calling a disable() method which can come handy when the host developer wants to cache or render a scene object only in host application but not in client application. 

<hr>

## Caching

Following the configuration and intialization, we can set out to cache different ObjGL components.  This includes caching the View, Volume, VertexLst, Light, Texture, and Material etc.


@code{.cpp}
pTriangle->cacheView();
pTriangle->cacheVolume();
pTriangle->cacheLight();
pTriangle->cacheTexture();
pTriangle->cacheMaterial();
pTriangle->cacheGeometry();
@endcode

In each of these functions defined in the Triangle class, a core ObjGL component is defined and then cached.  On caching, a pointer handle to the cached component is returned which is then used in render instructions to point to the appropriate cached components.

The following code snippet illustrates how an ObjGL::View component is cached and an ObjGL::ViewObj handle is returned.

@code{.cpp}
{
glm::vec4   vR = glm::vec4(1.0f, 0.0f, 0.0f, 0.0f);
glm::vec4   vU = glm::vec4(0.0f, 1.0f, 0.0f, 0.0f);
glm::vec4   vD = glm::vec4(0.0f, 0.0f, -1.0f, 0.0f);
glm::vec4   vP = glm::vec4(0.0f, 0.0f, 3.5f, 1.0f);

glm::mat4   mP = glm::perspective(glm::radians(45.0f), 1.0f, 0.001f, 5.00f);

  _view = ObjGL::View(mP, glm::mat4(vR, vU, vD, vP));
}

_viewObj = objGL.cache(_view);
@endcode

Similarly for every other component, a cache call returns a handle to the cached component.

@code{.cpp}
_volumeObj    = objGL.cache(_volume);
@endcode

@code{.cpp}
_lightObj     = objGL.cache(_light);
@endcode

@code{.cpp}
_pTriangleVO = objGL.cache(_vLst);
@endcode

The handles returned by the cache calls are then used in render instructions to point to the corrresponding cache components.

<hr>

## Rendering 

Once the caching is done, the render loop can begin and render instructions can be issued using the cache handles and corresponding transform properties.  The three common render instructions are look(), survery(), and render().  Apart from these three render instructions,
a render call or loop works with other control instructions including clear(), frameStart(), frameEnd() and finish().  

The method frameStart() begins to populate a ObjGL::RenderBundle component at the start of a single frame and at the end of a frame, frameEnd() call seals the RenderBundle.  This render bundling is done to avoid issuing individual render messages for each host render call.  This bundles the individual render calls from the host, into a packet of render messages, before sending it across to the clients.

The control and render instructions are issued in a strict order(except for look() and survery() which can be interchanged)
inside the render loop as shown below.

    clear
    frameStart
    look
    survey
    render
    frameEnd
    finish

### Look and Survey

The look() and survey() functions respectively sets the current view/camera and volume properties using the ObjGL::ViewObj and ObjGL::VolumeObj handles.

@code{.cpp}
objGL.look(_viewObj);
objGL.survey(_volumeObj);
@endcode

### Render

The render() function takes the ObjGL::VertexLstObj, ObjGL::TextureObj, ObjGL::MaterialObj handles as the first three components.  Currently ObjGL host mandates providing a texture and a material for every geometry.  The fourth component is the transform matrix of the VertexLst.  The last two components set the layer and light information for the scene.  

@code{.cpp}
objGL.render(_pTriangleVO, _pTriangleTO, _pTriangleMO, glm::mat4(), ObjGL_LAYER0, ObjGL::Lights::ObjGL_NO_LIGHTS);
@endcode

The scene lights are set using light bitfields.  Multiple lights can be set by using OR function between the light bitfields.

<hr>

## Clean Up

Post the rendering, when the host application closes or it tries to cache new geometry or other components, it must be removed from the host as well as the client memeory.  An array of remove() instructions are issued which clears the host cache, and also composes a remove message to be sent across to clients. 

@code{.cpp}
objGL.remove(_viewObj);
objGL.remove(_volumeObj);

objGL.remove(_pVolumeVO);
objGL.remove(_pVolumeTO);
objGL.remove(_pVolumeMO);

objGL.remove(_pTriangleVO);
objGL.remove(_pTriangleTO);
objGL.remove(_pTriangleMO);
@endcode

<hr>