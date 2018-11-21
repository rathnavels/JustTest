Tutorial - ObjGL Client {#tutorial01client}
============================

This is a simple ObjGL client application tutorial!

This connects to a broadcasting host, receives broadcast messages and parses cache and render instructions by linking to the ObjGL::Render library and then renders onto a 2D viewport as defined by the client application.  Please refer the <B> Tutorial01Client </B> project in the SDK as you follow this tutorial.

<hr>

Each client application has two main components <code> ObjGL::ComMan </code> and <code>ObjGL::DSR</code>.  We begin with initializing a ComMan(Command Manager) object and a DSR(Display Side Renderer) object.

@code{.cpp}
ComMan              *pComMan(0);                      
DSR                 *pRenderer(0);                    
@endcode


@code{.cpp}
pComMan          = new ComMan();
pRenderer        = new ViewClient();
@endcode

These two objects work in parallel, with ComMan managing the communication with the host and DSR managing the parsing of the received messages and subsequent rendering.

<hr>

## Configuration and Initialization

Every client application takes a .json configuration file as a command line argument, which defines the host server properties to connect to and the rendering properties pertinent to the client.  

### A Sample Client Configuration JSON File

For example, this config file for this tutorial is <B> RenderView.json </B>
@code{.json}
{
  "ComMan": {
    "Publisher": {
      "Address": "127.0.0.1",
      "Port": "5557"
    },
    "Responder": {
      "Address": "127.0.0.1",
      "Port": "5558"
    }
  },
  "RenCon": {
    "Renderer" : "View",
    "WindowDim": [ 1024, 1024 ]
  },
  "OpenGL": {
    "Version":"4.6",
	"Pipeline": "shader",
	"Library": "GLFW"
  }
}
@endcode

The properties under <b> ComMan </b> Publisher sets the host address and port for broadcast messages.  The Responder sets the properties for cache request messages sent by the client to the host.  The properties under <b> RenCon </b> sets the type of renderer and window properties.  In this case, the Renderer 'View' denotes a simple 2D client.

Once the .json file is parsed, the ComMan configures the Publisher and Responder with corresponding IP address and port details.  The choice of factory can also be parsed from the configuration file, and the appropriate factory is initialized.

The Display Side Renderer object is initialized, which in turn sets up a list of function pointers that correspond to the type of incoming message.

@code{.cpp}
pRenderer->init();
@endcode

<hr>

## Rendering

Before the render loop begins, the ComMan is set in motion in a different thread.

@code{.cpp}
pComMan->start();
@endcode

Inside the render loop, three message list pointers are initialized, each for cache messages, render messages  from the host machine and cache update request messages sent from the display to the host machine.

@code{.cpp}
MsgLstPtr     spMsgCacheLst(0);
MsgLstPtr     spMsgRenderLst(0);
MsgLstPtr     spMsgSendLst(0);
@endcode

The ComMan object calls the <code> recvMsgLst() </code> method to receive the incoming cache and render messages.  This populates the lists with corresponding cache and render messages.

@code{.cpp}
pComMan->recvMsgLst(spMsgCacheLst,spMsgRenderLst);
@endcode

As soon as the messages are received, they are parsed and corresponding action is followed through by calling the function pointers that are assigned initially. The call to <code> processMsgs() </code> decodes the type of message and calls the appropriate function through the function pointer.

The client renderer is supposed to derive from DSR and override <code> prerender() </code> and <code> render() </code> functions.  

@code{.cpp}
if (spMsgCacheLst)
  pRenderer->processMsgs(spMsgCacheLst, spReqSendLst);

if (spMsgRenderLst)
{
  pRenderer->processMsgs(spMsgRenderLst, spReqSendLst);
  pRenderer->preRender();
  pRenderer->render();
  pRenderer->postRender();
}
@endcode

When a render message is processed by <code> processMsgs() </code> and found to point to a cache handle which is not available in the cache map, the <code> spReqSendLst </code> is populated and an update request message is sent to the host.

@code{.cpp}
if (spMsgSendLst && !spMsgSendLst->empty())
  pComMan->sendMsgLst(spMsgSendLst);
@endcode

<hr>










