# Resource binding

When [drawing vertices](draw-vertices.md) using an [effect](../effects-and-shaders/index.md), the shaders will expect certain resources to be available. These include:

- Texture and buffers
- Samplers
- Constant buffers

# Automatic resource binding

For convenience the engine provides the @'SiliconStudio.Xenko.Rendering.EffectInstance' class to handle the details of enumerating these resources from a loaded effect as well as binding them.

It exposes the @'SiliconStudio.Xenko.Graphics.RootSignature', which has to be set as [pipeline state](pipeline-state.md),
and allows to fill constant buffers and bind resources based on a @'SiliconStudio.Xenko.Rendering.ParameterCollection'.

**Code:** Using an EffectInstance

```cs
// Create a EffectInstance and use it to set up the pipeline
var effectInstance = new EffectInstance(EffectSystem.LoadEffect("MyEffect").WaitForResult());
pipelineStateDescription.EffectBytecode = effectInstance.Effect.Bytecode;
pipelineStateDescription.RootSignature = effectInstance.RootSignature;

// Update constant buffers and bind resources
effectInstance.Apply(context.GraphicsContext);
```

# Manual resource binding

When more optimized code is required, e.g. in many parts of Xenko's [rendering pipeline](../rendering-pipeline/index.md), constant buffers updates and resource binding can be done manually.

// TODO: ResourceLayouts, Descriptors, DescriptorSets