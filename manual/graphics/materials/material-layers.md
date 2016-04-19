# Material Layers

Material layers gives a powerful way to combine multiple materials to build more complex materials. 

 

![media/material-layers-1.png](media/material-layers-1.png) 

 

 

The following screenshot shows the blending of a rust material with a gold material.

![media/material-layers-2.png](media/material-layers-2.png) 

 

The following diagram shows the definition of a material blend above:

![media/material-layers-3.png](media/material-layers-3.png) 

| Property        | Description                                                                                                                              |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Material        | A reference to a material                                                                                                                |
| Blend Map       | A blend map [scalar provider](material-colors.md) that defines how much the current material is blend to the previous one (upper layer). |
|                 |                                                                                                                                          |
|                 | - A value of 0 means that the previous material in the layer is fully used                                                               |
|                 | - A value of 1 means that the material referenced by this layer is fully used                                                            |
|                 | - Values in-between 0 and 1 will generate a blend of a parameters of the previous material with the material defined by this layer       |
|                 |                                                                                                                                          |
|                 |                                                                                                                                          |
| Layer Overrides |                                                                                                                                          |
| - UV Scale      | A UV scale applied to all textures UV of the material of the layer (excluding the occlusion map)                                         |
|                 |                                                                                                                                          |
|                 |                                                                                                                                          |


> **Note**
> 
> - Individual attributes of layers are blend if they share the same shading model otherwise it is the result of the shading model that is blend.    
