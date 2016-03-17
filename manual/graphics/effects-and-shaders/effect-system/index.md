# Effect System

# Effect Language: combine shaders easily

Xenko Shaders are written in [XKSL (Xenko Shader Language)](../shading-language/index.md).

This language is quite flexible and allow user to write lot of small reusable shader blocks, one per feature.

They can be easily combined in a coherent shader with the help of [Xenko Effect Language](effect-language.md).

This logic happens in .XKFX files.

# Effect System: instantiate Effect

@'SiliconStudio.Xenko.Effects.EffectSystem' is used to instantiate @'SiliconStudio.Xenko.Graphics.Effect' from a given .XKFX definition.

At runtime, if effect parameters are supposed to instantiate a new shader, it will be automatically generated and used.

# Effect Permutations: enumerate shaders

Since some platform can't compile shaders at runtime (iOS, Android, etc...), [effect permutation files (.pdxfxlib)](effect-permutations.md) are used to enumerate all those permutations ahead-of-time.

