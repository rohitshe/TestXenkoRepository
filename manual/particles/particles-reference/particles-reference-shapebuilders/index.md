# Particle Shape Builders

![media/particles-reference-shapebuilders-0.png](media/particles-reference-shapebuilders-0.png) 

Shape Builders create mesh shapes from the living particles so that they can be rendered. Currently only one type of shape can be assigned to an emitter.

## Billboard

Every particle is expanded to a 1 meter by 1 meter camera-facing quad. If the particle has any size, it is scaled accordingly. The billboards only support angular rotation.

## Hexagon

Every particle is expanded to a camera-facing hexagon with 0.5 meter sides. If the particle has any size, it is scaled accordingly. The hexagons only support angular rotation.

## Quad

Every particle is expanded to a 1 meter by 1 meter up-facing quad. If the particle has any size, it is scaled accordingly. The quads support 3D orientation and rotation.

The picture below shows a billboard, a hexagon and a quad:

![media/particles-reference-shapebuilders-1.png](media/particles-reference-shapebuilders-1.png) 

## More

Ribbons, trails and meshes are also planned for future releases.
