# Low-level API

The @'SiliconStudio.Xenko.Graphics.GraphicsDevice' class is the central class for displaying the application. It is used to create resources and present images on the screen.
It is accessible as a member of the @'SiliconStudio.Xenko.Engine.Game' and @'SiliconStudio.Xenko.Engine.ScriptComponent' classes.

Actions like drawing, setting graphics states and using resources, are recorded using @'SiliconStudio.Xenko.Graphics.CommandList' objects, for later execution by the device.
Many command lists can be filled at the same time, for example one per thread. A default command list is available as member of the @'SiliconStudio.Xenko.Games.GameBase.GraphicsContext' of your @'SiliconStudio.Xenko.Engine.Game'.

In methods, these objects are typically provided through contexts such as @'SiliconStudio.Xenko.Rendering.RenderContext' and @'SiliconStudio.Xenko.Rendering.RenderDrawContext'.

To successfully perform any drawing, multiple steps are required. These include:

- Setting textures as [render targets](textures-and-render-targets.md), clearing them, and seting viewports and scissors
- Setting up the graphics [pipeline state](pipeline-state.md), including input description, shaders, depth-stencil, blending, rasterizer, etc.
- [Binding resources](resources.md), such as constant buffers and textures
- [Drawing any sets of vertices](draw-vertices.md) using built-in primitives or custom vertex buffers
