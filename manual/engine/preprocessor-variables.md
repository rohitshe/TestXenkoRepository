# Preprocessor variables

Various preprocessor variables can be used if you need to have different code depending on platform and graphics API.

Note that using `Platform.Type (ref:{SiliconStudio.Core.Platform.Type})` is recommended where possible.

# Platform

| Variable                               | Value                          |
| -------------------------------------- | ------------------------------ |
| SILICONSTUDIO_PLATFORM_WINDOWS         | Windows (standard and RT)      |
| SILICONSTUDIO_PLATFORM_WINDOWS_DESKTOP | Windows (non-RT)               |
| SILICONSTUDIO_PLATFORM_WINDOWS_RT      | Windows RT                     |
| SILICONSTUDIO_PLATFORM_WINDOWS_PHONE   | Windows Phone                  |
| SILICONSTUDIO_PLATFORM_MONO_MOBILE     | Xamarin.iOS or Xamarin.Android |
| SILICONSTUDIO_PLATFORM_ANDROID         | Xamarin.Android                |
| SILICONSTUDIO_PLATFORM_IOS             | Xamarin.iOS                    |


# Graphics API

| Variable                                      | Value                 |
| --------------------------------------------- | --------------------- |
| SILICONSTUDIO_PARADOX_GRAPHICS_API_DIRECT3D   | Direct3D 11           |
| SILICONSTUDIO_PARADOX_GRAPHICS_API_OPENGL     | OpenGL (Core and ES)  |
| SILICONSTUDIO_PARADOX_GRAPHICS_API_OPENGLCORE | OpenGL Core (Desktop) |
| SILICONSTUDIO_PARADOX_GRAPHICS_API_OPENGLES   | OpenGL ES             |


# Example

```cs
#if SILICONSTUDIO_PLATFORM_WINDOWS
    // Windows specific code goes here...
#elif SILICONSTUDIO_PLATFORM_MONO_MOBILE
    // iOS and Android specific code goes here...
#else
    // Other platforms code goes here...
#endif
```


