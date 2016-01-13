# AudioEngine

The low level API offers an interface for audio playback and audio space localization.

# Overview

This API is mainly composed of 4 classes:

- The @'SiliconStudio.Xenko.Audio.AudioEngine' class
  - This is **the core of the audio module**. It creates and initializes the audio device on the specific machine. It is also in charge of updating all the sounds played at each frame, but the programmer does not need to manipulate it directly. Xenko creates a default audio engine that can be used at any time to load new sounds (see Usage to see how to access it).
- The @'SiliconStudio.Xenko.Audio.SoundMusic' class
  - Allows to **play long compressed musics** (usually background musics). Sound musics are high latency and should not be used to play short and frequent sounds. Due to hardware limitations, only one instance of sound music can be played at a given time. Starting to play a instance while another is still playing will stop the previous instance and start the new one. Sound musics implement the @'SiliconStudio.Xenko.Audio.IPlayableSound' interface. 
- The @'SiliconStudio.Xenko.Audio.SoundEffect' class 
  - Allows to **play short and repetitive WAV sounds with low latency** (e.g. gun shots, etc). A sound effect can be played directly, but most of the time they will be created via @'SiliconStudio.Xenko.Audio.SoundEffectInstance'. All the sound effect instances issued from a given sound effect share the same audio data. This provides a simple way to play simultaneously several times the same sound and avoid data duplication. The sound effect and sound effect instance classes implement the @'SiliconStudio.Xenko.Audio.IPositionableSound' interface which allows the programmer to localize the sound in a 3D scene. To do so, a @'SiliconStudio.Xenko.Audio.AudioEmitter' class and @'SiliconStudio.Xenko.Audio.AudioListener' class must be created and localization must be performed explicitly by calling the `Apply3D (ref:{SiliconStudio.Xenko.Audio.IPositionableSound.Apply3Dv})` function every time the localization must be updated. Usually the audio listener is the same for all the scene and is attached to the camera.
- The @'SiliconStudio.Xenko.Audio.DynamicSoundEffectInstance' class 
  - Allows to **play synthesized dynamic audio sounds**. The class inherits from @'SiliconStudio.Xenko.Audio.SoundEffectInstance' and thus can be similarly be localized. Data can be provided to a dynamic audio instance by using the function @'SiliconStudio.Xenko.Audio.DynamicSoundEffectInstance.SubmitBuffer'. Any time that the instance is going to miss data to play, the instance triggers the @'SiliconStudio.Xenko.Audio.SoundEffectInstance.BufferNeeded' event.

# Usage

To access the Xenko default audio engine, use the @'SiliconStudio.Xenko.Audio.IAudioSystem.AudioEngine' property, available through @'SiliconStudio.Xenko.Game and T:SiliconStudio.Xenko.Script' classes.

**Code:** Access to the default audio engine

```cs
var defaultAudioEngine = Audio.AudioEngine;```


## Loading a music or a sound

To load @'SiliconStudio.Xenko.Audio.SoundEffect' or @'SiliconStudio.Xenko.Audio.SoundMusic' preferably use the asset loader *Load*  and *LoadAsync* functions, or use their dedicated Load functions. 

**Code:** Load sound music and sound effects

```cs
// recommended method: use the asset loader
var soundMusic = Asset.Load<SoundMusic>("/assets/mySoundMusic"); // or await Asset.LoadAsync<SoundMusic>("/assets/mySoundMusic");
var soundEffect = Asset.Load<SoundEffect>("/assets/mySoundEffect"); // or await Asset.LoadAsync<SoundEffect>("/assets/mySoundEffect");
 
// alternative method: use the SoundEffect and SoundMusic load methods.
var soundMusic2 = SoundMusic.Load(audioEngine, "/assets/mySoundMusic");
var soundEffect2 = SoundEffect.Load(audioEngine, audioDataStream);```


## Playing a music or a sound

