# Film Grain

This effects adds noise at each frame to simulate the grain of films used in real-world cameras.

![images/film-grain-1.png](images/film-grain-1.png) 

The pattern is procedurally generated and changes at each frame. 

To simulate real film grain, the noise should be more visible in areas of medium light intensity. Whereas noise is less present in very bright or very dark areas.

The pattern locally modifies the luminance of the pixels affected.

![images/film-grain-2.png](images/film-grain-2.png) 

# Properties

| Property         | Description                                                                 |
| ---------------- | --------------------------------------------------------------------------- |
| Amount           | Amount/Strength of the effect.                                              |
| Grain Size       | Size of the grain.                                                          |
| Animate          | When enabled, the procedural pattern changes at each frame.                 |
| Luminance Factor | How strongly the original pixel luminance is affected by the grain pattern. |


