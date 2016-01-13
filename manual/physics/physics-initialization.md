# Physics Initialization

# Setup Physics in your game

> **Note**
> 
> Currently, Bullet Physics is used as physics engine.    

Once you create a new Game project the Physics engine will be dormant by default, to actually use Physics we need to initialize the engine first.

Within your Game class add a PhysicsSystem variable:

```cs
private IPhysicsSystem physicsSystem;```


Now we initialize the system, using Bullet2 as internal engine ( also currently the only available ):

```cs
physicsSystem = new Bullet2PhysicsSystem(this); //where "this" is our Game derived class
physicsSystem.PhysicsEngine.Initialize();```


The best location for this line is either in the Game constructor or in any step that will execute before the actual game starts ( e.g. LoadContent )

As you can see the Initialize method accepts flags. Those flags are used to configure the engine for your needs as following:

- None - A regular dynamic world engine
- CollisionsOnly - No dynamic just report collision events
- ContinuousCollisionDetection - A dynamic world with support for continuous collision detection, useful for FPS games

# Debugging

During debug you might want to enable the rendering of debug collider shapes. To do so add this code after initialization:

```cs
physicsSystem.PhysicsEngine.CreateDebugPrimitives = true;
physicsSystem.PhysicsEngine.RenderDebugPrimitives = true;
physicsSystem.PhysicsEngine.DebugEffect = new PhysicsDebugEffect(GraphicsDevice);```


 

 

 

