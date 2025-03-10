Code & Content
==============
In Unreal we have generally two types of codes and one type of content.

Content
-------
"Content" in terms of 3D models, sounds, textures, materials and so on are called ``Assets``.
You use these assets to make stuff in your game f.e. visible to the user.
There are multiple types of assets:

- StaticMesh
    A simple 3D Object like a .obj or .fbx
- Sound
    Sounds like the background music or footsteps.
- Skeleton
    Characters like the player or enemies have a skeleton to tell the engine f.e. where bones and joints are.
- TextureSample
    Textures like the grass texture, ficsit logo or the items in the inventory are simple images called "Texture Samples"
- Materials
    Every surface has a material applied on it, so the renderer does know how he should let the surface look like.
    Here textures and surface properties like reflection and roughness come together.
- BluePrints
    We will discuss this in this `Concept-Page <BluePrints>`_

Native Code
-----------
Native code describes code that is written in C++ and can't directly be changed in the Unreal Editor because the Editor itself uses this code.
This kind of code can interact with the computer on a very low level and is often used when performance is really important.
Much things of Satisfactory is written in C++ so you will often find out that there is the functionality you cant access from within ``BluePrints``.
Native code is compiled directly into machine code and that means for every platform you want to compile for, you would need to compile this code separately.

.. warning:: This can be a problem for you when the game updates. Because that would mean you need to download the C++ code again and compile it, and this can be difficult if you don't know what you do.

Besides this downside, native code allows you to low-level manipulation of the engine environment and also allows you to implement third-party libraries like discord, curl and Lua.

BluePrint Code
--------------
This is a kind of asset which is compiled into the pak files as a kind of bytecode which then gets loaded and linked at runtime through the Unreal BP VM.

.. hint:: To learn more about ``BluePrints`` visit the `Concept-Page <BluePrints>`_ about that topic.