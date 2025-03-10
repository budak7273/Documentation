Blue-Print Interface
====================
The BPI provides multiple helper classes and functions for manipulating the reflection system of unreal.

The Problem
-----------
Unreal uses a reflection system to have mainly a proper interface to there visual scripting language called Blueprint.
If you want now to add something to this reflection system you were not able to do this through the ``Unreal Header Tool`` which provides features for Unreal C++ Devs to create easily proper reflection system interfaces.
This is problematic especially if you want to integrate third-party libraries or make your code way more performant.

The Solution
------------
The BPI uses the same interface as the ``Unreal Header Tool`` uses but wraps this interface into easy to use builder classes.
Through this you can:

- Attach custom functions to existing ``UObjects``
- Creating custom ``UClasses``
- Creating custom functions
- and many more

General
-------
The BPI is located under ``assets/BPInterface.h`` in the SML root and also provides better structures as the SDK under ``util/Objects``

.. warning:: We heavily recommend using the BPI objects instead of the SDK ones cause the SDK is auto-generated and provides less save implementations.

.. error::
    Please build/attach things with the provided BuilderClasses only in on ``UGameEngine::Start``. Hook it if needed.
    
    .. code-block:: c++

        ::subscribe<&SML::Objects::UGameEngine::Start>([](void* ret, void* _) {
            // Build/Attach here
        });

.. toctree::
    :maxdepth: 2

    UObject
    UField
    UProperty
    UStruct
    UFunction
    UClass