# Color Transforms

Color transform is a special kind of effect designed to be combined in a chain at runtime. 

You can define a series a color transforms to apply to an image, each transform using the previous transform's output as its own input. 

At runtime, the series of transforms is squashed into one shader and rendered in a single draw call to get the maximum performance.

