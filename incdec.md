# inc and dec

The inc instruction increases the value by 1 and dec decreses the value by 1.

In C programming language, ``++`` and ``--`` operators are compiled into inc and dec instructions, respectively.
Why are ``++`` and ``--`` operators included in C language?
``++`` is faster than ``add cx, 1`` in the old processors. ``--`` is faster than dec instruction.

Why does processor support inc and dec instruction?
Because adding by 1 and substracting by 1 are very very common.
The inc and dec operations are not used so many times only for arithmatic calculation code but also loop code, array accessing, pointer operation and so on.
Therefore fast increasing by 1 and fast decresing by 1 are necessary.
