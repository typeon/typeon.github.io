---
layout: post
title:  "reference vs pointer in C++"
date:   2016-08-28 11:50:00 +0900
categories: c++
---

## Some opinions about refereces vs pointers.

**My rule of thumb is:**

- Use pointers for outgoing or in/out parameters. So it can be seen that the value is going to be changed. (You must use `&`)
- Use pointers if NULL parameter is acceptable value. (Make sure it's `const` if it's an incoming parameter)
- Use references for incoming parameter if it cannot be NULL and is not a primitive type (``const T&``).
- Use pointers or smart pointers when returning a newly created object.
- Use pointers or smart pointers as struct or class members instead of references.
- Use references for aliasing (eg. ``int &current = someArray[i]``)

**There are following differences between pointers and references.**

- When it comes to passing variables, pass by reference looks like pass by value, but has pointer semantics
  (acts like pointer).
- Reference can not be directly initialized to 0 (null).
- Reference (reference, not referenced object) can not be modified
  (equivalent to `* const` pointer).
- const reference can accept temporary parameter.
- Local const references prolong the lifetime of temporary objects

**Taking those into account my current rules are as follows.**

- Use references for parameters that will be used locally within a function scope.
- Use pointers when 0 (null) is acceptable parameter value or you need to store parameter for further use.
  If 0 (null) is acceptable I am adding "_n" suffix to parameter,
  use guarded pointer (like QPointer in Qt) or just document it.
  You can also use smart pointers. You have to be even more careful with shared pointers than with normal pointers
  (otherwise you can end up with by design memory leaks and responsibility mess).

**A function uses passed data without modifying it:**

- If the data object is small, such as a built-in data type or a small structure, pass it by value.
- If the data object is an array, use a pointer because that’s your only choice. Make the pointer a pointer to const.
- If the data object is a good-sized structure,
  use a const pointer or a const reference to increase program efficiency.
  You save the time and space needed to copy a structure or a class design. Make the pointer or reference const.
- If the data object is a class object, use a const reference.
  The semantics of class design often require using a reference, which is the main reason C++ added this feature.
  Thus, the standard way to pass class object arguments is by reference.

**From C++ FAQ Lite -**

> Use references when you can, and pointers when you have to.
>
> References are usually preferred over pointers whenever you don't need "reseating". This usually means that references are most useful in a class's public interface. References typically appear on the skin of an object, and pointers on the inside.
> The exception to the above is where a function's parameter or return value needs a "sentinel" reference — a reference that does not refer to an object. This is usually best done by returning/taking a pointer, and giving the NULL pointer this special significance (references must always alias objects, not a dereferenced NULL pointer).
>
> Note: Old line C programmers sometimes don't like references since they provide reference semantics that isn't explicit in the caller's code. After some C++ experience, however, one quickly realizes this is a form of information hiding, which is an asset rather than a liability. E.g., programmers should write code in the language of the problem rather than the language of the machine.

Original source: [http://stackoverflow.com/questions/7058339/when-to-use-references-vs-pointers](http://stackoverflow.com/questions/7058339/when-to-use-references-vs-pointers)
