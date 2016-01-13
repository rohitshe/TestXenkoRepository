# Effect Language

# Create shader in C#

You can create a shader at runtime with children of the @'SiliconStudio.Xenko.Shaders.ShaderSource' class. There are 3 of them:

- @'SiliconStudio.Xenko.Shaders.ShaderClassSource': corresponding to a unique class
- @'SiliconStudio.Xenko.Shaders.ShaderMixinSource': mix several @'SiliconStudio.Xenko.Shaders.ShaderSource', setting preprocessor values, defining compositions
- @'SiliconStudio.Xenko.Shaders.ShaderArraySource': used for arrays of compositions

This method will produce shaders at runtime. However, many platforms do not support HLSL and do not have the ability to compile shaders at runtime. Futhermore, this approach do not benefit from the reusability of the mixins. 

# Xenko Effect (XKFX)

Many shaders are small variations of pre-existing ones. For example, some meshes will cast shadows, some will receive them, some will need skinning etc. It can be useful to have a simple way to describe how to modify a shader from simple flags such as "I need skinning". In many projects, über shaders are the rule. Given a small set of preprocessor parameters, they will produce a unique shader. We wanted to offer the same kind of control. Since XKSL is built around the idea of simple blocks of code to mix with another ones, you just want to add the mixins that will handle the expected variation, the whole mix work being handled by the shader mixer. This feature is offered through Xenko Effect (.XKFX) files.

## General syntax

A XKFX file is a small program used to generate a shader. It will take a set of parameters (key and value in a collection) and produce a `ShaderMixinSource` ready to compile.

**Code:** Example of XKFX file

```cs
using SiliconStudio.Xenko.Effects.Data;

namespace ParadoxEffects
{
	params MyParameters
	{
		bool EnableSpecular = true;
	};
	shader BasicEffect
	{
		using params MaterialParameters;
		using params MyParameters;

		mixin ShaderBase;
		mixin TransformationWAndVP;
		mixin NormalVSStream;
		mixin PositionVSStream;
		mixin BRDFDiffuseBase;
		mixin BRDFSpecularBase;
		mixin LightMultiDirectionalShadingPerPixel<2>;
		mixin TransparentShading;
		mixin DiscardTransparent;

		if (MaterialParameters.AlbedoDiffuse != null)
		{
			mixin compose DiffuseColor = ComputeBRDFDiffuseLambert;
			mixin compose albedoDiffuse = MaterialParameters.AlbedoDiffuse;
		}

		if (MaterialParameters.AlbedoSpecular != null)
		{
			mixin compose SpecularColor = ComputeBRDFColorSpecularBlinnPhong;
			mixin compose albedoSpecular = MaterialParameters.AlbedoSpecular;
		}
	};
}```


## Adding mixins

 To add a mixin, simply use `mixin <mixin_name>`.

## Using parameters

The syntax is similar to C# syntax. The following rules are added:

- when you use parameter keys, don't forget to add the using `params <class_name>`. Otherwise, keys will be treated as variables.
- no need to tell the program where to check the values behind the keys. Just use the key.

**Code:** Parameters

```cs
using params MaterialParameters;
 
if (MaterialParameters.AlbedoDiffuse != null)
{
	mixin MaterialParameters.AlbedoDiffuse;
}```


The parameters behave like any variable, you can read and write their value. They can be used to perform test, to set template values. Since some parameters store mixins, key can be used for compositions and inheritance too.

## Custom parameters

You can create your own set of parameters using a structure definition syntax. Even if they are defined in the XKFX file, don't forget the `using` statement when you want to use them.

**Code:** Custom parameters

```cs
params MyParameters
{
	bool EnableSpecular = true; // true is the default value
}```


## Compositions

To add a composition, simply assign the composition variable to your mixin. This is done with the following syntax.

**Code:** Compositions

```cs
// albedoSpecular is the name of the composition variable in the mixin
mixin compose albedoSpecular = ComputeColorTexture;
 
or
 
mixin compose albedoSpecular = MaterialParameters.AlbedoSpecular;```


## Partial shaders

It is also possible to break the code into sub mixins that can be reused elsewhere. This is done through the following syntax.

**Code:** Partial shader

```cs
partial shader MyPartialShader
{
	mixin ComputeColorMultiply;
	mixin compose color1 = ComputeColorStream;
	mixin compose color2 = ComputeColorFixed;
}
 
// to use it
mixin MyPartialShader;
mixin compose myComposition = MyPartialShader;```


You can now use the `MyPartialShader` mixin like any other mixin in the code.

