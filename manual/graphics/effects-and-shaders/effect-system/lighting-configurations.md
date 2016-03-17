# Lighting configurations

A lighting configuration describes the counts and types of lights and shadows that a mesh can support.

# Forward lighting - xklightconf to generate lighting configuration

Each mesh can have multiple lighting configurations. The engine is able to select the most effective one at runtime depending on the the current scene. xklightconf files should be used to generate these lighting configurations. They are similar to xkfxlib (See [Effect Permuations](effect-permutations.md)), however they only consider the following keys:

- `LightingKeys.MaxDirectionalLights`: number of directional lights
- `LightingKeys.MaxPointLights`: number of point lights
- `LightingKeys.MaxSpotLights`: number of spot lights
- `ShadowMapParameters.ShadowMapCount`: number of shadow maps
- `ShadowMapParameters.ShadowMapCascadeCount`: number of cascade per shadow map
- `ShadowMapParameters.ShadowConfigurations`: configuration of shadows. It is an array containing `ShadowConfiguration` structures. In each `ShadowConfiguration`, you can define
  - `ShadowCount`: number of shadows
  - `LevelCount`: number of cascades per shadow
  - `FilterType`: shadow filtering (Nearest - Pcf & Variance to come)

Any other key will not be taken into account (but there will be no generated error or warning).

With this information, all the necessary shaders will be built at compile time.

**Code:** Example of xklightconf file

```cs
!LightingConfiguration
Id: ab903105-47e4-43ba-8253-70788007b203
BuildOrder: 1000
Tags: []
Permutations:
    Keys:
        Lighting.MaxDirectionalLights: !fxparam.range
            From: 0
            To: 2
        Lighting.MaxPointLights:
            - 0
            - 1
        Lighting.MaxSpotLights:
            - 0
            - 1
    Children:
        -   Keys:
                ShadowMapParameters.ShadowConfigurations:
                    - !ShadowConfigurationArray
                        Groups:
                            -   ShadowCount: 1
                                LevelCount: 4
                                FilterType: Nearest
                    - !ShadowConfigurationArray
                        Groups:
                            -   ShadowCount: 1
                                LevelCount: 4
                                FilterType: Nearest
                            -   ShadowCount: 1
                                LevelCount: 4
                                FilterType: Nearest
            Children: []
        -   Keys: {}
            Children: []
```


Once it is created, you can attach this configuration to your mesh through the `LightingParameters` field. Since it is an asset, you can share configurations with multiple meshes.

# Deferred lighting

In deferred lighting, the configuration is shared across all the meshes. In fact deferred lighting needs some predefined shaders. Please add this permutation to your **xkfxlib**.

**Code:** Deferred Lighting permutation

```cs
Permutations:
    Keys: {}
    Children:
        -   Keys:
                Effect.Name:{your DeferredEffect}
                MaterialAsset.UseParameters: true
                Mesh.UseParameters: true
                RenderingParameters.UseDeferred: true
            Children: []
        -   Keys:
                Effect.Name: {your LightingPrepassEffect}
                ShadowMapParameters.FilterType:
                    - !ShadowMapFilterType Nearest
                    - !ShadowMapFilterType PercentageCloserFiltering
                ShadowMapParameters.ShadowMapCascadeCount: !fxparam.range
                    From: 1
                    To: 4
            Children: []
```


**NOTE**: this will be done automatically in a later release.

