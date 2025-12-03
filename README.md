# OptimizedVehicleSystem - Documentation & Quick Start

## Overview
The **OptimizedVehicleSystem** is a high-performance, modular vehicle plugin for Unreal Engine 5. It allows you to create complex vehicles (Cars, Trucks, Bikes, Helicopters, and Hybrids) using **Static Meshes only**.

**Key Features:**
*   **Performance:** Runs on a custom asynchronous physics tick (`NativeAsyncTick`), offering significant performance gains over native Chaos vehicles.
*   **Modular Design:** No skeletal meshes required. Snap components together (Wheels, Engines, Rotors) to build any vehicle type.
*   **Hybrid Support:** Seamlessly transition between Ground driving and Flight physics in real-time.
*   **Bike Physics:** dedicated physics solver for 2-wheeled vehicles (leaning, counter-steering, stability).
*   **Full AI Support:** Includes a complete AI stack (Behavior Trees, Tasks, Services) and a simple "Move To" Async node for Blueprints.

---

## 1. Project Setup

1.  Ensure the **OptimizedVehicleSystem** plugin is enabled in `Edit > Plugins`.
2.  Ensure **Enhanced Input** is enabled in Project Settings.

---

## 2. Creating a Vehicle

### Step 1: Create the Pawn
1.  Create a new **Blueprint Class**.
2.  Search for and select **Pawn** (or `OptimizedTickPawn` for C++ extension).
3.  Name it (e.g., `BP_ModularCar`).

### Step 2: Setup the Chassis (Root)
*The system relies on the physical body being the Root Component.*
1.  Open the Blueprint.
2.  Drag your chassis **Static Mesh** to replace the `DefaultSceneRoot`.
3.  **Details Panel settings:**
    *   **Simulate Physics:** `True`
    *   **Mass (kg):** Set a realistic mass (e.g., 1500 for a car).
    *   **Collision:** Use `PhysicsActor` or `BlockAll`.

### Step 3: Add the Core
1.  Add the `ModularVehicleCore` component.
2.  This component automatically scans for attached parts (Wheels, Engines, Rotors) at `BeginPlay`.
3.  **Key Settings:**
    *   **Auto Calibrate Suspension:** If true, automatically calculates spring stiffness based on vehicle mass.
    *   **Arcade Drift Assist:** Helps align the vehicle with velocity during drifts (0.0 = Realism, 1.0 = Arcade).

---

## 3. Modular Components

### A. Modular Engine (Powertrain & Audio)
Adds torque and manages gear ratios.
*   **Add Component:** `ModularEngineComponent`.
*   **Audio:** Assign an `EngineSound`. The pitch automatically adjusts based on simulated RPM.
*   **Transmission:** Configurable Gear Ratios and Differential Ratio.
*   **Setup:** You usually only need one, but you can add multiple for hybrid setups.

### B. Modular Wheels (Suspension & Traction)
Handles raycasting, suspension physics, and friction.
*   **Add Component:** `ModularWheelComponent`.
*   **Visuals:** Assign a Static Mesh to `Wheel Mesh`. Use `Flip Visual Rotation` if it faces the wrong way.
*   **Configuration:**
    *   **bIsSteerWheel:** Check for front wheels.
    *   **bIsDriveWheel:** Check to apply engine torque (e.g., check Rear wheels only for RWD).
    *   **bIsHandbrakeWheel:** Check for wheels affected by the handbrake.
*   **Placement:** Place the component exactly where the wheel hub should be. The visual mesh will spawn there.

### C. Modular Rotors (Flight)
Adds vertical lift and flight control torque.
*   **Add Component:** `ModularRotorComponent`.
*   **Visuals:** Assign a Propeller mesh. It handles spool-up rotation and audio automatically.
*   **Physics:**
    *   `LiftPowerMultiplier`: Increases vertical thrust efficiency.
    *   `MaxRotationSpeed`: Visual speed of the blades.

