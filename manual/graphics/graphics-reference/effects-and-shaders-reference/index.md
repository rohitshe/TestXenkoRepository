# Effects and Shaders Reference

# Custom shader and effect

Xenko uses a programmable shading pipeline. The user can write a custom shader, create an associate effect and use it. This section will only cover how to use such effect. To learn how to create an effect from a shader, the user should learn the Xenko extensions of HLSL used in to write a shader. Please refer to the documentation.

The @'SiliconStudio.Xenko.Effects.EffectSystem' class provides an easy way to load an effect. It creates an instance of the @'SiliconStudio.Xenko.Graphics.Effect' class.

**Code:** Load an effect

```cs
var myEffect = EffectSystem.LoadEffect("MyEffect");
```


Then the user can apply it on any mesh or drawable entity:

**Code:** Apply an effect

```cs
// set the effect as active
myEffect.Apply();
 
// draw calls
// (...)
```


An effect often has a set of parameters. To learn how to set them, please refer to the documentation of the @'SiliconStudio.Xenko.Graphics.Effect' class.

# Simple effect shader

Xenko is bundled with a basic built-in shader: SimpleEffect. To use it, simply create an instance of the @'SiliconStudio.Xenko.Graphics.SimpleEffect' class. Please refer to the @'SiliconStudio.Xenko.Graphics.SimpleEffect' class reference to know all the available options. These options include:

- a tint color
- a transformation matrix
- a texture
- a texture sampler

**Code:** SimpleEffect shader

```cs
var simpleEffect = new SimpleEffect(GraphicsDevice);
simpleEffect.Texture = myTexture; // set a custom texture
 
// use it on a primitive
simpleEffect.Apply();
myTorus.Draw();
```


