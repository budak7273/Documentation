Factory-Connectors
==================
Factory connectors are SFs system to transfer items from one machine to the next one.
Also, conveyors have factory connectors to transfer items from a machine into its internal buffer (the belt).

The Factory-Connector-System is a pull-based system. That means if I a machine has f.e. an empty internal buffer and wants to get filled by a factory connector, the machine requests (grabs) an item from the given connector.
The connector then can reply with nothing or the item it transfers. If it returns something, the caller (the machine) needs to make sure it does something with that item, else the item just disappears.
The Factory-Connector itself asks in grab the connected Factory-Connector or it's outer object to return an item.
A machine (``AFGBuildable``) itself contains a Factory-Connector which contains a reference to a connected one, and that one will grab the item from the connected machine (belt).

This is a flow example of a machine output of the grab calls.

A conveyor which has a free space -> FactoryConnector in the conveyor which is connected to the machines connector -> Factory-Connector of the machine which is connected to the connector of the conveyor -> Machine

This is a flow example of a machine input of the grab calls.

Machine -> Factory-Connector of the machine connected to the connector of the conveyor -> Factory-Connector of the conveyor connected to the connector of the machine -> Conveyor

FGFactoryConnection
-------------------
A ``UFGFactoryConnection`` is a ``USceneComponent`` used to implement a Factory-Connector.
Use this directly as a component for you ``AFGBuildable``.

- M Connector
    The actual type of connection. (Pipes revealed but only existent as connector type)
- M Direction
    The I/O direction the factory connector works.
- M Connector Clearance
    After which length the conveyor is allowed to bend
- M Forward Peek and Grab to Buildable
    If the grab and peeks should get redirected to the owner (just ``FGBuildable`` works)