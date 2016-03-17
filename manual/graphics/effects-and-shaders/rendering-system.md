# Rendering System

# Shaders

**[Shaders](shading-language/index.md)** in Xenko are extended shaders derived from `HLSL`.

They provide true **composition** of modular shaders through the use of **[inheritance](shading-language/classes-mixins-and-inheritance.md)**, shader **[mixins](shading-language/composition.md)** and **[automatic weaving of shader in-out attributes](shading-language/automatic-shader-stage-input-output.md)**.

# Effects

**[Effects](effect-system/index.md)** in Xenko combines shaders into a full shader. They provide conditional **[composition](effect-system/effect-language.md)** of shaders and **[effect permutations](effect-system/effect-permutations.md).**

# Target everything

Xenko shaders are converted automatically to the target graphics platform, either plain HLSL or GLSL output shader files.

For mobile platforms, shaders are optimized by a GLSL optimizer in order to improve performance on these devices.

