UInterface
==========
`UInterfaces <UInterface>` are unreals answer to multiple inheritance. Interfaces are only able to hold public functions and are not able to hold f.e. static functions or member variables.
Each public function of a UInterface needs to be virtual so it can get overwritten by child classes.
This overwriting is key because interfaces are used to describe callable functions but their effect should differ from implementation to implementation.