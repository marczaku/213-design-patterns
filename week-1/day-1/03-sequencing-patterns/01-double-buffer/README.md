# 9 Double Buffer
Double Buffering is a technique used to ensure that something can be updated to a new version while preserving the old version until the new version is finished / available.

It is for example used for example in Rendering, so your Computer Screen only shows fully rendered scenes and you don't see how the scene is built up one object at a time.
- Unity: Uses Double-Buffering internally
- C#: All common Graphics APIs use Double-Buffering per default (OpenGL, DirectX, Vulcan, ...)
- C++: All common Graphics APIs use Double-Buffering per default (OpenGL, DirectX, Vulcan, ...)
