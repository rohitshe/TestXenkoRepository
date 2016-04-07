# Inheritance

This tutorial explains in depth how to create particles which inherit one or more attributes, like position or color, from other particles.

## Sample

You can check the **Child Particles Sample** if you need a quick look at a project which already uses inheritance from particles.

![images/particles-tutorials-inheritance-0.jpg](images/particles-tutorials-inheritance-0.jpg)

## Step-by-step

### Inheriting position

It helps if you think about inheritance in terms of parent and child particles. You can check the Fireworks particle system from the sample project.

It contains two emitters. Particles reference parent emitters by name, so in the first emitter you can see we have set the *Emitter Name* property. It's an optional name, but it's required if you want other emitters to be able to reference this emitter's particles.

In the second emitter we can create a new initializer, *Position from parent*. It allows us to reference the first emitter's particles and use their position for initializing the child particles. In the *Parent emitter* attribute we put the first emitter's name (which is "Parent").

This will randomly assign a parent particle for each child particle spawned and copy its position to the child particle. 

The *Parent Offset* seed is used to match fields when more than one attributes are inherited. For example if you want to inherit both *Position* and *Color* from the same parent particle (which is chosen at random) you should set the *Parent Offset* seed to be the same. Alternatively you can set the *Parent Offset* seed for both initializers to be different, in which case particles spawning from one parent's position can inherit their color from a different random particle. Usually you want to keep them the same, but in some cases you might want to mix them.

![images/particles-tutorials-inheritance-1.png](images/particles-tutorials-inheritance-1.png)

As you can see this kind of inheritance does not control spawn count, maximum particles or any other parameters and is very random. For most effects it is sufficient, but sometimes you want more direct control over the particles.

### Controlled inheritance

On occasion you will want to spawn a certain number of particles from a specific parent and have those particles only inherit attributes from the parent particle that spawned them.

To do this, choose a spawner for the child emitter from type *From parent*. Fill in the parent emitter's name in the *Parent emitter* field.

The *Spawn Control Group* determines how the particles will save their control information. You will need to assign the same control group on all initializers later in order to retrieve the spawning information.

There can be up to 4 control groups. If you spawn particles based on different conditions, or spawn more than 2 different child particles from the same parent, assign them different control groups so they don't get mixed up.

The *Particle Spawn Trigger* is the triggering condition on the parent side, which determines if particles should be spawned. If you leave it to None, no particles will be spawned, so set it to *On Hit* or *Lifetime*.

On hit works for parent particles which have a Collider assigned and triggers every time they hit the surface.

Lifetime is based on the parent particle's relative lifetime and triggers every frame the lifetime is within the limits. There are two sliders to control from which point to which point particles should be spawned, or you could reverse them to reverse the spawning condition. For example a particle with lifetime condition (0.9 - 1.0) will only spawn child particles in the last 10% of its lifetime.

Finally, there is the *Particles/trigger* field, which determines how many particles are spawned each time the condition is met.

For child emitters it's also a good practice to control the maximum number of particles the emitter can have, especially for non-deterministic cases, such as the collision hit.

#### Determinism

On the initializers choose a *Spawn Control Group* corresponding to the spawner's control group. This will force the initializers to only work for particles which were spawned with the triggering condition, skipping the rest (if more than one spawners are assigned).

### Ribbons and Trails

Ribbons and trails renderers are a little more difficult to set up in the beginning, as they are quite dependant on spawn order. In case of parents, they also become dependant on the parent's spawn order.

First, add a *Spawn Order* initializer to the parent. It will be used in the children particles.

Then, on the child emitter, remove all spawners and add only one, *From parent*. You want to control the spawning of the children particles so that all particles can be properly grouped in a ribbon behind the parent particle. If you add another spawner which adds random behavior to the system, the ribbons will be connected in the wrong way. Set the triggering condition to Lifetime.

Finally, on the child emitter side again, add a *Order from parent* initializer. This will assign a spawning order to the particles, but also group them by parent. If you set the *Sort* to use this order and assign a Ribbon shape builders, you will see how each trail is properly grouped behind the parent particle that spawned it.

### Circular behavior

Particle emitters can inherit attributes in a circular manner from each other, or even inherit attributes from particles in the same emitter. This can produce random or swingy effects, but can be interesting.

In the Colliding Particles system from the sample above you can see that particles are spawned on hit, but the parent emitter is the same! This means that each time a particle hits the surface, it will produce more of the same kind!

There are two important elements which allow this to happen. First, we have two sapwners. One of them spawns a small number of particles per second, which give us the initial elements to populate the system. The other spawner spawns more particles on hit and uses a control group.

The second element to the system is the *Position* initializer. There are two of them again. The first one assigns a position from where we want the particles to first appear. It works over all particles (even those spawned from parents), so if you leave it like this, it will just shoot more particles from the initial position every time they hit the surface.

The second initializer is *Position from parent* and it initializes the particles positions using the same control group as the On hit spawner.

What happens is that the *Position from parent* overwrites the positions for the particles with control group, leaving the particles spawned from the *Per second* spawner untouched. As a result you have a small number of particles constantly coming from a single entry point and multiplying like an avalanche every time they hit the surface.