### D. Modular Steering Wheel (Interior)
Purely visual component that rotates based on steering input.
*   **Add Component:** `ModularSteeringWheelComponent`.
*   **Setup:** Assign your steering wheel mesh.
*   **Axis:** Select which axis rotates (Roll, Pitch, or Yaw) to match your mesh's pivot.

---

## 4. Vehicle Types & Configurations

### Standard 4-Wheeled Car
*   1x Chassis (Root)
*   1x Core
*   1x Engine
*   4x Wheels (2x Steer, 2x Drive/Handbrake)

### Motorbikes
The system includes a dedicated controller for 2-wheeled physics.
1.  Setup a vehicle with exactly **2 Modular Wheels**.
2.  Select `ModularVehicleCore`.
3.  Check **bIsBikeConfiguration**.
4.  **Tuning:**
    *   `BikeMaxLeanAngle`: How far the bike tilts when turning.
    *   `BikeLowSpeedStiffness`: Keeps the bike upright when stopped/slow.

### Hybrids (Car + Heli)
1.  Setup a car as normal.
2.  Add **Modular Rotor** components (e.g., one main rotor, one tail rotor, or quadcopter layout).
3.  Map the `ToggleFlightModeAction` input.
4.  When activated, the Core disables wheel torque and enables rotor physics.

---

## 5. Input Configuration
Inputs are handled via Enhanced Input directly on the `ModularVehicleCore`.

1.  Select `ModularVehicleCore`.
2.  Go to the **Input Settings** category.
3.  Assign your **Input Mapping Context** (IMC).
4.  Bind the specific Actions:
    *   **Throttle/Steering/Handbrake:** Standard driving.
    *   **EngineStartAction:** Toggles engine on/off.
    *   **ToggleFlightModeAction:** Switches between Ground and Air physics.
    *   **VerticalThrust/Pitch/Yaw/Roll:** Flight controls.

---

## 6. AI System (New)

The plugin comes with a fully integrated AI Controller and Behavior Tree system specifically designed for these physics vehicles.

### Setup AI
1.  Add the `ModularVehicleAIComponent` to your Vehicle Pawn.
2.  The component will automatically spawn the `ModularVehicleAIController`.
3.  **Behavior Tree:** The plugin includes a default BT, but you can assign your own in the component settings.

### Moving the Vehicle (Blueprint)
You don't need to touch the Behavior Tree manually for simple movements. Use the **Async Action**:

1.  In your GameMode or Level Blueprint, right-click and search for **"Modular Vehicle Move To"**.
2.  **Target Location:** Where to go.
3.  **Prioritize Driving:**
    *   `True`: The AI will drive on the ground.
    *   `False`: The AI will fly (if it has rotors).
4.  **Flight Config:** Customize cruising altitude and landing behavior.
5.  **Outputs:** Use the `On Arrived` or `On Failed` pins to handle logic.

### AI Features
*   **Pathfinding:** Uses Navigation Mesh for ground vehicles.
*   **Smart Cornering:** Slows down before sharp turns (Calculated in `BTService_GroundPathPlanner`).
*   **Obstacle Recovery:** Auto-detects if the vehicle is stuck and attempts 3-point turns or air recovery maneuvers.
*   **Hybrid Logic:** The AI intelligently switches between driving and flying based on the `PrioritizeDriving` boolean and available parts.

---

## 7. Troubleshooting

*   **Vehicle won't move:**
    *   Is the Engine on? (Map the Start Engine input).
    *   Do you have `bIsDriveWheel` checked on at least one wheel?
    *   Is the Handbrake engaged?
*   **Vehicle flips immediately:**
    *   Check your Physics Asset collisions.
    *   Ensure `CenterOfMassOffset` in the Core settings is low (negative Z value).
*   **AI creates path but doesn't move:**
    *   Ensure a NavMeshBoundsVolume is present in the level.
    *   Check if `DesiredSpeed` in the AI Blackboard is being set (Debug via Behavior Tree view).
*   **Bike falls over:**
    *   Ensure `bIsBikeConfiguration` is checked.
    *   Increase `BikeUprightStiffness` in the Core settings.
