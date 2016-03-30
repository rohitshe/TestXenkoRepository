# Effects and Shaders

# Custom shaders and effects

Xenko uses a programmable shading pipeline. The user can write custom shaders, create @'SiliconStudio.Xenko.Graphics.Effect's from them and use them for drawing.
The @'SiliconStudio.Xenko.Rendering.EffectSystem' class provides an easy way to load an effect.

**Code:** Load an effect

```cs
var myEffect = EffectSystem.LoadEffect("MyEffect").WaitForResult();
```

You can then bind the effect as [pipeline state](../low-level-api/pipeline-state.md).

An effect also often defines a set of parameters. To set these, you will need to [bind resources](../low-level-api/resources.md) before drawing.

# Shaders

Shaders are authored in the [Xenko's shading language](shading-language/index.md), which is an extension of `HLSL`.

They provide true **composition** of modular shaders through the use of [inheritance](shading-language/classes-mixins-and-inheritance.md), shader [mixins](shading-language/composition.md) and [automatic weaving of shader in-out attributes](shading-language/automatic-shader-stage-input-output.md).

# Effects

[Effects](effect-language.md) in Xenko use C#-like syntax to further combine shaders. They provide **conditional composition** of shaders to generate **effect permutations**.

Since some platform can't compile shaders at runtime (iOS, Android, etc...), effect permutation files (.xkeffectlog) are used to enumerate all permutations ahead-of-time.

# Target everything

Xenko shaders are converted automatically to the target graphics platform, either plain HLSL for Direct3D, `GLSL` for OpenGL, or `SPIR-V` for Vulkan platforms.

For mobile platforms, shaders are optimized by a GLSL optimizer in order to improve performance on these devices.