# Shadows optimizations

The technique used to render shadows is called shadow mapping and consist in rendering to a depth texture from the point-of-view of the light all potential occluders, and then reuse this information to render the shadows along the rendering of the scene.

Internally, the engine is using a shadow map atlas texture for directional and spot lights.

Each lights with an enabled shadow gets a region of this map atlas texture.

The size of this region depends on several factors:

- The `shadowMapSizeFactor` based on the `LightShadowMap.Size` property:

| Shadow Size | Shadow Importance Factor             |
| ----------- | ------------------------------------ |
| XLarge      | 2.0                                  |
| Large       | 1.0 (default for directional lights) |
| Medium      | 0.5 (default for spot lights)        |
| Small       | 0.25 (default for omni lights)       |
| XSmall      | 0.125                                |


- The projected size of the light in screen-space (`lightSize`). 
  - For the directional light, the lightSize is equal to the max(screenWidth, screenHeight)
  - For the spot light, the lightSize is equal to the projection of the projected sphere at target spot light cone
- The `ShadowMapBaseSize` equals to `1024` (In 1.1.x-beta, it is currently hardcoded, but it will use a dynamic value based on the shader profile / GPU memory)

 

The final size of the shadow map is then calculated like this:

```cs
// Calculate the size factor
var shadowMapSizeFinalFactor = shadowImportanceFactor * shadowMapSizeFactor;
// Multiply the light projected size by the size factor
var shadowMapSize = NextPowerOfTwo(lightSize * shadowSizeFinalFactor);
// Clamp to a maximum size
shadowMapSize = min(shadowMapSize, ShadowMapBaseSize  * shadowSizeFinalFactor);
```