To play, loop, pause, stop, stop looping, change volume, pan and inquiry about the a sound play status just call the corresponding method from the @'SiliconStudio.Xenko.Audio.IPlayableSound' and @'SiliconStudio.Xenko.Audio.IPositionableSound'. Note that the sound loop status can be changed only when the sound is not playing.

**Code:** Playback action on a sound

```cs
soundMusic.IsLooped = true; // set the background music as looped.
soundMusic.Volume = 0.5f; // reduce by half the background sound intensity
soundMusic.Play(); // start the playback of the background music.
 
soundEffect2D.Pan = 0.75f; // locate a 2D sound effect in the scene by modifying its pan value.
soundEffect2D.Play(); // start playing the sound effect.
 
// ...
 
soundMusic.ExitLoop(); // stops looping the background sound.
 
// ...
 
if(soundMusic.PlayState == SoundPlayState.Stopped) // inquiry a sound play state
	nextSoundMusic.Play();
 
// ...
 
nextSoundMusic.Stop(); // immediately stop the new background music.```


## Playing a 3D localized sound

To localize a sound into the 3D space (distance intensity attenuation and doppler effect), first create an audio listener and emitter and then use the @'SiliconStudio.Xenko.Audio.IPositionableSound.Apply3D' function.

**Code:** 3D localization of sounds

```cs
void async Task UpdateSoundLocalization()
{
	var audioListener = new AudioListener(); // create the scene audio listener
	var audioEmitter1 = new AudioEmitter(); // create the audio emitter for the first sound effect with default distance and doppler scale
	var audioEmitter2 = new AudioEmitter { DistanceScale = 10, DopplerScale = 0.1 }; // create the audio emitter for the second sound effect and adjust its distance and doppler scale
 
	while(true)
	{
		await Scheduler.NextFrame();
 
		// update the scene listener position and velocity
		audioListener.Position = currentCameraPosition;
		audioListener.Velocity = currentCameraVelocity;
		audioListener.Forward = currentCameraForwardVector;
		audioListener.Up = currentCameraUpVector;
 
		// update emitter 1
		audioEmitter1.Position = currentPositionSE1;
		audioEmitter1.Velocity = currentVelocitySE1;
 
		// update emitter 2
		audioEmitter2.Position = currentPositionSE2;
		audioEmitter2.Velocity = currentVelocitySE2;
 
		// apply localization
		soundEffect1.Apply3D(audioListener, audioEmitter1);
		soundEffect2.Apply3D(audioListener, audioEmitter2);
	}
}```


To play a same sound effect simultaneously at different intensities and positions at the same time, use @'SiliconStudio.Xenko.Audio.SoundEffectInstance'. 

**Code:** Use Sound Effect Instances

```cs
// create the sound effect instances
var seInstance1 = soundEffect.CreateInstance(); // create an instance of the sound effect.
var seInstance2 = soundEffect.CreateInstance(); // create a second instance of the same sound effect.
 
// localize the sound effect instances.
seInstance1.Apply3D(listener, emitterSE1);
seInstance2.Apply3D(listener, emitterSE2);
 
// ...
 
if(shouldPlaySEInstance1)
	seInstance1.Play();
 
// ... 

if(shouldPlaySEInstance2)
	seInstance2.Play();```


## Playing a dynamic synthesized sound

Start by creating a @'SiliconStudio.Xenko.Audio.DynamicSoundEffectInstance' instance. Then submit data to play or set the submit it in the *BufferNeeded* callback. Finally make a call to *Play*.

**Code:** Dynamic Sound Effect Instance

```cs
DynamicSoundEffectInstance dynamicSE;
 
void SubmitNextData(object sender, EventArgs args)
{
	var dataBuffer = GenerateDataBuffer();
 
	dynamicSE.SubmitBuffer(dataBuffer);
}
 
void Execute()
{ 
	dynamicSE = new DynamicSoundEffectInstance(audioEngine, 48000, AudioChannels.Mono, AudioDataEncoding.PCM_16Bits);
	dynamicSE.BufferNeeded += SubmitNextData;
	dynamicSE.Play();
 
	//...
 
	dynamicSE.Stop();
}
 
 ```


