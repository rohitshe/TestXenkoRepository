# Shader stages

# Stages overview

The function for each stage has a predefined name so you should stick with it.

- `HSMain` for hull shader
- `HSConstantMain` for patch constant function
- `DSMain` for domain shader
- `VSMain` for vertex shader. It takes no arguments.
- `GSMain` for geometry shader
- `PSMain` for pixel shader. It takes no arguments.
- `CSMain` for compute shader. It takes no arguments.

These are all void methods.

The geometry and tessellation shaders need some kind of predefined structures as input and output. However, since our shaders are really generic, it is impossible to know beforehand what the structure will be. As a result, These shaders use `Input` and `Output` structures that will be automatically generated to fit the final shader.

# Vertex shader

A vertex shader has to set the variable with the semantic `SV_Position`. In `ShaderBase`, it is `ShadingPosition`.

**Code:** Example of vertex shader

```cs
override stage void VSMain()
{
	...
	streams.ShadingPosition = ...;
	...
}
```


# Pixel shader

A pixel shader has to set the variable with the semantic `SV_Target`. In `ShaderBase`, it is `ColorTarget`.

**Code:** Example of pixel shader

```cs
override stage void PSMain()
{
	...
	streams.ColorTarget = ...;
	...
}
```


# Geometry shader

**Code:** Example of geometry shader

```cs
[maxvertexcount(1)]
void GSMain(triangle Input input[3], inout PointStream<Output> pointStream)
{
	...
	// fill the streams object
	streams = input[0];
 	...
 
	// always append streams
	pointStream.Append(streams);
	...
}
```


`Input` can be used in the method body. It behaves like the streams object and contains the same members.

`Output` is only used in the declaration of the method. You should append the streams object to your geometry shader output stream.

# Tessellation shader

**Code:** Example of tessellation shader

```cs
[domain("tri")]
[partitioning("fractional_odd")]
[outputtopology("triangle_cw")]
[outputcontrolpoints(3)]
[patchconstantfunc("HSConstantMain")]
[maxtessfactor(48.0)]
void HSMain(InputPatch<Input, 3> input, out Output output, uint uCPID : SV_OutputControlPointID)
{
	...
	output = streams;
}
 
void HSConstantMain(InputPatch<Input, 3> input, const OutputPatch<Input2, 3> output, out Constants constants)
{
	...
	output = streams;
	...
}
 
[domain("tri")]
void DSMain(const OutputPatch<Input, 3> input, out Output output, in Constants constants, float3 f3BarycentricCoords : SV_DomainLocation)
{
	...
	output = streams;
	...
}
```


`Input` and `Input2` both behave like streams. Don't forget to assign `ouput` to `streams` at the end of your stage.

# Compute shader

**Code:** Example of compute shader

```cs
[numthreads(2, 3, 5)]
void CSMain()
{
	...
}
```


you can inherit from the `ComputeShaderBase` class and override the `Compute` method.

