# Graphics

In this section you will find details about how to use the editor and API related to Graphics and Rendering.

# Shaders

Shaders are authored in the [Xenko's shading language](effects-and-shaders/shading-language/index.md), which is an extension of `HLSL`.

They provide true **composition** of modular shaders through the use of [inheritance](effects-and-shaders/shading-language/classes-mixins-and-inheritance.md), shader [mixins](effects-and-shaders/shading-language/composition.md) and [automatic weaving of shader in-out attributes](effects-and-shaders/shading-language/automatic-shader-stage-input-output.md).

# Effects

[Effects](effects-and-shaders/effect-language.md) in Xenko use C#-like syntax to further combine shaders. They provide **conditional composition** of shaders to generate **effect permutations**.

# Target everything

Xenko shaders are converted automatically to the target graphics platform, either plain HLSL for Direct3D, `GLSL` for OpenGL, or `SPIR-V` for Vulkan platforms.

# Advanced Graphics

The Graphics module provides a set of comprehensive methods to display the game. Although the engine is available on multiple platforms, the whole system behaves like DirectX 11 from the user perspective.

A basic prior knowledge of the rendering pipeline is expected from the user.

## Topics

- [Lighting and Shadows](lighting-and-shadows/index.md)
- [Graphics Compositor](graphics-compositor/index.md)
- [Materials](materials/index.md)
- [Post-Effects](post-effects/index.md)
- [Rendering pipeling](rendering-pipeline/index.md)
- [Low-level API](low-level-api/index.md)
- [Effects and Shaders](effects-and-shaders/index.md)
- [HOWTOs](howtos/index.md)