# Textures and render targets

Xenko uses the @'SiliconStudio.Xenko.Graphics.Texture' class to interact with texture objects in code.

# Loading a Texture

To load a texture from an asset in Xenko, simply calls this function:

**Code:** Loading a texture

```cs
// loads the texture called duck.dds (or .png etc.)
var myTexture = Content.Load<Texture>("duck");
```


It will automatically generate a texture object with all of its field correctly filled.

# Creating a texture

The user can also create textures without any asset (for example to be used as render target). To create such a texture, simply call the constructor of the @'SiliconStudio.Xenko.Graphics.Texture' class. Please refer to the @'SiliconStudio.Xenko.Graphics.Texture' class reference to get the full list of available options and parameters. Some texture formats may not be available on all platforms.

**Code:** Creating a texture

```cs
var myTexture = Texture.New2D(GraphicsDevice, 512, 512, false, PixelFormat.R8G8B8A8_UNorm, TextureFlags.ShaderResource);
```


# Render targets

## Creating a render target

The @'SiliconStudio.Xenko.Graphics.GraphicsPresenter' class always provides a default render target and a depth buffer. They are accessible through the @'SiliconStudio.Xenko.Graphics.GraphicsPresenter.BackBuffer' and @'SiliconStudio.Xenko.Graphics.GraphicsPresenter.DepthStencilBuffer' properties. The presenter is exposed by the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.Presenter' property of the @'SiliconStudio.Xenko.Graphics.GraphicsDevice'. However, the user might want to use his own buffer to perform off-screen rendering or post-processes. As a result, Xenko offers a simple way to create textures that can act as render targets and a depth buffers.

**Code:** Creating a custom render target and depth buffer

```cs
// render target
var myRenderTarget = Texture.New2D(GraphicsDevice, 512, 512, false, PixelFormat.R8G8B8A8_UNorm, TextureFlags.ShaderResource | TextureFlags.RenderTarget);
 
// writable depth buffer
var myDepthBuffer = Texture.New2D(GraphicsDevice, 512, 512, false, PixelFormat.D16_UNorm, TextureFlags.DepthStencil);
```


Do not forget the flag @'SiliconStudio.Xenko.Graphics.TextureFlags.RenderTarget' to enable the render target behavior.

Make sure that the PixelFormat is correct, especially for the depth buffer. Be also careful of the available formats on the target platform!

## Using a render target

Once these buffers are created, they can easily be set as the current render targets.

**Code:** using a render target

```cs
// settings the render targets
CommandList.SetRenderTargetAndViewport(myDepthBuffer, myRenderTarget);
 
// setting the default render target
CommandList.SetRenderTargetAndViewport(GraphicsDevice.Presenter.DepthStencilBuffer, GraphicsDevice.Presenter.BackBuffer);
```

Also make sure that both the render target and the depth buffer have the same size. Otherwise, the depth buffer will not be used.

It is possible to set multiple render targets at the same time. See the overloads of @'SiliconStudio.Xenko.Graphics.CommandList.SetRenderTargets(SiliconStudio.Xenko.Graphics.Texture,SiliconStudio.Xenko.Graphics.Texture[])' and @'SiliconStudio.Xenko.Graphics.CommandList.SetRenderTargetsAndViewport(SiliconStudio.Xenko.Graphics.Texture,SiliconStudio.Xenko.Graphics.Texture[])' method.

Note that only the @'SiliconStudio.Xenko.Graphics.GraphicsPresenter.BackBuffer' is displayed on screen, so rendering in it is mandatory to display something.

## Clearing a render target

To clear render targets, call the @'SiliconStudio.Xenko.Graphics.CommandList.Clear(SiliconStudio.Xenko.Graphics.Texture,SiliconStudio.Core.Mathematics.Color4)' and @'SiliconStudio.Xenko.Graphics.CommandList.Clear(SiliconStudio.Xenko.Graphics.Texture,SiliconStudio.Xenko.Graphics.DepthStencilClearOptions,System.Single,System.Byte)' methods.

**Code:** Clearing the targets

```cs
CommandList.Clear(GraphicsDevice.Presenter.BackBuffer, Color.Black);
CommandList.Clear(GraphicsDevice.Presenter.DepthStencilBuffer, DepthStencilClearOptions.DepthBuffer); // only clear the depth buffer
```


Don't forget to clear the @'SiliconStudio.Xenko.Graphics.GraphicsPresenter.BackBuffer' and the @'SiliconStudio.Xenko.Graphics.GraphicsPresenter.DepthStencilBuffer' each frame, because it can result in unexpected behavior depending on the device. If you want to keep the contents of a frame, you should use an intermediate render target.



# Viewport

@'SiliconStudio.Xenko.Graphics.CommandList.SetRenderTargetAndViewport(SiliconStudio.Xenko.Graphics.Texture,SiliconStudio.Xenko.Graphics.Texture)' will adjust the current @'SiliconStudio.Xenko.Graphics.Viewport' to the full size of the render target.
If you want to render only to a subset of the texture, you can set render target and viewport separately using @'SiliconStudio.Xenko.Graphics.CommandList.SetRenderTarget(SiliconStudio.Xenko.Graphics.Texture,SiliconStudio.Xenko.Graphics.Texture)' and @'SiliconStudio.Xenko.Graphics.CommandList.SetViewport(SiliconStudio.Xenko.Graphics.Viewport)'.

Multiple viewports can be bound using @'SiliconStudio.Xenko.Graphics.CommandList.SetViewports(SiliconStudio.Xenko.Graphics.Viewport[])' and @'SiliconStudio.Xenko.Graphics.CommandList.SetViewport(System.Int32,SiliconStudio.Xenko.Graphics.Viewport)' overloads for use with a geometry shader.

**Code:** Setting the viewports

```cs
// example of a full HD buffer
CommandList.SetRenderTarget(GraphicsDevice.Presenter.DepthStencilBuffer, GraphicsDevice.Presenter.BackBuffer); // no viewport set
 
// example of setting the viewport to have a 10 pixel border around the image in a full hd buffer (1920x1080)
var viewport = new Viewport(10, 10, 1900, 1060);
CommandList.SetViewport(viewport);
CommandList.SetViewport(0, viewport);
```


# Scissor

The @'SiliconStudio.Xenko.Graphics.CommandList.SetScissorRectangles(System.Int32,System.Int32,System.Int32,System.Int32)' method is available to set the scissor. Contrary to the viewport, the user must provide the coordinates of the location of the vertices defining the scissor instead of its size. The method can be invocked with an array @'SiliconStudio.Core.Mathematics.Rectangle's in order to support multiple scissors.

**Code:** Setting the scissor

```cs
// example of setting the scissor to crop the image by 10 pixel around it in a full hd buffer (1920x1080)
CommandList.SetScissorRectangles(10, 10, 1910, 1070);
 
var rectangles = new[] { new Rectangle(10, 10, 1900, 1060), new Rectangle(0, 0, 256, 256) };
CommandList.SetScissorRectangles(rectangles);
```

