Paintable
=========
Making things paintable is not that difficult as it seems. You can use the standard way, or create your implementation.

Standard Material (Factory_Inst)
--------------------------------
The standard material (Factory_Inst) used by SF and provided by the SF modding unreal starter project is the best way to implement the ability to paint.

It gets implemented by the ``FGBuildable``-class und changes the primary and secondary material parameters of the Factory_Inst material.
You can find it in the unreal starter project under ``Content/FactoryGame/Buildable/-Shared/Material``.

This material implements also multiple other surface materials. It works as an atlas map.

This is a simplified representative atlas map for the material you can use to UV map your models.

.. image:: Factory_Inst.png

Custom implementation
---------------------
You can implement the ability to paint in any object by implementing the ``IFGColorInterface``.
Simply override the given functions with the functionality you need and you're done!

The paint gun will now interact with your object, it can do this by using the reflection system.