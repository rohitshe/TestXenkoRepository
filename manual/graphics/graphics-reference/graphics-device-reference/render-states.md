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
GraphicsDevice.SetRasterizerState(GraphicsDevice.RasterizerStates.CullBack);```


However, the user can create its own custom state. It needs a @'SiliconStudio.Xenko.Graphics.RasterizerState' object and a @'SiliconStudio.Xenko.Graphics.RasterizerStateDescription' object. Please refer to the reference documentation of the @'SiliconStudio.Xenko.Graphics.RasterizerStateDescription' class to get the complete list of available options and the default values.

**Code:** Custom rasterizer states

```cs
var rasterizerStateDescription = new RasterizerStateDescription(CullMode.Front);
rasterizerStateDescription.ScissorTestEnable = true; // enables the scissor test
var rasterizerState = RasterizerState.New(GraphicsDevice, rasterizerStateDescription);
GraphicsDevice.SetRasterizerState(rasterizerState);```


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
GraphicsDevice.SetDepthStencilState(GraphicsDevice.DepthStencilStates.None);```


If necessary, the user can set totally customs depth and stencil states. It needs a @'SiliconStudio.Xenko.Graphics.DepthStencilState' object.

**Code:** Custom depth and stencil state

```cs
// depth test is enabled but it doesn't write
var depthStencilStateDescription = new DepthStencilStateDescription(true, false);
var depthStencilState = DepthStencilState.New(GraphicsDevice, depthStencilStateDescription);
 
// setting the depth state and the stencil with a reference value of 0
GraphicsDevice.SetDepthStencilState(depthStencilState);
 
// setting the depth state and the stencil with a reference value of 2
GraphicsDevice.SetDepthStencilState(depthStencilState, 2);```


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
GraphicsDevice.SetBlendState(GraphicsDevice.BlendStates.Opaque);```


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
GraphicsDevice.SetBlendState(blendState, Color4.White, 2);```


# Viewport states

When the user sets a render target or a depth buffer, the viewport is reset to its full size. However, the user might want to use a custom viewport. To achieve this, Xenko provides a method which, as a result, should be called **after** the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.SetRenderTarget' method.

The user can set the viewport thanks to the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.SetViewport' methods and the @'SiliconStudio.Xenko.Graphics.Viewport' class. The origin of a viewport is at the top left of the screen. The user can also specify which viewport to update.

**Code:** Setting the viewports

```cs
// example of a full HD buffer
GraphicsDevice.SetRenderTarget(GraphicsDevice.BackBuffer); // viewport automatically set at 1920*1080
 
// example of setting the viewport to have a 10 pixel border around the image in a full hd buffer (1920x1080)
var viewport = new Viewport(10, 10, 1900, 1060);
GraphicsDevice.SetViewport(viewport);
GraphicsDevice.SetViewport(0, viewport);```


# Scissor states

The @'SiliconStudio.Xenko.Graphics.GraphicsDevice.SetScissorRectangles' method is available to set the scissor. Contrary to the viewport, the user must provide the coordinates of the location of the vertices defining the scissor instead of its size. The method can be invocked with a @'SiliconStudio.Core.Mathematics.Rectangle' object in order to support multiple scissors.

**Code:** Setting the scissor

```cs
// example of setting the scissor to crop the image by 10 pixel around it in a full hd buffer (1920x1080)
GraphicsDevice.SetScissorRectangles(10, 10, 1910, 1070);
 
var rectangles = new[] { new Rectangle(10, 10, 1900, 1060), new Rectangle(0, 0, 256, 256) };
GraphicsDevice.SetScissorRectangles(rectangles);```


