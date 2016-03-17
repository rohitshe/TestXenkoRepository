# Clear RenderFrame

Clears a render frame. 

![images/clear-renderframe-1.png](images/clear-renderframe-1.png) 

# Properties

| Property      | Description                                                                                                              |
| ------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Clear Flags   | Specify what to clear in the render frame:                                                                               |
|               |                                                                                                                          |
|               | - Color: Clears both the Color and Depth                                                                                 |
|               | - DepthOnly: Clears only the Depth                                                                                       |
|               |                                                                                                                          |
|               |                                                                                                                          |
| Color         | A color used to clear the color texture of the render frame, only valid when **Clear Flags** property is set to `Color`. |
| Depth Value   | The depth value used to clear the depth texture of the render frame.                                                     |
| Stencil Value | The stencil value used to clear the stencil texture of the render frame                                                  |
| Output        | See Common properties in [Scene Renderers](index.md)                                                                     |


 

 

 

