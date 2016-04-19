# How do I setup a HDR pipeline with posteffects?

Assuming that you are already familiar with the  [Scene Graphics Compositor](../graphics-compositor/index.md), we would like to setup a High Dynamic Range rendering pipeline.

The principle of HDR rendering is to render to a scene to a HDR render target and then apply a Tone Mapping to convert HDR to a LDR render target to display it on the screen.

| LDR Rendering                                                                                                                | HDR Rendering                                                                                                                |
| ---------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| ![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-1.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-1.png)  | ![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-2.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-2.png)  |


> **Note**
> 
> As part of the Graphics Compositor, we will provide a set of default pre-configured rendering paths including a HDR to LDR rendering path as described here.    

# Step0: Visualize the pipeline

We would like to setup the following pipeline:

 

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-3.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-3.png) 

Without a HDR to LDR rendering path, a scene illuminated with several lights with high intensities would just look white:

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-1.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-1.png) 

# Step1: Go to the Graphics Compositor Properties

In the current scene, select the root entity

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-4.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-4.png) 

The properties of the graphics compositor are editable in the Property Editor:

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-5.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-5.png) 

# Step2: Add a camera slot and Select a Camera

Click on the ![media/add.png](media/add.png)  on the Cameras category:

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-7.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-7.png) 

And then click on the finger point to select a Camera in a Scene (If you don't have a camera in a scene, right click on the root scene to create a new camera)

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-8.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-8.png) 

# Step3 : Add a layer and create a HDR RenderFrame Output

- Then add a new layer. Change the name to HDR Layer
- In the Output, select **RenderFrame**
- In the **Format property** of the RenderFrame, select **HDR**

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-9.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-9.png) 

# Step 4: Add Renderers to the HDR Layer

Then on the HDR Layer, you can click on the Renderers ![media/add.png](media/add.png)  and add the following renderers:

- Add a **Clear RenderFrame**
- Add a **Render Camera**

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-10.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-10.png) 

# Step 5: Add a Render Effect to the Master (LDR) Layer

Now we need to transform the HDR output of the HDR Layer to the renderframe LDR of the Windows output

- Add a **Render Effect** by clicking on the Renderers ![media/add.png](media/add.png)
- For the Effect, select "**Post-Processing Effects**"

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-11.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-11.png) 

# Step 6: Configure the Post Processing Effects with a Tone Map operator

On the Post-Processing Effects

- Disable all non-necessary effects (Depth Of Field, Bright Filter...etc.)
- In the Color Transforms, press ![media/add.png](media/add.png)  to add a new color transformation and select the **ToneMap** transform

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-12.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-12.png) 

The scene should now look more natural instead of a saturated white:

![media/how-do-i-setup-a-hdr-pipeline-with-posteffects-2.png](media/how-do-i-setup-a-hdr-pipeline-with-posteffects-2.png) 

 

 

 

 

