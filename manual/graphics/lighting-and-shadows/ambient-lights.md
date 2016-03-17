# Ambient Lights

# Overview

An ambient light is a uniform light with no direction.

![images/AmbientLightOverview.png](images/AmbientLightOverview.png) 

An object is lit uniformly:

| Ambient Lighting                                     |
| ---------------------------------------------------- |
| ![images/AmbientLight.png](images/AmbientLight.png)  |
|                                                      |
| Material Pure Diffuse                                |


# Properties

Properties that defines a ambient light:

![images/AmbientLightProperties.png](images/AmbientLightProperties.png) 

 

| Property     | Description                                                                                                                                                                                    |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Type         | Ambient                                                                                                                                                                                        |
| Color        | The color of this point light                                                                                                                                                                  |
|              |                                                                                                                                                                                                |
|              | *Note Currently, the light support an RGB color but will provide also temperature colors.*                                                                                                     |
| Intensity    | The intensity of this light. The color sampled from the skybox is basically multiplied by this value and the intensity defined in the Skybox Component before sending the color to the shader. |
|              |                                                                                                                                                                                                |
|              | Hence the final color is defined as:                                                                                                                                                           |
|              |                                                                                                                                                                                                |
|              | `final Color = Skybox Sampler Color * Light Skybox Intensity * Skybox Intensity`                                                                                                               |
|              |                                                                                                                                                                                                |
|              | *Note: Currently, this value has no units as it is dependent on the values coming from the skybox.*                                                                                            |
| Culling Mask | Defines which entity groups are affected by this light. By default, all groups are affected.                                                                                                   |


 

 

 

 

