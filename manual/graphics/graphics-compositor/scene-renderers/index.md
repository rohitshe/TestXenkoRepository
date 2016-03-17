# Scene Renderers

![images/scene-renderers-1.png](images/scene-renderers-1.png) 

 

Renderers can be added to a layer to perform a specific rendering operation in the scene graphics compositor pipeline.

# Common Properties

| Property | Description                                                                                                                                                   |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Output   | Defines which render frame to clear:                                                                                                                          |
|          |                                                                                                                                                               |
|          | - Current: The render frame defined at the Layer level (default)                                                                                              |
|          | - Master: The render frame defined at the compositor level (when the compositor is defined at the root level, the Master is bound to the window render frame) |
|          | - Shared: A render frame defined as an asset that can be shared with different scenes/renderers                                                               |
|          | - Direct: A manual render frame (accessible only by using the API)                                                                                            |
|          |                                                                                                                                                               |
|          |                                                                                                                                                               |


# Renderers

In the following table are the default renderer that are available:




- [Clear RenderFrame](clear-renderframe.md)
- [Render Camera](render-camera.md)
- [Render Effect](render-effect.md)
- [Render Child Scene](render-child-scene.md)





