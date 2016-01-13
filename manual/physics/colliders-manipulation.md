# Colliders Manipulation

Once the colliders are created within the Game Studio we are ready to manipulate them from the game code.

Step by step:

- **Entity Asset loading:**
  
  ```cs
  var ball0 = Asset.Load<Entity>("ball");
```
  
  
  The asset is now loaded but not quite yet into the Physics pipeline, and so we can change now the initial starting position for example:
  
  ```cs
  ball0.Transformation.Translation = new Vector3((VirtualResolution.X / 2.0f) + 100, VirtualResolution.Y / 2.0f, 0.0f);
```
  
  
  Having a correct initial position is very important for a RigidBody as it will start immediate simulation and so might have consequences in the physics world.
  
   
- **Adding the entity to the engine pipeline:**
  
  ```cs
  Entities.Add(ball0);
```
  
  
  This step is simple but hides something very important: this is exactly when the Collider/RigidBody/Character is created and added to the Physics engine for simulation. 
  
  This also means that from now we got a valid  Collider/RigidBody/Character object we can manipulate.
  
   
- **Access the Collider/RigidBody/Character objects:**
  
  ```cs
  var ball0RigidBody = ball0.GetOrCreate(PhysicsComponent.Key)[0].RigidBody;
```
  
  
  An Entity with a PhysicsComponent might have multiple PhysicsElement/s. Using the indexer on PhysicsComponent you can easy access each of the elements.
  
  Above only one element was present. In turn PhysicsElement contains easy to use properties to access the physics objects, just make sure you select the right one (e.g. A Character cannot cast to RigidBody)
- **From now you can use the SiliconStudio.Xenko.Physics library to manipulate in depth Colliders. E.g.:**
  
  ```cs
  ball1RigidBody.CanSleep = false; //disable sleeping so to never get stuck in case of no motion
  ball1RigidBody.Restitution = 1.0f;
```

 

 

