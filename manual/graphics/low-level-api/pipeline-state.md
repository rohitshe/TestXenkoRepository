# Pipeline states

Xenko offers total control over the graphics pipeline state. This includes:

- Rasterizer state
- Depth and stencil state
- Blend state
- Effects
- Input layout
- Output description

State is compiled into immutable @'SiliconStudio.Xenko.Graphics.PipelineState' objects, which describe the whole pipline.
They are then bound using a @'SiliconStudio.Xenko.Graphics.CommandList'.

**Code:** Creating state objects

```cs
// Creating pipeline state object
var pipelineStateDescription = new PipelineStateDescription();
var pipelineState = PipelineState.New(GraphicsDevice, ref pipelineStateDescription);
 
// Applying the state to the pipeline
CommandList.SetPipelineState(pipelineState);
```

For convenience, the @'SiliconStudio.Xenko.Graphics.MutablePipelineState' class is provided. It allows to set states independently, while caching the underlying pipeline states.

**Code:** Mutable pipeline state

```cs
// Creating the pipeline state object
var mutablePipelineState = new MutablePipelineState();

// Setting values and rebuilding
mutablePipelineState.State.BlendState = BlendStates.AlphaBlend
mutablePipelineState.Update
 
// Applying the state to the pipeline
CommandList.SetPipelineState(mutablePipelineState.CurrentState);
```

# Rasterizer state

The rasterizer can be set using the @'SiliconStudio.Xenko.Graphics.PipelineStateDescription.RasterizerState' property. A set of predefined descriptions is held by the @'SiliconStudio.Xenko.Graphics.RasterizerStates' class. They deal with the cull mode, and should be enough for most use cases:

- @'SiliconStudio.Xenko.Graphics.RasterizerStates.CullNone': no culling
- @'SiliconStudio.Xenko.Graphics.RasterizerStates.CullFront': front-face culling
- @'SiliconStudio.Xenko.Graphics.RasterizerStates.CullBack': back-face culling

**Code:** Setting the cull mode

```cs
pipelineStateDescription.RasterizerState = RasterizerStates.CullNone;
pipelineStateDescription.RasterizerState = RasterizerStates.CullFront;
pipelineStateDescription.RasterizerState = RasterizerStates.CullBack;
```


You can, however, create your own custom state. Please refer to the @'SiliconStudio.Xenko.Graphics.RasterizerStateDescription' documentation to find the complete list of available options and default values.

**Code:** Custom rasterizer states

```cs
var rasterizerStateDescription = new RasterizerStateDescription(CullMode.Front);
rasterizerStateDescription.ScissorTestEnable = true; // enables the scissor test
pipelineStateDescription.RasterizerState = rasterizerStateDescription;
```


# Depth and stencil states

The @'SiliconStudio.Xenko.Graphics.PipelineStateDescription.DepthStencilState' property contains the depth and stencil states. A set of commonly used states is defined by the @'SiliconStudio.Xenko.Graphics.DepthStencilStates' class:

- @'SiliconStudio.Xenko.Graphics.DepthStencilStates.Default': depth read and write with a less-than comparison
- @'SiliconStudio.Xenko.Graphics.DepthStencilStates.DefaultInverse': read and write with a greater-or-equals comparison
- @'SiliconStudio.Xenko.Graphics.DepthStencilStates.DepthRead': read only with a less-than comparison
- @'SiliconStudio.Xenko.Graphics.DepthStencilStates.None': neither read nor write

**Code:** Setting the depth state

```cs
pipelineStateDescription.DepthStencilState = DepthStencilStates.Default;
pipelineStateDescription.DepthStencilState = DepthStencilStates.DefaultInverse;
pipelineStateDescription.DepthStencilState = DepthStencilStates.DepthRead;
pipelineStateDescription.DepthStencilState = DepthStencilStates.None;
```


If required, the user can set custom depth and stencil states. Please refer to @'SiliconStudio.Xenko.Graphics.DepthStencilStateDescription' for the complete list of available options and default values.

**Code:** Custom depth and stencil state

```cs
// depth test is enabled but it doesn't write
var depthStencilStateDescription = new DepthStencilStateDescription(true, false);
pipelineStateDescription.DepthStencilState = depthStencilStateDescription;
```

The stencil reference is not part of the @'SiliconStudio.Xenko.Graphics.PipelineState' and is set using @'SiliconStudio.Xenko.Graphics.CommandList.SetStencilReference'.

**Code:** Setting the stencil reference

```cs
CommandList.SetStencilReference(2);
```


# Blend state

