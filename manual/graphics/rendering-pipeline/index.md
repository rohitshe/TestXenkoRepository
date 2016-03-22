# Rendering pipeline

Xenko's rendering pipeline aims to provide a maximum in both, performance and extensibility. It is designed around the following key concepts.

# Render Features

Rendering code is divided into @'SiliconStudio.Xenko.Rendering.RenderFeature's, each of which is responsible for processing a single type of @'SiliconStudio.Xenko.Rendering.RenderObject'.
Features are executed in multiple phases: **Collect**, **Extract**, **Prepare** and **Draw**. This allows to isolate and apply optimizations to each step of the pipeline separately.

See [Render features](render-features.md) for more details.

# Render Views

Scenes can be rendered from multiple @'SiliconStudio.Xenko.Rendering.RenderView's. Examples are shadow views, which are rendered from the view point of a light, or separate views per player.

Views are a first-class concept, and available to all rendering-phases, allowing batching across multiple views.

# Render Stages

The **Draw**-phase is executed a series of times, each time with combination of a @'SiliconStudio.Xenko.Rendering.RenderStage' and a @'SiliconStudio.Xenko.Rendering.RenderView'.
A stages is used to select the [effect](../effects-and-shaders/index.md) and [pipeline state](../low-level-api/pipeline-state.md) per object, as well as to define the output of the current pass.

// TODO: Graphics compositor

See [Render stages](render-stages.md) for more details.

# Visibility

@'SiliconStudio.Xenko.Rendering.RenderObject's are registered with a @'SiliconStudio.Xenko.Rendering.VisibilityGroup'. During the **Collect**-phase the visibility group performs
customizable culling and filtering based on the @'SiliconStudio.Xenko.Rendering.RenderView', @'SiliconStudio.Xenko.Rendering.RenderStage', etc.

See [Render object collection](render-object-collection.md) for more details.
