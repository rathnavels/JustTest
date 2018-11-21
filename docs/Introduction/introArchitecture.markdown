ObjGL Architecture {#introArchitecture}
============================

## ObjGL Architecture

### Host Architecture

@htmlonly <style>div.image img[src="../Images/ObjGLHostArchitecture.png"]{width:40px;}</style> \endhtmlonly
@image html "../Images/ObjGLHostArchitecture.png" 


- Main thread 
    -    Caches Geometry, Textures, Material, Lights, View and Volume
    -    Interacts with user.
    -    Physics and Transforms
    -    Rendering
- ObjGL Manager thread 
    -    Initiates ObjGL server
    -    Composes cache/render calls into cache/render bundle messages.
    -    Broadcasts messages via TCP/IP 
    -    Tends to ‘Request’ messages from client.


### Client Architecture

@htmlonly <style>div.image img[src="../Images/ObjGLClientArchitecture.png"]{width:40px;}</style> \endhtmlonly
@image html "../Images/ObjGLClientArchitecture.png" 

- Main thread 
    - Controls the render operations 
- Subscriber thread
    - Receives broadcast messages from ObjGL host
- Requestor thread
    - Requests and receives out-of-band cache updates from host application.
