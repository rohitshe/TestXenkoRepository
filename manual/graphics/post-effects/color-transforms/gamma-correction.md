# Gamma Correction

All the post-effect calculations are made in a linear-space (also called RGB space). Which means doubling the color value of a pixel will double the light it emits. 

This guarantees correct lighting calculations.

However our monitors don't behave this way: for dark color values they tend to emit much less light than they should. 

This is why after all our post-effects have been applied, we must apply [Gamma correction](http://en.wikipedia.org/wiki/Gamma_correction)  as the final step, to transform our image from a linear space to a sRGB space (or gamma space). 

A buffer in the sRGB space will be be displayed correctly on a monitor or a TV screen.

![images/gamma-correction-1.png](images/gamma-correction-1.png) 

*Non-gamma-corrected images have their dark areas appear darker than they're supposed to.*

![images/gamma-correction-2.png](images/gamma-correction-2.png) 

# Properties

| Property | Description                                     |
| -------- | ----------------------------------------------- |
| Value    | Gamma value. A traditional value is around 2.2. |


 

 

