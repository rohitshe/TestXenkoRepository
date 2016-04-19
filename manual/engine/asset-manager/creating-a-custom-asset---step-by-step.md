# Creating a custom asset - step by step

> **Note**
> 
> This tutorial is little bit outdated, and will be improved later.    

Xenko allows to integrate new assets into the asset pipeline.

In Xenko, an asset can have 3 representations at different time on a project:

- **Design representation**: contains end-user properties or parameters of an asset
- **Storage representation**: contains the binary representation of an asset. Optimized for loading at game time.
- **InGame** **representation**: contains the runtime representation of an asset loaded at game time. It is instantiated from the storage representation. We can have different representation of an asset at runtime from a same storage representation.




![media/9503662.png](media/9503662.png) 




In the following section, we are going to explain the step to create a custom asset, and how to integrate it into the asset pipeline from design time to using it at runtime.

# **Creating a custom Asset, step by step**

An asset is often represented differently at design time than at runtime, though they sometimes can share some of their datas.

In Xenko, an asset should be defined in a different assembly depending on its representation:

- The design representation of an asset should be stored in a **design time assembly**, meaning that this assembly won't be accessible at runtime.
- The storage and runtime representation of an asset should be stored in a **ingame assembly,** meaning that this assembly will be accessible at both design time and game time.

Suppose that we want to create an asset called `RangeAsset` that will generate an array of float based on some generator parameters (From, To, Step).

## Create an in-game project/assembly and design time assembly

We need to create two separate project assembly:

- create a project assembly that will contain our **ingame asset classes**
- create a project assembly that will contain our **design time asset classes**

> **Warning**
> 
> TODO: we should provide a VS wizard/template to create this kind of project    

## Create the in-game asset

In the ingame assembly, we are going to create the asset representation of the storage `RangeValues` that will be used by the game.

The class could be implemented like this:

```cs
[DataContract("RangeValues")] // Specify that this classes is serializable
[ContentSerializer(typeof(DataContentSerializer<RangeValues>))]  // Specify that this class is serializable through the asset manager
public partial class RangeValues
{
    public RangeValues()
    {
        Values = new List<float>();
    }
    public List<float> Values { get; set; }
}
```


## Create the design-time asset

In the design-time project (RangeAssetLib) , we are going to create 2 classes:

- The asset description in itself `RangeAsset` that describes parameters of the asset
- The asset compiler `CustomAssetCompiler` that will describe how to generate `RangeValues` based on `RangeAsset` description.

The `RangeAsset` must inherit from @'SiliconStudio.Assets.Asset'class in order to work under the Asset pipeline:

```cs
[DataContract("HeightMap")] // Name of the Asset serialized in YAML
[AssetFileExtension(".xkrange")] // The extension file used
[AssetCompiler(typeof(RangeAssetCompiler))] // The compiler used to transform this asset to RangeValues
[AssetDescription("RangeAsset ", "A RangeAsset")] // A description used to display in the asset editor
public class RangeAsset : Asset
{
    public float From {get; set;}
    public float To {get; set;}
    public float Step {get; set;}
}
```


This asset is providing a compiler in order to transform it from design-time to game time

```cs
/// <summary>
/// Compiler to transform <see cref="RangeAsset"/> to <see cref="RangeValues"/>
/// </summary>
internal class RangeAssetCompiler : AssetCompilerBase<RangeAsset>
{
    protected override void Compile(AssetCompilerContext context, string urlInStorage, UFile assetAbsolutePath, RangeAsset asset, CompilerResult result)
    {
        result.BuildSteps = new ListBuildStep() { new RangeAssetCommand(urlInStorage, asset) };
    }
    /// <summary>
    /// Command used by the build engine to convert the asset
    /// </summary>
    private class RangeAssetCommand : AssetCommand<RangeAsset>
    {
        public RangeAssetCommand(string url, RangeAsset asset) : base(url, asset)
        {
        }
        protected override Task<ResultStatus> DoCommandOverride(ICommandContext commandContext)
        {
            var assetManager = new AssetManager();
            // Generate our data for in-game time
            var inGameAsset = new RangeValues();                
            for(float index = asset.From; index <= asset.To; index += asset.Step)
                inGameAsset.Values.Add(index);
            // Save in-game asset
            assetManager.Save(Url, inGameAsset);
            return Task.FromResult(ResultStatus.Successful);
        }
    }
}
```


# **Using a custom Asset, step by step**

Once the asset is defined at design time and can be used at compile time, we can integrate it into a Xenko project.

## Add a reference to the design-time assembly into the Xenko package

In order to compile our custom asset `RangeAsset`, the asset compiler must be able to load the asset from a YAML file and find the `RangeAssetCompiler` in order to compile it.

```cs
!Package
Id: 53FD85E9-4C8B-430D-8CD9-E01B97BC8F8B
...
Profiles:
    ...
        ProjectReferences:
            -   Id: CC235793-54DE-4ED6-9A2C-0CF2521CDF93
                Location: ../../RangeAssetLib/RangeAssetLib.csproj
                Type: Library
```


> **Warning**
> 
> This will later be part of GameStudio interface.    

## Add an asset file

We need to write a `RangeAsset` file in a YAML file with the extension `.xkrange` that will be loaded by the compiler in the MyGame solution:

**Code:** MyRangeAsset1.xkrange

```cs
!RangeAsset
From: 0
To: 10
Step: 1
```


> **Warning**
> 
> TODO: An asset file can also be create through the asset editor if it provides a factory. Explain how to create a factory    

## Loading the asset at game time

We simply have to use the AssetManager to load the asset at game time:

```cs
var rangeValues = Assets.Load<RangeValues>("MyRangeAsset1");
```


# Advanced usages

- It is possible to reference other assets from an asset an perform computation on it (using AssetReference to describe the link and loading the referenced asset through the AssetManager in the compiler)
- An asset stored can be loaded at runtime into different representation. For example, TextureAsset can be loaded at runtime as an Image (CPU) or a Texture (GPU).

# Other examples

For example, in Xenko, we have the @'SiliconStudio.Xenko.Assets.Texture.TextureAsset' that represents a Texture that can be loaded at runtime.

The design representation of a `TextureAsset` contains information about how to import a texture, source of the data, how to resize the texture, change the format...etc.

**Code:** A texture asset at design time stored in a xktex file

```cs
!Texture
Id: a03b1e55-227b-4e91-b4fa-f610ce4fb4c1
Source: ../sourceimages/checker.png
Width: 100
Height: 100
IsSizeInPercentage: true
ColorKeyColor: {R: 255, G: 0, B: 255, A: 255}
ColorKeyEnabled: false
Format: null
GenerateMipmaps: true
PremultiplyAlpha: true
```


The `TextureAsset` has a compiler that will save the texture using Image serializer. At runtime, this texture can be loaded through the asset manager using either an image or a texture.

 

