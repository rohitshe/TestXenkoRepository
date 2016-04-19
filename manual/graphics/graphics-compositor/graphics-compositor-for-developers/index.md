# Graphics Compositor For Developers

The following diagram shows the Scene Graphics Compositor by Layers interfaces and implementation classes:

- The @'SiliconStudio.Xenko.Rendering.Composers.SceneGraphicsCompositorLayers' is the default implementation of @'SiliconStudio.Xenko.Rendering.Composers.ISceneGraphicsCompositor' and provides a layered based compositor system
- It contains a collection of cameras slots
- A collection of @'SiliconStudio.Xenko.Rendering.Composers.SceneGraphicsLayer'
- Each layer contains a collection of @'SiliconStudio.Xenko.Rendering.ISceneRenderer'

![media/graphics-compositor-for-developers-1.png](media/graphics-compositor-for-developers-1.png) 

# Renderers

The default renderers implementing the @'SiliconStudio.Xenko.Rendering.ISceneRenderer'

- @'SiliconStudio.Xenko.Rendering.SceneCameraRenderer' to render the current scene from a camera
- @'SiliconStudio.Xenko.Rendering.SceneDelegateRenderer' to delegate the rendering to a method callback
- @'SiliconStudio.Xenko.Rendering.ClearRenderFrameRenderer' to clear the colors/depth of a render frame
- @'SiliconStudio.Xenko.Rendering.SceneEffectRenderer'to apply an image effect to a render frame
- @'SiliconStudio.Xenko.Rendering.SceneChildRenderer'to render a child scene to a render frame

 

![media/graphics-compositor-for-developers-2.png](media/graphics-compositor-for-developers-2.png) 

 

