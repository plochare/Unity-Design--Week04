# Unity Design - Week 04

## Agenda

7. [Animator Controllers / Mixamo](#Animator-Controller)
8. [Cinemachine](#Cinemachine)

1. [Unity Mobile App Build](#Unity-Mobile-App-Build-Process)  
2. [Unity Remote](#Unity-Remote)  
3. [Cross-Platform UI](#Cross-Platform-UI)
4. [Unity Multiplayer with Netcode for GameObjects](#Unity-Multiplayer-Netcode)  
5. [Class Assignment - Week #04](#Create-with-Code)  


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



