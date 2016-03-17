# Effect Permutations

XKFX files need a set of parameters to produce a shader. In a real project, there are many shaders, so there are many sets of parameters. We also want to create and compile all these shaders at project compilation time. So we want to generate all the permutations of parameters we need in a project. This is done through xkfxlib files.

# Xkfxlib

## Syntax

A xkfxlib file is a simple representation of a tree where values are set at each node and the resulting set of parameters output at each leaf.

**Code:** Example of xkfxlib file

```cs
!EffectLibrary
Id: 00000000-0000-0000-0000-000000000000
BuildOrder: 1000
Root:
	Keys:
		Effect.Name: BasicEffect
	Children:
		-	Keys:
				MaterialAsset.UseParameters: true
				Mesh.UseParameters: true
		-	Keys:
				MaterialAsset.UseParameters: false
				Mesh.UseParameters: false
```


It is possible to specify a set of values using the following syntax:

**Code:** Specifying a set of values

```cs
MaterialAsset.UseParameters: [true, false]
 
or
 
Mesh.UseParameters:
	- true
	- false
```


Range of values is also available.

**Code:** Specifying a range of values

```cs
MyParameters.IntParam: !fxparam.range
	From: 0
	To: 16
	Step: 2
 
or
 
MyParameters.FloatParam: !fxparam.range
	From: 1.52
	To: 4.58
	Step: 0.67
```


Be sure to correctly tune your xkfxlib file so that not too many nor too few shaders are produced.

## Some important parameters

During the compilation of your project, the xkfxlib file will generate many permutation. However it is not the only source of parameters, meshes and materials also store some parameters.

In the previous examples, you can see 3 important keys:

- Effect.Name: name of the effect in the XKFX in case you have many effects.
- MaterialAsset.UseParameters: notify the asset compiler to take the parameters from the materials to extend the permutation
- Mesh.UseParameters: notify the asset compiler to take the parameters from the meshes to extend the permutation

## Example

**Code:** Example of XKFX

```cs
!EffectLibrary
Id: 00000000-0000-0000-0000-000000000000
BuildOrder: 1000
Root:
	Keys:
		Effect.Name: BasicEffect
	Children:
		-	Keys:
				MyParameters.IntParam0: !fxparam.range
					From: 0
					To: 4
					Step: 2
				MyParameters.IntParam1: 5
		-	Keys:
				MyParameters.UseSkinning: [true, false]
				Mesh.UseParameters: false
```


This code will produce 5 permutations.

**Code:** Produced permutations

```cs
Effect.Name=BasicEffect, MyParameters.IntParam0=0, MyParameters.IntParam1=5
Effect.Name=BasicEffect, MyParameters.IntParam0=2, MyParameters.IntParam1=5
Effect.Name=BasicEffect, MyParameters.IntParam0=4, MyParameters.IntParam1=5
Effect.Name=BasicEffect, MyParameters.UseSkinning=true, Mesh.UseParameters=false
Effect.Name=BasicEffect, MyParameters.UseSkinning=false, Mesh.UseParameters=false
```


