UFunction
=========
`UFunctions <UFunction>`_ are `UStructs <UStruct>`_ containing information about a function of a class.
Each function contains a name, flags and again some children as well as eventually a native function pointer if it is a native function.

Children
--------
These children (of the base class `UStruct <UStruct>`_) describe which input parameters, output parameters and return value this function has.

Invoking
--------
You can invoke a `UFunction`_ through two main ways.

1.  Using the ``invoke``-function of the `UFunction`_

    .. code-block:: c++
        
        UObject* obj;
        struct ParameterStruct;
        UFunction* func;
        ParamterStruct params;
        
        func->invoke(obj, params);
2.  Using the ``processEvent``-function of `UObject`_

    .. code-block:: c++
        
        UObject* obj;
        struct ParameterStruct;
        UFunction* func;
        ParamterStruct params;
        
        obj->processEvent(func, params);

ParamterStruct
--------------
All parameters the given function has, need to get a place in a structure, the so-called "Parameter Structure" (aka. "ParamStruct")
This structure is simply all C++ types given through the `UPropertys <UProperty>`_ in a row.

This example shows a param struct for a ``FVector`` and an `UObject <UObject>`_-ref as input, a ``bool`` as an output parameter and an ``int`` as a return value.

.. code-block:: c++

    struct paramStruct {
        FVector vectorInput;
        UObject* otherObj;
        bool worked;
        int count;
    }

Flags
-----
The Function-Flags provide the ability to define different kinds of functions like abstract functions, static function or just member functions.
These flags work as register, each bit of the integer is a different flag.
To check, set or unset a flag use the bitwise ``&`` (and), ``|`` (or) and ``~`` (negate) operators.

Native Exec-Function
--------------------
The native execution function gets called when the `UFunction`_ get invoked.
The native function needs to be static and gets a `UObject`_-ptr as context object, an FFrame-ref, to get and set parameters and also to increase the code pointer, and a pointer to the Return-Value-Structure (a struct containing all return values, not out params).
This means your exec-function declaration needs to look like ``static void execFuncName(UObject* obj, FFrame& stack, void* retVals)``

Input Params
''''''''''''
The stack (or FFrame) is used to give access to the execution and value flow.
Getting parameters (in and out) depends on the order given in the building of the UFunction.
To get a value you simply need to allocate a space where the input can get copied to, and after that, you just give the pointer to the location the ``stepCompIn`` function which gets the input param and stores it in that pointer.

.. code-block:: c++

    void execFunc(void* ctx, FFrame& stack, void* ret) {
        FString input1;
        FString input2;
        frame.stepCompIn(&input1);
        frame.stepCompIn(&input2);

        // other stuff
    }

Output Params
'''''''''''''
Output params are very similar to the input parameters with one key difference: You use the ``stepCompInRef`` function which takes again a pointer to allocated storage for the value but this time it also returns a reference, and to set the final value you just need to set the reference.

.. code-block:: c++

    void execFunc(void* ctx, FFrame& stack, void* ret) {
        FString out1_temp;
        int out2_temp;
        FString& out1 = frame.stepCompInRef(&out1_temp);
        int& out2 = frame.stepCompInRef(&out2_temp);

        // other stuff

        out1 = "nice";
        out2 = 42;
    }

Return Values
'''''''''''''
The retval pointer is passed with a valid pointer to allocated space for the return values.
To set those, just simply set the values of the struct pointed to.

.. code-block:: c++

    struct RetVals {
        int retVal1;
    }

    void execFunc(void* ctx, FFrame& stack, RetVals* ret) {
        // other stuff

        ret->retVal1 = 42;
    }

Code Counter
''''''''''''
When the exec function gets executed you should increment the code counter of the stack after you get the params. If the current code counter is nullptr, just leave it as it is.

.. code-block:: c++
    
    void execFunc(void* ctx, FFrame& stack, void* ret) {
        // params

        stack.code += !!stack.code;

        // other stuff
    }

Building
--------
The BPI provides the ``SML::Paks::FunctionBuilder``-Class in ``assets/BPInterface.h`` for easy declaring and defining custom `UFunctions <UFunction>`_.

The class contains for each basic type of function a static initialize function.

- static
    Use this for static functions (f.e. in Blueprint Function Libraries).
    You will get in the execution function the default object as a context object.
- member
    Use this for member functions (f.e. in actors who need the context of the actor to work properly). You get the "this" object in the execute-function as a context object.

As you will see, the function takes the name of the new function as a parameter. 

Native-Function
'''''''''''''''
If your UFunction should get linked to a native execution function use the provided ``native`` to set the function pointer and the needed function-flags.

Parameters
''''''''''
To add a `UProperty <UProperty>`_ use ``param`` and pass the property builder.
If your property needs an offset value, and you don't provide one, the builder will get the last property and uses its offset and dimension to get the new offset.
If there is no previous property, it uses 0 as offset and 0 as dimension.

Flags
'''''
The builder class provides the ``addFlag`` and ``removeFlag`` methods for manipulating the function flag register.

Finishing
'''''''''
To finish building just call the ``build`` function of the builder class. If you use the function in f.e. the ClassBuilder, you just give it the unbuild ``FunctionBuilder``

Attaching
'''''''''
You're also able to attach a new custom `UFunction`_ to an existing `UClass`_ with the ``attach`` function. The class will get properly built by the builder class when needed.