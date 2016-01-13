# Draw vertices

When loading a scene, Xenko handles automatically the draw calls to display the scene through the entity system. In this page, only the manual draw calls will be introduced.

# Primitives

The Xenko engine provides the user a set of built-in primitives:

- plane
- cube
- sphere
- geosphere
- cylinder
- torus
- teapot

These are not automatically created with the @'SiliconStudio.Xenko.Graphics.GraphicsDevice' so the user has to manually ask for their creation. This is really simple since they are part of the engine through the @'SiliconStudio.Xenko.Graphics.GeometricPrimitive' class.

**Code:** Creating and using a primitive

```cs
// creation
var myCube = GeometricPrimitive.Cube.New(GraphicsDevice);
var myTorus = GeometricPrimitive.Torus.New(GraphicsDevice);
 
// (...)
 
// draw one on screen
myCube.Draw();
```


There is no effect associated to them so the user has to manually set one. For example he can use the built-in @'SiliconStudio.Xenko.Graphics.SimpleEffect' one. Here is a page with information about [effects and shaders](../effects-and-shaders-reference/index.md).

# Custom drawing

Outside of these primitives, the user can draw any set of vertices. There are many functions to draw vertex array objects based on the way the vertices are indexed, the type of primitive etc. To know how to create vertex array object, refer to the @'SiliconStudio.Xenko.Graphics.VertexArrayObject' class documentation.

The user should first specify the vertex array object he wants to draw. This is done through the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.SetVertexArrayObject' method. Then he can call the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.Draw' method. He needs to specify various parameters including the primitive type. Please refer to the @'SiliconStudio.Xenko.Graphics.GraphicsDevice.Draw' method and its derived methods documentation (for example @'SiliconStudio.Xenko.Graphics.GraphicsDevice.DrawIndexed', @'SiliconStudio.Xenko.Graphics.GraphicsDevice.DrawInstanced' etc.).

**Code:** Drawing a VAO

```cs
// create the VAO
var myVao = VertexArrayObject.New(GraphicsDevice, ...);
 
// set an effect
myEffect.Apply();
 
// set the VAO
GraphicsDevice.SetVertexArrayObject(myVao);
 
// draw 100 triangles
GraphicsDevice.Draw(PrimitiveType.TriangleList, 300);
```


The list of the supported primitive types can be found in the @'SiliconStudio.Xenko.Graphics.PrimitiveType' enum documentation.

 

