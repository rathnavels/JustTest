ObjGL Instruction Set {#introInstrSet}
============================

### Instruction Set

ObjGL Instruction Set is of three categories.
    
    - Control
    - Cache
    - Render
    


| Control | Cache | Render |
|--------|--------|--------|
|Clear|Cache Viewpoint|Look|
|Finish|Cache View Volume|Survey|
|FrameStart|CacheLight|Render|
|FrameEnd|Cache Material|.|
|.|Cache Vertex List|.|
|.|Cache Texture|.|
|.|Remove Viewpoint|.|
|.|Remove ViewVolume|.|
|.|Remove Light|.|
|.|Remove Material|.|
|.|Remove Vertex List|.|
|.|Remove Texture|.|


#### Control
Control instructions are used to initiate and delineate render frames.  Cache coherency is maintained within control blocks, i.e., cache update is locked between a clear and a finish command.  The frameStart() and frameEnd() calls respectively starts and ends populating an ObjGL::RenderBundle.  

#### Cache
Cache instructions consists of functions that are responsible for the caching and removing of data such as geometry, material, textures, lights, view and volume data to and from the render hardware respectively.  A cache instruction from the host, caches the data in the host graphics device memory by making factory calls.  It also makes a server insert call which then composes a cache message that is to be sent over to the client displays as a cache message.  

#### Render
Render instructions consists of three main commands.  The ‘look’ method is used to specify the active view object and ‘survey’ method is used to specify the active view volume object for a volumetric or light-field display.  This implies that an application can have multiple view and view volume objects cached in the memory.  A client application may allow the user to switch between different views and volumes by making it the active view or volume object using ‘look’ and ‘survey’ methods at run time.  
The render instruction is responsible for enabling the actual rendering process by activating/ deactivating the light, binding the material and texture, and rendering the vertex lists.  The applications running ObjGL would cache geometry, textures, and lights in advance of rendering a frame.  