The @'SiliconStudio.Xenko.Graphics.PipelineStateDescription.BlendState' and @'SiliconStudio.Xenko.Graphics.PipelineStateDescription.SampleMask' properties control blending.
A set of commonly used blend states is defined by the @'SiliconStudio.Xenko.Graphics.BlendStates' class:

- @'SiliconStudio.Xenko.Graphics.BlendStates.Additive': sums the colors 
- @'SiliconStudio.Xenko.Graphics.BlendStates.AlphaBlend': sums the colors using the alpha of the source on the destination color
- @'SiliconStudio.Xenko.Graphics.BlendStates.NonPremultiplied': sums using the alpha of the source on both colors
- @'SiliconStudio.Xenko.Graphics.BlendStates.Opaque': replaces the color

**Code:** Setting the blend state

```cs
// Set common blend states
pipelineStateDescription.BlendState = BlendStates.Additive;
pipelineStateDescription.BlendState = BlendStates.AlphaBlend;
pipelineStateDescription.BlendState = BlendStates.NonPremultiplied;
pipelineStateDescription.BlendState = BlendStates.Opaque;

// Set the sample mask
pipelineStateDescription.SampleMask = 0xFFFFFFFF;
```

If required, the user can set custom depth and blend states. Please refer to @'SiliconStudio.Xenko.Graphics.BlendStateDescription' for the complete list of available options and default values.

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
pipelineStateDescription.BlendState = blendStateDescription;
```

The blend factor is not part of the @'SiliconStudio.Xenko.Graphics.PipelineState' and is set using @'SiliconStudio.Xenko.Graphics.CommandList.SetBlendFactor'.

**Code:** Setting the blend factor

```cs
CommandList.SetBlendFactor(Color4.White);
```

# Effects

The pipeline state also includes the shaders that you want to use for drawing.
To bind an @'SiliconStudio.Xenko.Graphics.Effect' to the pipeline, simply set the @'SiliconStudio.Xenko.Graphics.PipelineStateDescription.EffectBytecode'
and @'SiliconStudio.Xenko.Graphics.PipelineStateDescription.RootSignature' properties of the @'SiliconStudio.Xenko.Graphics.PipelineStateDescription' to the matching values of the effect.

An @'SiliconStudio.Xenko.Shaders.EffectBytecode' contains the actual shader programs. For more information on effect, please refer to [Effects and Shaders](../effects-and-shaders/index.md).

The @'SiliconStudio.Xenko.Graphics.RootSignature' describes the number and kind of resources that are expected by the effect. The next chapter covers how to [bind resources](resources.md) to the pipeline.

**Code:** Binding an effect

```cs
var effect = EffectSystem.LoadEffect("MyEffect").WaitForResult();
pipelineStateDescription.EffectBytecode = effect.Bytecode;
pipelineStateDescription.RootSignature = effect.RootSignature;
```

# Input layout

The pipeline state describes the layout in which vertices are sent to the device through the @'SiliconStudio.Xenko.Graphics.PipelineStateDescription.InputElements' and @'SiliconStudio.Xenko.Graphics.PipelineStateDescription.PrimitiveType' properties.

The chapter on [drawing vertices](draw-vertices.md) describes how to create custom vertex buffers and their @'SiliconStudio.Xenko.Graphics.VertexDeclaration' in more detail.

**Code:** Setting an input layout

```cs
VertexDeclaration vertexDeclaration = ...
pipelineStateDescription.InputElements = vertexDeclaration.CreateInputElements();
pipelineStateDescription.PrimitiveType = PrimitiveType.TriangleStrip;
```

# Output description

Finally, the @'SiliconStudio.Xenko.Graphics.PipelineStateDescription.Output' property of the @'SiliconStudio.Xenko.Graphics.PipelineStateDescription' defines the number and @'SiliconStudio.Xenko.Graphics.PixelFormat' of all bound render targets.

Please refer to [Textures and render targets](textures-and-render-targets.md) for details on how to actually bind render targets to the pipeline.

**Code:** Creating an output description

```cs
var renderOutputDescription = new RenderOutputDescription(GraphicsDevice.Presenter.BackBuffer.Format, GraphicsDevice.Presenter.DepthStencilBuffer.Format);
pipelineStateDescription.Output = renderOutputDescription;
```

For convenience, the @'SiliconStudio.Xenko.Graphics.RenderOutputDescription.CaptureState(SiliconStudio.Xenko.Graphics.CommandList)' method can be used to retrieve the output description, that was last set on a @'SiliconStudio.Xenko.Graphics.CommandList'.
This is especially useful in combination with @'SiliconStudio.Xenko.Graphics.MutablePipelineState', when the render target might not be known up front.

**Code:** Capturing output desciption

```cs
mutablePipelineState.State.Output.CaptureState(CommandList);
mutablePipelineState.Update();
```
