# Unity Design - Week 04

## Agenda

2. [Video Player](#Unity-Video-Player)  
3. [Audio Basics](#Unity-Audio-Basics)  
4. [Particle System](#Unity-Particle-System)  
5. [Lighting System](#Unity-Lighting)  
6. [Tweening with DOTween](#DO-Tween)  
7. [Animator Controllers / Mixamo](#Animator-Controller)
8. [Cinemachine](#Cinemachine)

1. [Unity Mobile App Build](#Unity-Mobile-App-Build-Process)  
2. [Unity Remote](#Unity-Remote)  
3. [Cross-Platform UI](#Cross-Platform-UI)
4. [Unity Multiplayer with Netcode for GameObjects](#Unity-Multiplayer-Netcode)  
5. [Class Assignment - Week #04](#Create-with-Code)  


---
# 🎬 Unity Video Player 

A hands-on guide for using Unity’s **Video Player** component will help you import videos, configure the Video Player, and render clips as full-screen cutscenes or in-world displays.

---

## 🧰 Step 1: Importing a Video
1. Drag your video file into the Unity **Assets** folder.  
2. Select the video asset and review import settings in the **Inspector**:  
   - ✅ **Transcode**: Ensures platform compatibility (recommended).  
   - ✅ **Loop**: Enable if the video should repeat.  

📸 *Example: Video asset import settings*  
![Video Import Settings](images/video-import-settings.png)

---

## 🎮 Step 2: Add a Video Player Component
1. Create an empty GameObject (`GameObject > Create Empty`).  
2. Rename it `VideoPlayer`.  
3. In the **Inspector**, click **Add Component > Video Player**.  
4. Drag your video clip into the **Video Clip** slot.  

📸 *Example: Video Player component attached to GameObject*  
![Video Player Component](images/video-player-component.png)

---

## 📺 Step 3: Choose a Render Mode

### 1. **Camera Far/Near Plane**
- Plays video **full-screen** in front of or behind a camera.  
- Perfect for cutscenes or splash screens.  
- Set `Render Mode = Camera Near Plane` or `Far Plane`.  

📸 *Full-screen video render via camera*  
![Camera Render Example](images/video-camera-mode.png)

---

### 2. **Material Override (In-World Screen)**
- Apply video as a **texture** on a 3D object (like a TV screen or billboard).  
- Steps:
  1. Create a **3D Object > Quad**.  
  2. Create a **Material** and assign it to the Quad.  
  3. In Video Player → `Render Mode = Material Override`.  
  4. Assign your Quad’s Mesh Renderer and set the property (e.g. `_MainTex`).  

📸 *In-world TV screen example*  
![Material Override Example](images/video-material-override.png)

---

### 3. **Render Texture**
- Flexible method: sends video to a **RenderTexture** asset.  
- Useful for post-processing, UI backgrounds, or custom shaders.  

Steps:  
1. Create **Assets > Create > Render Texture**.  
2. Assign it in Video Player → `Target Texture`.  
3. Use the RenderTexture in any **UI Image** or **Material**.  

📸 *Render Texture setup in Inspector*  
![Render Texture Example](images/video-render-texture.png)

---

## 🔧 Step 4: Fixing Aspect Ratio
- If the video looks **stretched**:
  - Adjust Quad’s scale when using Material Override.  
  - Match RenderTexture width/height to the video’s resolution.  

📸 *Correct aspect ratio vs stretched*  
![Aspect Ratio Fix](images/video-aspect-ratio.png)

---

## 💡 Tips & Best Practices
- ✅ Always enable **Transcode** for cross-platform support.  
- ✅ Use **Render Texture** for advanced effects (shaders, UI backgrounds).  
- ✅ Keep videos **compressed** for performance (especially mobile).  
- ✅ Use **Material Override** for immersive, in-world playback.  
- ✅ Use **Camera Render** for simple, full-screen cutscenes.  

---

## 🎯 Example Use Cases
- 🎬 **Cutscenes** → Render mode: Camera Near Plane.  
- 📺 **TV/Monitor in scene** → Render mode: Material Override.  
- 🌌 **Background animation** → Render mode: Render Texture + UI Image.  

---

## 📚 References
- 📖 [Unity Manual – Video Player](https://docs.unity3d.com/Manual/VideoPlayer.html)  
- 📖 [Unity Manual – Render Texture](https://docs.unity3d.com/Manual/class-RenderTexture.html)  

---

# 🎵 Unity Audio Basics

This tutorial explains how to add and control **audio in Unity**.  
It covers importing audio clips, using Audio Sources & Listeners, creating spatial sound, mixing, and scripting audio playback.

---

## 📌 What You’ll Learn
- Import audio into Unity.  
- Play sounds with **Audio Source** and **Audio Listener**.  
- Set up **2D vs 3D spatial audio**.  
- Control audio using the **Audio Mixer**.  
- Trigger sounds with **C# scripts**.  

---

## 🧰 Step 1: Importing Audio

1. Drag your audio file (`.wav`, `.mp3`, `.ogg`) into the **Assets** folder.  
2. Select the clip → configure in **Inspector**:
   - ✅ **Loop** → for music or ambient sounds.  
   - ✅ **Load In Background** → avoids freezing during playback.  
   - ✅ **Preload Audio Data** → loads sound on startup.  

---

## 🔊 Step 2: Audio Source

The **Audio Source** plays the sound.  

1. Create a new GameObject → `GameObject > Audio > Audio Source`.  
2. In the **Inspector**, assign your audio clip.  
3. Key options:
   - **Loop** → repeats continuously.  
   - **Play On Awake** → starts automatically.  
   - **Spatial Blend** → choose between **2D (flat)** and **3D (positional)**.  
   
---

## 👂 Step 3: Audio Listener

- The **Audio Listener** represents the “ears” of the player.  
- Unity automatically attaches one to the **Main Camera**.  
- Only one active listener should exist in your scene.  


---

## 🎧 Step 4: 2D vs 3D Spatial Audio

- **2D Audio** → volume and pan are constant (use for UI sounds or music).  
- **3D Audio** → affected by distance and position (use for footsteps, gunfire, ambience).  

⚙️ To set:
- In **Audio Source** → adjust **Spatial Blend**:
  - `0` = 2D  
  - `1` = 3D  


---

## 🎚️ Step 5: Audio Mixer

The **Audio Mixer** allows you to balance music, SFX, and ambience.  

1. Create a Mixer → `Assets > Create > Audio Mixer`.  
2. Add groups (e.g., **Music**, **SFX**, **Ambience**).  
3. Assign an **Audio Source** to a group.  
4. Add effects (Reverb, Low-Pass Filter, Volume control).  

---

## 🧑‍💻 Step 6: Play Audio with Scripts

Example: play/pause audio when pressing **Spacebar**.

```csharp
using UnityEngine;

public class AudioExample : MonoBehaviour
{
    public AudioSource audioSource;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            if (!audioSource.isPlaying)
                audioSource.Play();
            else
                audioSource.Pause();
        }
    }
}
```

---

## 📌 Creative Core: Audio Tutorial
- Adding sound effects and music to games.
- Controlling playback with Unity’s audio system.
- Using the **Audio Source**, **Audio Listener**, and **Audio Mixer**.
- Creating immersive **3D spatial audio**.
- Using **scripts** to trigger sounds dynamically.
- https://learn.unity.com/project/creative-core-audio

---

# 🎇 Unity Particle System 

The **Unity Particle System** is a powerful feature that allows you to create visual effects such as fire, smoke, explosions, rain, sparks, and more.  
This guide provides a **beginner-friendly introduction** to using particle systems in your Unity projects.

---

## 📌 What is a Particle System?

A **particle system** is a collection of small images (called *particles*) that are animated and combined to create dynamic effects.  
Each particle can represent things like smoke puffs, fire sparks, or falling snowflakes.

Particles are controlled by a **Particle System Component**, which defines:
- How particles are emitted (rate, shape, bursts).
- How they look (material, color, size).
- How they behave over time (gravity, velocity, lifetime).

---

## ⚙️ Adding a Particle System

1. **In the Unity Editor**:
   - Right-click in the **Hierarchy** → `Effects` → `Particle System`.
   - A default particle system appears in your Scene.

2. **Adjust the Inspector**:
   - Select the Particle System in the Hierarchy.
   - The **Inspector** shows modules like *Emission, Shape, Size over Lifetime, Renderer,* etc.

---

## 🔑 Core Modules

| Module | Description |
|--------|-------------|
| **Main** | Controls overall settings like duration, looping, start lifetime, speed, size, and color. |
| **Emission** | Defines how many particles spawn per second or in bursts. |
| **Shape** | Determines the emission shape (cone, sphere, box, mesh, etc.). |
| **Velocity over Lifetime** | Adds directional movement to particles. |
| **Size over Lifetime** | Scales particles as they age. |
| **Color over Lifetime** | Gradually changes colors during particle lifespan. |
| **Renderer** | Defines how particles are drawn (material, shader, sorting). |

---

## 🎨 Example Effects

- **Fire** 🔥: Use a **cone shape**, orange/yellow gradient colors, and fast-moving particles.
- **Smoke** 💨: Use a **sphere shape**, gray color gradient with size growing over time.
- **Rain** 🌧️: Use a **box shape**, high emission rate, and downward velocity.
- **Sparkles** ✨: Small particles with random colors and short lifetimes.

---

## 🛠️ Customizing Materials

- By default, particles use Unity’s **Default Particle Material**.
- You can create a **new material** with a shader (e.g., `Particles/Standard Unlit`) and assign a custom texture (like smoke, spark, or snowflake sprite).

---

## 📜 Sample Script Control

You can also control particle systems using **C# scripting**:

```csharp
using UnityEngine;

public class ParticleTrigger : MonoBehaviour
{
    public ParticleSystem ps;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            ps.Play();   // Start particle emission
        }
        if (Input.GetKeyDown(KeyCode.S))
        {
            ps.Stop();   // Stop emission
        }
    }
}
```

---
# Unity Particle Systems 🌟

This project teaches you how to create visual effects like fire, smoke, snow, and rain using Unity’s Particle System. :contentReference[oaicite:0]{index=0}

---

## 🧱 Key Concepts & Components

| Component / Module | Purpose |
|---------------------|---------|
| **Particle System GameObject** | The container in the scene that emits particles. Created via **GameObject → Effects → Particle System** or added as a component. :contentReference[oaicite:5]{index=5} |
| **Main Module** | Core settings: lifetime, start speed, start size, start color, looping, etc. These define how particles behave overall. :contentReference[oaicite:6]{index=6} |
| **Emission Module** | Controls how many particles are emitted, rate over time, burst emission. :contentReference[oaicite:7]{index=7} |
| **Shape Module** | Defines the shape from which particles are emitted (sphere, cone, box, etc.). :contentReference[oaicite:8]{index=8} |
| **Other Modules** | Many additional modules like Velocity over Lifetime, Color over Lifetime, Size over Lifetime, Noise, etc. These let you fine-tune effects. :contentReference[oaicite:9]{index=9} |
| **Simulation Space** | Determines whether particles move in local space (following the emitter) or world space (independent once emitted). :contentReference[oaicite:10]{index=10} |

---

## ⚙️ Hands-On Steps

Here’s a simplified workflow to follow:

1. **Create a Particle System**  
   `GameObject > Effects > Particle System` (or add the component to an existing GameObject).  

2. **Configure the Main Module**  
   - Set **Looping** if you want continuous effects (snow, rain).  
   - Adjust **Start Lifetime**, **Start Speed**, **Start Size** to control how long particles live, how fast, and how big.  
   - Choose **Start Color** for the initial look.  

3. **Adjust Emission & Shape**  
   - Emission → change emission rate or set up bursts.  
   - Shape → select the shape of emission (cone, sphere, box, etc.) and adjust its parameters.  

4. **Preview & Tweak**  
   Move and rotate the emitter, watch how particles behave in real time. Adjust other modules like Color over Lifetime, Size over Lifetime, etc., to refine the visual.  

5. **Use Scripting (Optional but Powerful)**  
   Use code to start, stop, or modify Particle Systems at runtime.  
   Example stub:

```csharp
   using UnityEngine;

   public class ParticleController : MonoBehaviour
   {
       public ParticleSystem ps;

       void Start()
       {
           // Start emission
           ps.Play();
       }

       public void StopParticles()
       {
           ps.Stop();
       }
   }
```

---
   
## 📌 Getting Started: Particle Systems
- What the **Particle System** in Unity is for — visual effects, environmental effects, etc. :contentReference[oaicite:1]{index=1}  
- How to **add** a Particle System to your scene. :contentReference[oaicite:2]{index=2}  
- Core modules and properties of the Particle System (emission, shape, lifetime, speed, etc.). :contentReference[oaicite:3]{index=3}  
- How to script/control Particle Systems via code. :contentReference[oaicite:4]{index=4}  
- https://learn.unity.com/project/getting-started-with-particle-systems

---

# 🌟 Unity Lighting

Lighting is one of the most powerful tools in Unity for creating mood, realism, and atmosphere in your games. It not only affects how objects look but also impacts performance and player immersion.

---

## 🔦 What is Lighting in Unity?

In Unity, **lighting** determines how objects in a scene are illuminated and perceived by the camera. Lighting can simulate sunlight, lamps, glowing effects, or even abstract moods.

Unity’s Lighting system consists of three main parts:

1. **Light Components** (Directional, Point, Spot, Area)
2. **Global Illumination (GI)** (real-time or baked light bounces)
3. **Environment Lighting** (skybox, ambient light, reflection probes)

---

## 💡 Types of Light Components

Unity provides several built-in light types, each useful for different scenarios:

| Light Type                  | Use Case                                   | Icon | Notes                                              |
| --------------------------- | ------------------------------------------ | ---- | -------------------------------------------------- |
| **Directional Light**       | Simulates sunlight or moonlight            | ☀️   | Parallel rays; great for outdoor scenes.           |
| **Point Light**             | Emits light in all directions from a point | 💡   | Ideal for lamps, torches, glowing objects.         |
| **Spot Light**              | Cone-shaped light, like a flashlight       | 🔦   | Perfect for focused effects like stage lights.     |
| **Area Light** (Baked only) | Rectangular light source                   | 📐   | Great for soft indoor lighting, windows, or lamps. |

---

## ⚙️ Key Light Component Properties

When you select a Light in Unity, you’ll see these common settings in the **Inspector**:

* **Type** → Directional, Point, Spot, Area
* **Color** → Tint of the light (warm, cool, dramatic tones)
* **Intensity** → Brightness of the light
* **Range** (Point/Spot only) → Distance the light reaches
* **Spot Angle** (Spot only) → Width of the cone
* **Shadows** → Enable and adjust **Hard** or **Soft Shadows**
* **Mode** →

* **Realtime** – Updates every frame, dynamic but costly
* **Baked** – Precomputed, static, performance-friendly
* **Mixed** – Combines real-time and baked

---

## 🌍 Global Illumination (GI)

Lighting doesn’t stop at direct rays — **GI** simulates how light bounces around the scene.

* **Realtime GI** → Updates dynamically (good for day/night cycles).
* **Baked GI** → Precomputed for static environments (faster runtime performance).

Unity’s **Lighting Window** (`Window > Rendering > Lighting`) lets you configure GI, skyboxes, ambient light, and reflection probes.

---

## 🖼️ Environment Lighting

* **Skybox** → Wraps the scene with a 360° texture (day, night, fantasy worlds).
* **Ambient Light** → Adds soft light to areas not hit by direct lights.
* **Reflection Probes** → Capture reflections for shiny surfaces.

---

## 🎮 Hands-On Example

1. Create a **new Unity 3D Scene**.
2. Add a **Directional Light** (if not already present).
3. Change its **Rotation** to simulate different times of day.
4. Add a **Point Light** to a lantern or object.
5. Enable **Shadows** and experiment with **Intensity** and **Range**.
6. Open **Window > Rendering > Lighting** and adjust:

* Skybox material
* Ambient color
* Enable Baked GI

---

## 📚 Additional Resources

* [Unity Lighting Manual](https://docs.unity3d.com/Manual/Lighting.html)
* [Unity Learn – Lighting Fundamentals](https://learn.unity.com/tutorial/introduction-to-lighting)
* [Unity HDRP & URP Lighting](https://docs.unity3d.com/Packages/com.unity.render-pipelines.high-definition@latest)

---

## 📌 Creative Core: Lighting Tutorial
https://learn.unity.com/tutorial/get-started-with-lighting?version=6.0

---

## 📌 Live Session Video: Lighting
** This session explores different types of light and their specific use cases.**
You will learn about:
- color temperature, intensity and exposure
- the relationship between light and shadow and how these direct the viewer’s eye
- use volumetric light to add polish by creating “god rays,”
- how to output final shots via the recorder

- https://learn.unity.com/tutorial/lighting-july-28-2021

---

# 🎬 Unity DOTween

Tweening (short for *in-betweening*) lets you smoothly animate values over time, such as movement, rotation, scale, UI transitions, or color changes.

**[DOTween](http://dotween.demigiant.com/)** is a fast, efficient, full-featured tweening engine for Unity. It’s widely used in professional projects because of its flexibility and performance.

---

## 🚀 What is DOTween?

DOTween provides:

* ✨ **Powerful animations** for any value, not just transforms.
* 🔗 **Sequences** – chain multiple tweens in order.
* 🎛️ **Ease of use** – simple one-liners to animate properties.
* ⚡ **High performance** – optimized for mobile and consoles.
* 🛠️ **Extensibility** – works with custom variables, UI, shaders, and more.

---

## 📦 Installation

### Option 1: Unity Asset Store

1. Open **Unity Asset Store** inside the Unity Editor.
2. Search for **DOTween** and install the free version (or Pro for extra features).

### Option 2: Package from Website

1. Download DOTween from [dotween.demigiant.com](http://dotween.demigiant.com/).
2. Import the `.unitypackage` into your Unity project.
3. Run **DOTween Utility Panel** via `Tools > Demigiant > DOTween Utility Panel` to set up.

---

## 💡 Basic Usage

Here’s how to move a GameObject smoothly across the screen:

```csharp
using UnityEngine;
using DG.Tweening; // DOTween namespace

public class DoTweenExample : MonoBehaviour
{
    void Start()
    {
        // Move the object to position (5, 2, 0) over 2 seconds
        transform.DOMove(new Vector3(5f, 2f, 0f), 2f);
    }
}
```

---

## 🛠️ DOTween API Overview

### 1. Move Object

```csharp
transform.DOMove(Vector3 targetPosition, float duration);
```

Moves a GameObject to the target position.

---

### 2. Rotate Object

```csharp
transform.DORotate(new Vector3(0, 180, 0), 2f);
```

Rotates a GameObject smoothly.

---

### 3. Scale Object

```csharp
transform.DOScale(Vector3.one * 2f, 1f);
```

Doubles the size of the object in 1 second.

---

### 4. Color Tween

```csharp
spriteRenderer.DOColor(Color.red, 1.5f);
```

Tweens a sprite’s color over 1.5 seconds.

---

### 5. UI Tweening

```csharp
myText.DOFade(0f, 2f);  // fade out text
myImage.DOFillAmount(1f, 3f); // fill radial image
```

---

### 6. Sequences

Chain multiple animations together:

```csharp
Sequence mySequence = DOTween.Sequence();
mySequence.Append(transform.DOMoveX(3, 1f))
          .Append(transform.DOScale(Vector3.one * 2, 0.5f))
          .Append(transform.DORotate(new Vector3(0, 180, 0), 1f))
          .OnComplete(() => Debug.Log("Sequence Finished!"));
```

---

### 7. Looping

```csharp
transform.DOMoveY(3f, 1f).SetLoops(-1, LoopType.Yoyo);
```

Makes the object bounce up and down forever.

---

### 8. Callbacks

```csharp
transform.DOMoveX(5, 2f).OnComplete(() => Debug.Log("Done!"));
```

---

## 🎮 Tutorial: Bouncy UI Button

Let’s animate a UI button to “bounce” when clicked:

```csharp
using UnityEngine;
using UnityEngine.UI;
using DG.Tweening;

public class DoTweenButton : MonoBehaviour
{
    public Button myButton;

    void Start()
    {
        myButton.onClick.AddListener(() =>
        {
            myButton.transform.DOScale(Vector3.one * 1.2f, 0.2f)
                .SetEase(Ease.OutBack)
                .OnComplete(() =>
                {
                    myButton.transform.DOScale(Vector3.one, 0.2f).SetEase(Ease.InBack);
                });
        });
    }
}
```

✅ Result: The button pops when clicked with a smooth easing curve.

---

## 📸 Example Scenarios

* Smooth **camera transitions**
* UI elements sliding in/out
* Looping **background animations**
* Complex cutscene animations using **Sequences**
* Sprite flashing or color transitions

---

## 📚 Resources

* [DOTween Official Documentation](http://dotween.demigiant.com/documentation.php)
* [DOTween Pro](http://dotween.demigiant.com/pro.php) – extended features for UI and visual tools

---

# 🎬 Unity Animation System & Mixamo Integration

Animating characters brings your Unity projects to life.  
This guide covers **Unity’s Animation System** and provides a **hands-on workflow to import Mixamo animations**.

---

## 📌 What is the Unity Animation System?

The **Unity Animation System** allows you to animate characters and objects using:

- **Keyframe Animation**: Manual animation inside Unity.
- **Imported Animations**: From tools like Mixamo, Blender, or Maya.
- **Animator Controller**: A state machine that manages multiple animations and transitions.

### Key Concepts

| Concept | Description |
|---------|-------------|
| **Animator Component** | Attaches to a GameObject to control animations. |
| **Animation Clip** | A single animation sequence (e.g., walk, run, jump). |
| **Animator Controller** | Organizes animation clips in states and transitions. |
| **Blend Trees** | Smoothly blend between multiple animations based on parameters. |
| **Root Motion** | Moves the GameObject in world space according to the animation. |

---

## ⚙️ Unity Animator Setup

1. **Add Animator Component**
   - Select your character in the **Hierarchy**.
   - Click **Add Component → Animator**.
   - Create or assign an **Animator Controller**.

2. **Open Animator Controller**
   - Double-click the Animator Controller in **Project Window**.
   - Add states by dragging animation clips into the Animator.
   - Define **transitions** between states based on conditions like speed or triggers.

---

## 🌐 Using Mixamo Animations in Unity

**Mixamo** provides free rigged characters and pre-made animations. Follow these steps to integrate Mixamo animations into Unity.

### Step 1: Choose or Upload Character
1. Go to [Mixamo.com](https://www.mixamo.com/).
2. Select a character from the library or upload your own rigged character.

### Step 2: Pick Animations
1. Search for the desired animation (walk, run, jump, attack, etc.).
2. Click the animation to preview it in the Mixamo viewer.

### Step 3: Download Animation
1. Click **Download**.
2. Choose **FBX for Unity**.
3. Set **Pose → T-Pose**.
4. Choose **With Skin** (for full character) or **Without Skin** (if adding only animation).

### Step 4: Import into Unity
1. Drag the downloaded FBX into **Assets** in Unity.
2. Select the FBX file, go to **Rig** tab, and set:
   - **Animation Type → Humanoid**
   - Click **Apply**.
3. Unity automatically generates **Animation Clips** from the FBX.

### Step 5: Configure Animator Controller
1. Open your **Animator Controller**.
2. Drag the imported Animation Clips into the Animator window.
3. Create **transitions** between animations.
   - Example: Idle → Walk → Run
4. Add **parameters**:
   - Float: `Speed` for movement
   - Trigger: `Jump` for jump animation

### Step 6: Enable Root Motion (Optional)
- If the animation moves the character in world space (e.g., walking forward), check **Apply Root Motion** in the Animator.

---

## 🛠️ Controlling Animations via Script

```csharp
using UnityEngine;

public class CharacterController : MonoBehaviour
{
    public Animator animator;

    void Update()
    {
        float move = Input.GetAxis("Vertical");
        animator.SetFloat("Speed", move);

        if (Input.GetButtonDown("Jump"))
        {
            animator.SetTrigger("Jump");
        }
    }
}
```

---

## 📌 Getting Started: Animator Controller
https://learn.unity.com/course/introduction-to-3d-animation-systems/unit/the-animator

---

# 🎥 Unity Cinemachine

**Unity Cinemachine** is a powerful camera system that allows you to create dynamic, procedural, and cinematic camera behavior without writing complex code.  

---

## 📌 What is Cinemachine?

Cinemachine is Unity’s advanced camera system, providing:

- **Dynamic Camera Movement**: Automatically follows characters, objects, or paths.
- **Procedural Camera Control**: Adjusts framing, zoom, and rotation based on target behavior.
- **Cinematic Shots**: Supports cutscenes, smooth transitions, and complex camera rigs.

Cinemachine works alongside Unity’s **Timeline** for cinematic sequences.

---

## ⚙️ Installing Cinemachine

1. Open **Unity Package Manager** (`Window → Package Manager`).
2. Search for **Cinemachine**.
3. Click **Install**.
4. After installation, you can access Cinemachine via `GameObject → Cinemachine`.

---

## 🔑 Core Components

| Component | Description |
|-----------|-------------|
| **Cinemachine Virtual Camera** | A camera that follows or looks at targets. Can be blended with other cameras. |
| **Cinemachine Brain** | Attached to the main Unity Camera to interpret virtual cameras. |
| **Cinemachine FreeLook** | A 3D orbiting camera, ideal for third-person perspectives. |
| **Cinemachine Dolly Track** | Creates cameras that follow a predefined path. |
| **Cinemachine ClearShot** | Automatically switches between multiple virtual cameras based on priority and visibility. |

---

## 🛠️ Creating a Basic Camera Setup

1. **Add Main Camera** (if none exists) and ensure it has a **Cinemachine Brain** component.
2. **Create a Virtual Camera**:
   - `GameObject → Cinemachine → Create Virtual Camera`
   - Assign a **Target** (e.g., the player character).
3. Adjust **Follow** and **Look At** fields:
   - **Follow**: the object the camera moves with.
   - **Look At**: the object the camera points at.
4. Play the scene to see the camera follow and track the target dynamically.

---

## 🎯 FreeLook Camera for Third-Person

- Create a FreeLook camera: `GameObject → Cinemachine → FreeLook Camera`.
- Set the **Follow** and **Look At** targets.
- Adjust **Rig Settings**:
  - **Top Rig / Middle Rig / Bottom Rig**: Control height and radius of orbits.
  - **Axis Control**: Adjust speed and responsiveness.
- Ideal for games like third-person adventure, platformers, and RPGs.

---

## 🎬 Dolly Tracks & Cinematics

1. Create a **Cinemachine Dolly Track**: `GameObject → Cinemachine → Dolly Track with Cart`.
2. Place **Waypoints** along the path.
3. Assign **Virtual Camera** to the **Dolly Cart**.
4. Animate the cart using **Timeline** for cinematic sequences or cutscenes.

---

## 📜 Sample Script Control

You can switch cameras dynamically with C#:

```csharp
using UnityEngine;
using Cinemachine;

public class CameraSwitcher : MonoBehaviour
{
    public CinemachineVirtualCamera cam1;
    public CinemachineVirtualCamera cam2;

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Alpha1))
        {
            cam1.Priority = 10;
            cam2.Priority = 0;
        }
        if (Input.GetKeyDown(KeyCode.Alpha2))
        {
            cam1.Priority = 0;
            cam2.Priority = 10;
        }
    }
}
```

---

## 📌 Getting Started: Cinemachine Tutorial
https://learn.unity.com/tutorial/cinemachine-cameras-2018

---

# 📱 Unity Mobile App Build Process

This guide covers **how to build and publish Unity applications** for **Android** and **iOS** platforms.  

---

## 📌 Prerequisites

### Common Requirements
- Unity installed with **Android Build Support** and/or **iOS Build Support**.
- **Scripting Backend**: IL2CPP recommended.
- **Player Settings** configured (bundle ID, version, orientation, icons).
- Device for testing (Android phone/tablet or iPhone/iPad).

### Tools
- **Android**:
  - Android Studio (for SDK & NDK)
  - Java JDK (usually comes with Android Studio)
- **iOS**:
  - Xcode installed on macOS

---

## ⚙️ Configuring Player Settings

1. Open **File → Build Settings → Player Settings**.
2. Configure the following:

| Setting | Android | iOS |
|---------|---------|-----|
| **Company & Product Name** | ✅ | ✅ |
| **Package/Bundle Identifier** | `com.yourcompany.app` | `com.yourcompany.app` |
| **Version & Build Number** | ✅ | ✅ |
| **Orientation** | Portrait / Landscape | Portrait / Landscape |
| **Icon & Splash Screen** | ✅ | ✅ |
| **Minimum OS Version** | API level 19+ (Android) | iOS 11+ |

---

## 📦 Android Build Process

1. Open **File → Build Settings → Android** → Switch Platform.
2. Connect **Android device** (optional) or build APK/ABB.
3. Configure **Build Settings**:
   - Target: **Google Android**
   - Texture Compression: **ASTC recommended**
   - Build System: **Gradle**
4. Click **Build** or **Build and Run**.
5. Locate APK/AAB file in chosen directory.
6. Test the build on a real device.

**Publishing to Google Play:**
1. Sign APK/AAB using a keystore.
2. Upload the signed APK/AAB to [Google Play Console](https://play.google.com/console).
3. Fill store listing, screenshots, and submit for review.

Official Unity docs: [Android Build Process](https://docs.unity3d.com/6000.2/Documentation/Manual/android-BuildProcess.html)

---

## 📦 Google Play Setup
- https://developer.android.com/games/pgs/unity/unity-start

---

## 🍎 iOS Build Process

1. Open **File → Build Settings → iOS** → Switch Platform.
2. Click **Build** → choose a folder → Unity generates an **Xcode project**.
3. Open the generated project in **Xcode**.
4. Configure in Xcode:
   - **Signing & Capabilities** → select team and provisioning profile.
   - **Bundle Identifier** matches Player Settings in Unity.
   - Target device and deployment target.
5. Build and run on device or simulator.

**Publishing to App Store:**
1. Archive the app in Xcode (`Product → Archive`).
2. Upload to App Store Connect.
3. Fill app metadata, screenshots, and submit for review.

Official Unity docs: [iOS Build Process](https://docs.unity3d.com/6000.2/Documentation/Manual/iphone-BuildProcess.html)

---

## 🍎 IOS Build Tutorial
- https://learn.unity.com/tutorial/publishing-for-ios

---

## 🛠️ Build Tips

- Use **IL2CPP** for better performance and app store compliance.
- Enable **Development Build** for testing with logs.
- Test frequently on real devices to check performance and resolution.
- Optimize textures, audio, and assets for mobile performance.
- Keep Android min SDK and iOS deployment target consistent with your target devices.

---

## 📚 Further Learning

- [Unity Manual: Android Build](https://docs.unity3d.com/6000.2/Documentation/Manual/android-BuildProcess.html)  
- [Unity Manual: iOS Build](https://docs.unity3d.com/6000.2/Documentation/Manual/iphone-BuildProcess.html)  

---

# 📱 Unity Remote 

**Unity Remote** is a tool that allows you to **test and play your Unity project on a mobile device in real-time** without building the full application.  
This guide explains Unity Remote and provides a step-by-step tutorial for setup and use.

Unity Remote lets you:

- Stream your **Game View** from the Unity Editor to a mobile device.
- Test **touch input, accelerometer, and device sensors** without building the full APK or iOS app.
- Rapidly iterate on mobile UI and controls during development.

> Note: Unity Remote is for **testing only**; final performance may differ on actual builds.

---

## ⚙️ Prerequisites

### Common Requirements

- Unity installed.
- Mobile device (Android or iOS).
- USB cable for device connection.
- Unity Remote app installed on the device:
  - **Android**: [Unity Remote 5 on Google Play](https://play.google.com/store/apps/details?id=com.unity3d.unityremote5)
  - **iOS**: [Unity Remote 5 on App Store](https://apps.apple.com/us/app/unity-remote-5/id1089798891)

---

## 🔧 Setting Up Unity Remote

### Step 1: Connect Your Device

1. Plug your mobile device into your computer via USB.
2. Enable **Developer Mode / USB Debugging**:
   - Android: Enable **Developer Options → USB Debugging**.
   - iOS: Enable **Developer Mode** in Settings.

---

### Step 2: Configure Unity Editor

1. Open **Edit → Project Settings → Editor**.
2. Under **Unity Remote**, set:
   - **Device**: `Any Android Device` or `Any iOS Device`
   - **Compression Method**: `JPEG` or `PNG`
   - **Resolution**: `Original` or `Scaled`
3. Open **File → Build Settings → Platform**:
   - Ensure your platform matches your device (Android or iOS).

---

### Step 3: Launch Unity Remote

1. Open the **Unity Remote app** on your device.
2. Click **Play** in the Unity Editor.
3. The Game View will stream directly to your mobile device.
4. Interact with the game using **touch, tilt, or other mobile inputs**.

---

## 🛠️ Testing Controls and Input

- **Touch Input**: Unity Remote sends touch positions from device to Editor.
- **Accelerometer**: Device tilt affects `Input.acceleration` in Unity.
- **Gyroscope**: Device rotation affects `Input.gyro` data.

**Example Script for Touch Input:**

```csharp
using UnityEngine;

public class TouchExample : MonoBehaviour
{
    void Update()
    {
        if (Input.touchCount > 0)
        {
            Touch touch = Input.GetTouch(0);
            Vector3 worldPos = Camera.main.ScreenToWorldPoint(touch.position);
            Debug.Log("Touch position: " + worldPos);
        }
    }
}
````

Attach this to a GameObject and see touch positions logged while using Unity Remote.

---

## 🚀 Tips

* Use **Unity Remote** early in development to quickly iterate on mobile UI and gameplay.
* Remember **performance will differ** from actual builds; use Unity Remote for input and layout testing only.
* Disable **Unity Remote** for final testing on actual device builds to check performance, resolution, and frame rate.

---

## 📚 Further Learning

* [Unity Manual: Unity Remote](https://docs.unity3d.com/Manual/UnityRemote5.html)
* [Unity Learn: Mobile Development](https://learn.unity.com/tutorial/mobile-game-development)

---

## Mobile Development Tutorial

This endless runner game example (Trash Dash) is optimised for mobile, it shows the use of object pooling, origin reset, asset bundles, additive scene loading and a curved world shader.
- [Endless Runner Sample Game](https://learn.unity.com/tutorial/mobile-development-techniques)

---

# Interactive Guide: Cross-Platform UI
The Interactive Guide to Cross-Platform UI is a set of guided, in-editor tutorials for experienced Unity creators seeking to learn the workflows used for creating cross-platform user interfaces.
- [Cross-Platform UI Project](https://assetstore.unity.com/packages/templates/tutorials/interactive-guide-cross-platform-ui-208063)

---

# Touch Input System
- https://discussions.unity.com/t/get-up-and-running-with-the-input-system/1620267

---

# Unity Multiplayer with Netcode for GameObjects 🚀

Build scalable and fun multiplayer experiences in Unity using **Netcode for GameObjects (NGO)**. This guide introduces the basics of setting up a multiplayer project, creating networked players, and syncing game state across clients.

---

## 📖 Overview

Unity provides multiple networking solutions, but **Netcode for GameObjects** is the recommended starting point for small to mid-sized multiplayer games such as:

* Co-op adventures
* Competitive arena shooters
* Casual multiplayer experiences

**Key Features of NGO** :

* **NetworkObject** – sync any GameObject across clients (spawn, despawn, ownership).
* **NetworkBehaviour** – extend MonoBehaviour with networking capabilities.
* **NetworkVariables** – synchronize data continuously across clients.
* **Remote Procedure Calls (RPCs)** – trigger events or actions remotely.
* **NetworkManager** – central controller for connections, transport, and session management.

---

## ⚙️ Getting Started

### 1. Install Dependencies

* Unity **6.0 or higher** (LTS recommended)
* Add **Netcode for GameObjects** via **Package Manager**

  * Go to `Window > Package Manager` → select **Unity Registry** → install `Netcode for GameObjects` .

### 2. Add the **NetworkManager**

1. Create an empty GameObject → add **NetworkManager** (`Netcode > NetworkManager`) .
2. Select a transport layer (e.g., **Unity Transport**).
3. Save the scene and add it to **Build Settings**.

---

## 🧑‍🤝‍🧑 Player NetworkObjects

Each player is represented by a **Player NetworkObject** :

* Contains the **character controller** and player visuals.
* Stores and syncs **player-specific data** (name, score, inventory).
* Owned by the client who controls it.

🔧 Setup:

1. Create a prefab of your player GameObject.
2. Add:

   * **NetworkObject**
   * **NetworkBehaviour** (for logic, variables, RPCs)
   * **NetworkTransform** (syncs movement)
   * **NetworkAnimator** (syncs animations)
3. Register the prefab in the **NetworkManager > Player Prefab** slot .

---

## 🔄 Synchronizing Data

### NetworkVariables

Best for **continuous state** (health, position, score):

```csharp
public class PlayerStats : NetworkBehaviour {
    public NetworkVariable<int> Health = new NetworkVariable<int>(100);

    public void TakeDamage(int amount) {
        if (IsServer) {
            Health.Value -= amount;
        }
    }
}
```

### RPCs

Best for **discrete events** (shooting, ability use):

```csharp
[ServerRpc]
void ShootServerRpc(Vector3 position) {
    // Spawn bullet on server and sync across clients
}
```

📌 Use **NetworkVariables** for states, **RPCs** for events .

---

## 🎮 Testing Multiplayer

Unity 6 introduces **Multiplayer Play Mode (MPPM)** :

* Run multiple editor instances at once.
* Simulate host + clients without building separate executables.
* Configure under `Window > Multiplayer Play Mode`.

---

## 🛠️ Authority & Ownership

NGO is **server-authoritative** by default :

* **Server** controls spawning, despawning, and final state.
* Clients can **own objects** using `SpawnWithOwnership`.

Useful checks inside scripts:

* `IsServer` → running on server
* `IsClient` → running on client
* `IsOwner` → current player owns this object
* `IsLocalPlayer` → player object of this client

---

## 📊 Handling Latency

Latency can cause **rubber-banding** and **desync**. Unity provides :

* **Client-side prediction & reconciliation**
* **Unity Transport Debug Simulator** → simulate packet loss, jitter, lag
* **Interpolation & anticipation** for smoother gameplay

---

## 📚 Sample Projects & Resources

* [Unity Learn: Get Started with NGO](https://learn.unity.com/tutorial/get-started-with-netcode-for-gameobjects#)
* [Boss Room (Co-op Sample)](https://github.com/Unity-Technologies/com.unity.multiplayer.samples.coop)
* [Bitesize NGO Samples](https://github.com/Unity-Technologies/com.unity.multiplayer.samples.bitesize)

---

## 🎮 Getting Started: Netcode for GameObjects Tutorial
- https://learn.unity.com/tutorial/get-started-with-netcode-for-gameobjects

---

- https://github.com/Unity-Technologies/com.unity.netcode.gameobjects
- https://github.com/Unity-Technologies/com.unity.multiplayer.samples.bitesize
- https://github.com/Unity-Technologies/com.unity.multiplayer.samples.coop
- https://github.com/Unity-Technologies/InputSystem

--- 



