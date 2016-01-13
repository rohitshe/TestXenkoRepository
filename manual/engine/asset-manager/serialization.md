# Serialization

# High-level serialization (Assets and Chunks)

## Assets

User easily access data through their URL through the @'SiliconStudio.Core.Serialization.Assets.AssetManager'.

Internally, URL are mapped to a Chunk through the **Index**, and then the **Chunk** is loaded, with its possible sub-objects.

## Index

There is an **index** file, that maps URL to a chunk hash (since chunks are immutable, we can generate the hash of their content and use it as chunk key).

## Chunks

When stored on HDD, data is stored in a Chunk format.

When building assets, each chunk will be a separate file. However, at the end of the build, they will all be consolidated in an [Asset Bundle](asset-bundles.md), which is a collection of Chunks.

Each chunk is accessible through its Murmur hash (git-like immutable storage).

> **Tip**
> 
> If you want to implement your own chunk serializer, you can do so by inheriting @'SiliconStudio.Core.Serialization.Contents.ContentSerializerBase`1'.
> 
> The default one fallback to low-level serialization, but serializes @'SiliconStudio.Core.Serialization.ContentReference`1' in separate chunks.    

# Low-level serialization (binary stream)

Low-level serialization can serialize structures (and their references) in a single byte stream.

You can use the low-level serialization directly through the @'SiliconStudio.Core.Serialization.BinarySerializationWriter' and @'SiliconStudio.Core.Serialization.BinarySerializationReader' classes.

> **Tip**
> 
> If you want to implement your own serializer, you need to inherit from @'SiliconStudio.Core.Serialization.DataSerializer`1' and register it with a @'SiliconStudio.Core.Serialization.Serializers.DataSerializerAttribute' or a @'SiliconStudio.Core.Serialization.Serializers.DataSerializerGlobalAttribute' attribute.    

