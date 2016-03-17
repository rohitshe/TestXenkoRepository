# Render states

Xenko offers total control over the rendering states. This includes:

- Rasterizer states
- Depth and stencil states
- Blend states
- Viewport states
- Scissor states

The states are accessible through the @'SiliconStudio.Xenko.Graphics.GraphicsDevice' class.

# Rasterizer states

The user can set the rasterizer states thanks to the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.SetRasterizerState' method. The @'SiliconStudio.Xenko.Graphics.GraphicsDevice' class has a set of predefined rasterizer states which should be enough in most cases. They deal with the cull mode:

- CullNone: no culling
- CullFront: front-face culling
- Cullback: back-face culling

**Code:** Setting the cull mode

```cs
GraphicsDevice.SetRasterizerState(GraphicsDevice.RasterizerStates.CullNone);
GraphicsDevice.SetRasterizerState(GraphicsDevice.RasterizerStates.CullFront);
GraphicsDevice.SetRasterizerState(GraphicsDevice.RasterizerStates.CullBack);
```


However, the user can create its own custom state. It needs a @'SiliconStudio.Xenko.Graphics.RasterizerState' object and a @'SiliconStudio.Xenko.Graphics.RasterizerStateDescription' object. Please refer to the reference documentation of the @'SiliconStudio.Xenko.Graphics.RasterizerStateDescription' class to get the complete list of available options and the default values.

**Code:** Custom rasterizer states

```cs
var rasterizerStateDescription = new RasterizerStateDescription(CullMode.Front);
rasterizerStateDescription.ScissorTestEnable = true; // enables the scissor test
var rasterizerState = RasterizerState.New(GraphicsDevice, rasterizerStateDescription);
GraphicsDevice.SetRasterizerState(rasterizerState);
```


# Depth and stencil states

The user can set the depth and stencil states thanks to the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.SetDepthStencilState' method. The @'SiliconStudio.Xenko.Graphics.GraphicsDevice' class has a set of predefined depth states:

- Default: depth read and write with a less than function
- DefaultInverse: read and write with a greater-equal function
- DepthRead: read only with a less than function
- None: no read nor write

**Code:** Setting the depth state

```cs
GraphicsDevice.SetDepthStencilState(GraphicsDevice.DepthStencilStates.Default);
GraphicsDevice.SetDepthStencilState(GraphicsDevice.DepthStencilStates.DefaultInverse);
GraphicsDevice.SetDepthStencilState(GraphicsDevice.DepthStencilStates.DepthRead);
GraphicsDevice.SetDepthStencilState(GraphicsDevice.DepthStencilStates.None);
```


If necessary, the user can set totally customs depth and stencil states. It needs a @'SiliconStudio.Xenko.Graphics.DepthStencilState' object.

**Code:** Custom depth and stencil state

```cs
// depth test is enabled but it doesn't write
var depthStencilStateDescription = new DepthStencilStateDescription(true, false);
var depthStencilState = DepthStencilState.New(GraphicsDevice, depthStencilStateDescription);
 
// setting the depth state and the stencil with a reference value of 0
GraphicsDevice.SetDepthStencilState(depthStencilState);
 
// setting the depth state and the stencil with a reference value of 2
GraphicsDevice.SetDepthStencilState(depthStencilState, 2);
```


# Blend state

The user can set the blend state thanks to the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.SetBlendState' method. The @'SiliconStudio.Xenko.Graphics.GraphicsDevice' class has a set of predefined blend states:

- Additive: sums the colors 
- AlphaBlend: sums the colors using the alpha of the source on the destination color
- NonPremultiplied: sums using the alpha of the source on both colors
- Opaque: replaces the color

**Code:** Setting the blend state

```cs
GraphicsDevice.SetBlendState(GraphicsDevice.BlendStates.Additive);
GraphicsDevice.SetBlendState(GraphicsDevice.BlendStates.AlphaBlend);
GraphicsDevice.SetBlendState(GraphicsDevice.BlendStates.NonPremultiplied);
GraphicsDevice.SetBlendState(GraphicsDevice.BlendStates.Opaque);
```


If necessary, the user can create custom blend states. It needs a @'SiliconStudio.Xenko.Graphics.BlendState' object and a @'SiliconStudio.Xenko.Graphics.BlendStateDescription' object. Please refer to the reference documentation of the @'SiliconStudio.Xenko.Graphics.BlendStateDescription' and @'SiliconStudio.Xenko.Graphics.BlendStateRenderTargetDescription' classes to get the complete list of available options and the default values.

**Code:** Custom blend state

```cs
// create the object describing the blend state per target
var blendStateRenderTargetDescription = new BlendStateRenderTargetDescription();
blendStateRenderTargetDescription.BlendEnable = true;
blendStateRenderTargetDescription.ColorSourceBlend = Blend.SourceColor;
// etc.
// create the blendStateDescription object
var blendStateDescription = new BlendStateDescription(Blend.SourceColor, Blend.InverseSourceColor);
blendStateDescription.AlphaToCoverageEnable = true; // enable alpha to coverage
blendStateDescription.RenderTargets[0] = blendStateRenderTargetDescription;
var blendState = BlendState.New(GraphicsDevice, blendStateDescription);
 
// most common set
GraphicsDevice.SetBlendState(blendState);
 
// using a blend factor and a mask
GraphicsDevice.SetBlendState(blendState, Color4.White, 2);
```

