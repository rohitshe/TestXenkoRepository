# Post-Effects For Developers

The following diagram shows the ImageEffects interfaces and implementation classes used to develop the post-processing effects:

- The interface @'SiliconStudio.Xenko.Rendering.Images.IImageEffect' is the root interface for implementing an arbitrary image effect.
- The interface @'SiliconStudio.Xenko.Rendering.IImageEffectRenderer' is the interface for implementing a post-effect that can be instantiated from the editor.
  - For example, @'SiliconStudio.Xenko.Rendering.Images.PostProcessingEffects' and @'SiliconStudio.Xenko.Rendering.Images.GaussianBlur' are inheriting from this interface and accessible from the editor
- The @'SiliconStudio.Xenko.Rendering.Images.ImageEffect' is a base implementation of the @'SiliconStudio.Xenko.Rendering.Images.IImageEffect'. It is recommended to derive from this class to develop a custom effect.

![media/post-effects-for-developers-1.png](media/post-effects-for-developers-1.png) 

 

 

