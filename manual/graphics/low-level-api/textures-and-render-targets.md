# Textures and render targets

Xenko uses the @'SiliconStudio.Xenko.Graphics.Texture' class to interacts with texture objects in the code.

# Loading a Texture

To load a texture from an asset in Xenko, simply calls this function:

**Code:** Loading a texture

```cs
// loads the texture called duck.dds (or .png etc.)
var myTexture = Asset.Load<Texture2D>("duck");
```


It will automatically generate a texture object with all of its field correctly filled.

# Creating a texture

The user can also create textures without any asset (for example to be used as render target). To create such a texture, simply call the constructor of the @'SiliconStudio.Xenko.Graphics.Texture' class. Please refer to the @'SiliconStudio.Xenko.Graphics.Texture' class reference to get the full list of available options and parameters. Some texture formats may not be available on all platforms.

**Code:** Creating a texture

```cs
var myTexture = Texture2D.New(GraphicsDevice, 512, 512, PixelFormat.R8G8B8A8_UNorm, TextureFlags.ShaderResource);
```


# Render targets

## Creating a render target

The @'SiliconStudio.Xenko.Graphics.GraphicsDevice' class always provides a default render and a depth buffer. They are accessible through the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.BackBuffer' and @'SiliconStudio.Xenko.Graphics.GraphicsDevice.DepthStencilBuffer' properties. However, the user might want to use his own buffer to perform off-screen rendering or post-processes. As a result, Xenko offers a simple way to create a render target and a depth buffer.

**Code:** Creating a custom render target and depth buffer

```cs
// render target
var myRenderTarget = Texture2D.New(GraphicsDevice, 512, 512, PixelFormat.R8G8B8A8_UNorm, TextureFlags.ShaderResource | TextureFlags.RenderTarget).ToRenderTarget();
 
// writable depth buffer
var myDepthBuffer = Texture2D.New(GraphicsDevice, 512, 512, PixelFormat.D16_UNorm, TextureFlags.DepthStencil).ToDepthStencilBuffer(false);
```


Do not forget the flag TextureFlags.RenderTarget to enable the render target behavior.

Make sure that the PixelFormat is correct, especially for the depth buffer. Be also careful of the available formats on the target platform!

## Using a render target

Once these buffers are created, the user can easily set them are the current render targets.

**Code:** using a render target

```cs
// settings the render targets
GraphicsDevice.SetRenderTarget(myDepthBuffer, myRenderTarget);
 
// setting the default render target
GraphicsDevice.SetRenderTarget(GraphicsDevice.DepthStencilBuffer, GraphicsDevice.BackBuffer);
```


Also make sure that both the render target and the depth buffer have the same size. Otherwise, the depth buffer will not be used.

It is possible to set multiple render targets at the same time. The user must call the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.SetRenderTargets' method.

Note that only the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.Backbuffer' is displayed on screen, so rendering in it is mandatory to display something.

## Clearing a render target

To clear a render target, the user must call the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.Clear' method.

**Code:** Clearing the targets

```cs
GraphicsDevice.Clear(GraphicsDevice.BackBuffer, Color.Black);
GraphicsDevice.Clear(GraphicsDevice.DepthStencilBuffer, DepthStencilClearOptions.DepthBuffer); // only clear the depth buffer
```


The user must not forget to clear the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.Backbuffer' and the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.DepthStencilBuffer' at each frame because it can have unexpected behavior depending on the device. If the user wants to keep the content of a frame, he should use an intermediate render target.

