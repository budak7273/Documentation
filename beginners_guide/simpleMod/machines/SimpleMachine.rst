First Simple Machine
====================
In this chapter we create our first simple machine which simply counts the items and the different types of items passes through it.

Create the machine
------------------
Creating a simple machine is... simple...
First of all, you need to create again a `buildable <../buildable>`_ with descriptor and recipe, but this time use f.e. :download:`this <Mesh_SimpleMachine.fbx>` as the mesh and :download:`this <Icon_SimpleMachine.png>` as icon.

Now we simply want to add the `factory-connectors <../../Concepts/FactoryConnector>`_ (conveyor input and output).
For that add two new components of type ``FGFactoryConnectionSim`` to the actor and place them in the lower center of the input/output connection points of the mesh, don't forget to rotate them (currently we dont realy have any reference how you can see in wich direction the connection points to).

Set the outputs direction to output and the type to Item.
The inputs type also needs to get set to Item but the direction needs to be input.

We also want ot make our machine depend on electricity. For that we need to add a `FGPowerConnectionComponent <../../Concepts/PowerNetwork>`_ at the position of the power connection point of the mesh.
Set ``M Max Num Connections`` to something like 1.

Init logic
----------
The init logic happens in the ``BeginPlay`` event and initializes the base state of the factory (also based on the save information)
There is not that much we need to do here. The only thing we need to do is to set the PowerInfo of the power connector to your added ``FGPowerInfoComponent`` and we need to set the target consumption of the power-info to something like 1.0.

.. image:: SimpleMachine_Init.jpg

Grab logic
----------
The grab function of the ``FGBuildable`` gets executed by the output connector when a new item gets requested.
To add logic to it, we simply need to override it.

Because we want to store the count of items transferred and the count of different item-types, we will need to add a new global integer variable, let's call it "itemAmount" and and a array of ``FGItem``-Classes, call it "itemTypes".

Now, in the grab logic, first we check if the machine has power. We do this by using the "HasPower"-node of the ``FGPowerInfoComponent``. If it returns false, we return nothing and the boolean is false.

But if it's true, we use the grab function of the input connector to request a new item.

If the boolean is true, we increase the "itemAmount" integer by one and add the item unique to the "itemTypes". Now we simply log "itemAmount" and the size of the "itemTypes"-array. After that we return the item we got from the grab and true.

If the boolean is false, we just return nothing (the boolean needs to be false).

.. image:: SimpleMachine_grab.jpg

That's it, try out your newly created machine!