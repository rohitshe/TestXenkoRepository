# AudioSystem

The audio high level API provides simple ways to automate and control the sounds 3D localization integrated with the Entity system.

# Overview

The high level audio system is mainly composed of 4 classes

- The @'SiliconStudio.Xenko.Engine.AudioSystem' class
  - The **core class of the high level API**. It implements the @'SiliconStudio.Xenko.Engine.IAudioSystem'. By doing so, it provides an access to the low level API @'SiliconStudio.Xenko.Audio.AudioEngine', and provides the possibility to add or remove listeners to the system. 
- The @'SiliconStudio.Xenko.Engine.AudioListenerComponent' class 
  - **Adds audio listening capabilities to an existing Entity**. This component has to be attached to an @'SiliconStudio.Xenko.EntityModel.Entity'. By doing so, the programmer marks the entity as possible listener in the audio scene. To actually activate a listener it has to added to the audio system using the @'SiliconStudio.Xenko.Engine.IAudioSystem.AddListener'.
- The @'SiliconStudio.Xenko.Engine.AudioEmitterComponent' class 
  - **Adds audio emitting capabilities to an existing Entity**. This component has to be attached to an @'SiliconStudio.Xenko.EntityModel.Entity'. By doing so, the programmer expresses the fact entity can emits sounds. To actually associate (resp. remove) sounds to the entity, use the @'SiliconStudio.Xenko.Engine.AudioEmitterComponent.AttachSoundEffect' (resp. @'SiliconStudio.Xenko.Engine.AudioEmitterComponent.DetachSoundEffect') method. Sounds that are attached to an emitter component needs to be instanciable and 3D localizable, that is why only mono track @'SiliconStudio.Xenko.Audio.SoundEffect' can be used. Insofar as a same sound effect can be associated to several different entities, the programmer has to ask the entities for a controller to be able to control the playback of each entities independently. This can be done with the @'SiliconStudio.Xenko.Engine.AudioEmitterComponent.GetSoundEffectControler' method .The programmer can also control the sound distance attenuation scale and the Doppler effect scale for the entity by modifying the @'SiliconStudio.Xenko.Engine.AudioEmitterComponent.DistanceScale' and @'SiliconStudio.Xenko.Engine.AudioEmitterComponent.DopplerScale' properties.
- The @'SiliconStudio.Xenko.Engine.AudioEmitterSoundController' class 
  - **Controls the playback of the sound effect associated to entities**. The class implements the @'SiliconStudio.Xenko.Audio.IPlayableSound' interface. Controllers can not be created directly but has to be queried entities via their audio emitter component using the @'SiliconStudio.Xenko.Engine.AudioEmitterComponent.GetSoundEffectControler' method.

# Usage

The @'SiliconStudio.Xenko.Engine.AudioSystem' can be accessed via the @'SiliconStudio.Xenko.Engine.IAudioSystem' interface available in the @'SiliconStudio.Xenko.Game' and the @'SiliconStudio.Xenko.Script' classes.

**Code:** Access the audio system

```cs
// Accessing a method of the AudioSystem
Audio.AddListener(...);```


## Loading a sound or music

To load sound effects or sound musics, use the asset loader *Load* and *LoadAsync* functions.

 

**Code:** Load sound effects and sound musics

```cs
var soundMusic = Asset.Load<SoundMusic>("/assets/mySoundMusic"); // or await Asset.LoadAsync<SoundMusic>("/assets/mySoundMusic");
var soundEffect = Asset.Load<SoundEffect>("/assets/mySoundEffect"); // or await Asset.LoadAsync<SoundEffect>("/assets/mySoundEffect");```


## Playing a music

To play a background compressed music use directly the low level API @'SiliconStudio.Xenko.Audio.SoundMusic' class. Notice that the background music is never localized, so it does not takes in account whether the programmer added listeners to the scene or not.

 

**Code:** Play a background music

```cs
soundMusic.IsLooped = true;
soundMusic.Play();```


## Attaching an Audio listener and emitter

To add a listener to the scene, just create a @'SiliconStudio.Xenko.Engine.AudioListenerComponent', attach it to an entity and then add it to the audio system. Several listeners can be added to the system at the same time. 

**Code:** Add listeners to the scene

```cs
var listenerComponent1 = new AudioListenerComponent(); // create the audio listener component
cameraEntity1.Set(AudioListenerComponent.Key, listenerComponent1); // attach it to the camera entity
Audio.AddListener(listenerComponent1); // activate the listener by adding it to the audio system
 
//...
 
// possibly change the active rendering camera and update the listener in accordance.
// ...
Audio.RemoveListener(listenerComponent1);
Audio.AddListener(listenerComponent2);```


 

To create audio emitters, first create the @'SiliconStudio.Xenko.Engine.AudioEmitterComponent', then associate them to an entity and then attach them sound effects.

**Code:** Create sound emitters

```cs
// create the audio emitter components
var emitterComponent1 = new AudioEmitterComponent(); 
var emitterComponent2 = new AudioEmitterComponent();
 
// associate them to entities
entity1.Set(AudioEmitterComponent.Key, emitterComponent1);
entity2.Set(AudioEmitterComponent.Key, emitterComponent2);

// attach sound effects to the components
emitterComponent1.AttachSoundEffect(soundEffect1);
emitterComponent1.AttachSoundEffect(soundEffect2);
emitterComponent2.AttachSoundEffect(soundEffect1);```


 

To control the playback of sound effects associated to entities, query the @'SiliconStudio.Xenko.Engine.AudioEmitterComponent' for a controller and use it like a standard playable sound.

**Code:** Control sound emitter playback with sound controllers

```cs
// query the sound controllers corresponding to the different sound effects and entities.
var soundControllerEntity1SE1 = emitterComponent1.GetSoundEffectController(soundEffect1);
var soundControllerEntity1SE2 = emitterComponent1.GetSoundEffectController(soundEffect2);
var soundControllerEntity2SE1 = emitterComponent2.GetSoundEffectController(soundEffect1);
 
soundControllerEntity1SE2.IsLooped = true; // loop sound effect 2 of entity 1
soundControllerEntity1SE2.Volume = 0.60f; // reduce sound intensity for sound effect 2 of entity 1
soundControllerEntity1SE2.Play(); // start playing the sound effect 2 of entity 1
 
// ...
 
soundControllerEntity1SE1.Play(); // plays sound effect 1 of entity 1
 
// ... 

soundControllerEntity2SE1.Play(); // plays sound effect 1 of entity 2
 ```


# Remarks

- If no sound listeners are added to the audio system playing the sounds via sound controllers will have no effects.
- A sound effect can be played directly without being associated to an audio emitter component, but it this case the sound will not be localized in the 3D scene automatically.
- Adding and removing listeners does not affect sound musics.
- Adding and removing listeners while some sound controllers are playing should be avoided. 
- Playing a sound controller will have no effects if the corresponding entities are not added to the entity system.
- A sound controller is invalided as soon as the corresponding sound effect is detached from the audio emitter component.

