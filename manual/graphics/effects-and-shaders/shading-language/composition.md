# Composition

In addition to the inheritance system, XKSL introduces the concept of composition. A composition simply is a member whose type is another XKSL class. It is defined the same way as variables.

You can compose with an instance of the desired class or an instance of class that inherits from the desired one.

**Code:** Composition

```cs
class CompositionBase
{
	float4 Compute()
	{
		return float4(0.0);
	}
};
 
class CompositionClassA : CompositionBase
{
	float4 myColor;
 
	override float4 Compute()
	{
		return myColor;
	}
};
 
class CompositionClassB : CompositionBase
{
	float4 myColor;

	override float4 Compute()
	{
		return 0.5 * myColor;
	}
};
 
class BaseClass
{
	CompositionBase Comp0;
	CompositionBase Comp1;
 
	float4 GetColor()
	{
		return Comp0.Compute() + Comp1.Compute();
	}
};
```


The compositions are compiled in their own context, meaning that the non stage variables are only accessible within the composition. It is also possible to have compositions inside compositions.

If you want to access the root compilation context, you can use the following format.

**Code:** Accessing root context

```cs
class CompositionClassC : CompositionBase
{
	BaseClass rootShader = stage;
 
	float4 GetColor()
	{
		return rootShader.GetColor();
	}
};
```


This is really error prone since `CompositionClassC` expects `BaseClass` to be available in the root context.

It is also possible to create an array of compositions the same way you use an array of values. Since there is no way to know beforehand the number of compositions, you should iterate using a `foreach` statement.

**Code:** Array of compositions

```cs
class BaseClassArray
{
	CompositionBase Comps[];
	
	float4 GetColor()
	{
		float4 resultColor = float4(0.0);
 
		foreach (var comp in Comps)
		{
			resultColor += comp.Compute();
		}
 
		return resultColor;
	}
};
```


# Stage behavior

The behavior of the `stage` keyword is quite straightforward: only one instance of the variable or method is produced.

**Code:** Stage member behavior

```cs
class BaseClass
{
	stage float BaseStageValue;
	float NonStageValue;
};
 
class TestClass : BaseClass
{
	BaseClass comp0;
	BaseClass comp1;
};
 
// resulting class (representation)
class TestClass
{
	float BaseStageValue;
	float NonStageValue;
	float comp0_NonStageValue;
	float comp1_NonStageValue;
};
```


**Code:** Stage member bahavior

```cs
class BaseClass
{
	stage float BaseStageMethod()
	{
		return 1.0;
	}

	float NonStageMethod()
	{
		return 2.0;
	}
};
 
class TestClass : BaseClass
{
	BaseClass comp0;
	BaseClass comp1;
};
 
// resulting class (representation)
class TestClass
{
	float BaseStageMethod()
	{
		return 1.0;
	}

	float NonStageMethod()
	{
		return 2.0;
	}
	float comp0_NonStageMethod()
	{
		return 2.0;
	}
	float comp1_NonStageMethod()
	{
		return 2.0;
	}
};
```


Keep in mind that even in composition, you can call for base methods, override them etc. Overriding happens in the same order than the compositions.

This behavior is useful when you need a value in multiple composition but you only need to compute it once (for example the normal in view space).

# Clone behavior

The `clone` keyword has a less trivial behavior. It prevents the `stage` keyword to produce a unique method.

**Code:** Clone example

```cs
class BaseClass
{
	stage float BaseStageMethod()
	{
		return 1.0;
	}
 
	stage float BaseStageMethodNotCloned()
	{
		return 1.0;
	}
};
 
class CompClass : BaseClass
{
	override clone float BaseStageMethod()
	{
		return 1.0 + base.BaseStageMethod();
	}
 
	override float BaseStageMethodNotCloned()
	{
		return 1.0f + base.BaseStageMethodNotCloned();
	}
};
 
class TestClass : BaseClass
{
	CompClass comp0;
	CompClass comp1;
};
 
// resulting class (representation)
class TestClass
{
	// cloned method
	float base_BaseStageMethod()
	{
		return 1.0;
	}
 
	float comp0_BaseStageMethod()
	{
		return 1.0 + base_BaseStageMethod();
	}
 
	float BaseStageMethod() // in fact comp1_BaseStageMethod
	{
		return 1.0 + comp0_BaseStageMethod; // 3.0f
	}
 
	// not cloned method
	float base_BaseStageMethodNotCloned()
	{
		return 1.0f;
	}
 
	float BaseStageMethodNotCloned()
	{
		return 1.0f + base_BaseStageMethodNotCloned(); // 2.0f
	}
};
```


This behavior is useful when you want to repeat a simple function but with different parameters (like adding color on top of another).

 

