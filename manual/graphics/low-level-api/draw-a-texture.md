# Draw a texture

In addition to the convenient way to create and manipulate textures in Xenko, the engine provides a built-in method to display such texture on screen. This can be useful to display a buffer fullscreen in case of indirect rendering, to generate mipmaps or for debugging purposes (visualization of GBuffer). The user simply calls the @'SiliconStudio.Xenko.GraphicsDevice.DrawTexture' method. The user cannot change the shader used on this texture. The sampling method and a color filter are the only variables accessible to the user. To create a sampler, please refer to the corresponding documentation.

**Code:** Drawing a texture

```cs
// display the texture
GraphicsDevice.DrawTexture(myTexture);
 
// display the texture with a red filter
GraphicsDevice.DrawTexture(myTexture, Color.Red);
 
// use a custom sampler
GraphicsDevice.DrawTexture(myTexture, mySamplerState);
```


The texture will be displayed in the set viewport (so often fullscreen). To change where the exture will be displayed or wich part will be displayed, the user should use [viewport](render-states.md#viewport-states) and [scissor](render-states.md#scissor-states) states.

# Sampler states

Since a sampler state can be provided, the @'SiliconStudio.Xenko.Graphics.GraphicsDevice' class offers a set of predefined ones. These are:

- PointWrap
- PointClamp
- LinearWrap
- LinearClamp
- AnisotropicWrap
- AnisotropicClamp

**Code:** Set a sampler state over a DrawTexture

```cs
GraphicsDevice.DrawTexture(myTexture, GraphicsDevice.SamplerStates.PointWrap);
GraphicsDevice.DrawTexture(myTexture, GraphicsDevice.SamplerStates.PointClamp);
GraphicsDevice.DrawTexture(myTexture, GraphicsDevice.SamplerStates.LinearWrap);
GraphicsDevice.DrawTexture(myTexture, GraphicsDevice.SamplerStates.LinearClamp);
GraphicsDevice.DrawTexture(myTexture, GraphicsDevice.SamplerStates.AnisotropicWrap);
GraphicsDevice.DrawTexture(myTexture, GraphicsDevice.SamplerStates.AnisotropicClamp);
```


The user can also create custom samplers. This is achieved thanks to the @'SiliconStudio.Xenko.Graphics.SamplerState' and @'SiliconStudio.Xenko.Graphics.SamplerStateDescription' classes. To have the whole list of features, please refer to the documentation of each class.

**Code:** Custom sampler

```cs
// create the sampler state description containing all the states
var samplerStateDescription = new SamplerStateDescription(TextureFilter.MinMagPointMipLinear, TextureAddressMode.Clamp);
samplerStateDescription.MaxAnisotropy = 8;
 
// create the sampler state object
var samplerState = new SamplerState(GraphicsDevice, samplerStateDescription);
 
// use it
GraphicsDevice.DrawTexture(myTexture, samplerState);
```


