# Template

Class templating is available in XKSL. Contrary to many templating system, pdxsl requires strong typed templates. The available types are:

- value types from HLSL (float, int, float2, float3, float4)
- 2D textures
- sampler states
- semantics: used to replace semantics on variables.
- link types: used to replace link annotations

A instanciated class will behave the same way as any other class. The value, texture and sampler template parameters are accessible like any other variable. However it is impossible to modify their value. Attempting to do so will result in a compilation error. If a template variable is incorrectly used (e.g. using a sampler as a semantic), it should result in a compilation error. However, the behavior is officially unknown.

**Code:** Templating

```cs
class TemplateClass<float speed, Texture2D myTexture, SamplerState mySampler, Semantic mySemantic, LinkType myLink>
{
	[Color]
	[Link("myLink")]
	float4 myColor;
 
	stream float2 texcoord : mySemantic;
 
	float4 GetValue()
	{
		return speed * myColor * myTexture.Sample(mySampler, streams.texcoord);
	}
};
 
// TO INSTANCIATE THE CLASS, USE A CODE AS FOLLOW
TemplateClass<1.0f, Texturing.Texture0, Texturing.Sampler0, TEXCOORD0, MyColorLink>
```


 

 

