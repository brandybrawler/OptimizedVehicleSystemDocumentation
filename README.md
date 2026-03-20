# OptimizedVehicleSystem — Documentation & Quick Start

## Overview

The **OptimizedVehicleSystem** is a high-performance, modular vehicle plugin for Unreal Engine 5. It allows you to create complex vehicles (Cars, Trucks, Bikes, Helicopters, and Hybrids) using **Static Meshes only** — no skeletal mesh rigging required.

**What's Included:**
- **Modular Vehicle Physics** — Cars, trucks, bikes, helicopters, and hybrid VTOL vehicles.
- **Complete AI Traffic System** — Hundreds of AI vehicles driving roads, obeying traffic rules, and following each other at safe distances using the Mass Entity Framework.
- **Vehicle Damage** — Real-time mesh deformation with part detachment.
- **Dynamic Mud Terrain** — Vehicles sink and leave tracks in deformable mud pits.
- **14 Vehicle Blueprints** — Ready-to-use cars, trucks, and helicopters.
- **City Environment** — A full city level with buildings, road network, and pre-configured traffic. Import the plugin, open the level, and press Play.

**Performance:** Runs on a custom asynchronous physics tick (`NativeAsyncTick`), offering significant performance gains over native Chaos vehicles.

---

## 1. Project Setup

1. Ensure the **OptimizedVehicleSystem** plugin is enabled in `Edit > Plugins`.
2. Ensure **Enhanced Input** is enabled in Project Settings.

### Plug & Play Demo

The plugin includes a complete city level that works out of the box:

1. Import the plugin into any UE5 project.
2. Open the city level from the plugin's Content folder.
3. Press Play.

You'll immediately see AI vehicles driving roads, helicopters flying overhead, and traffic flowing. No configuration required.

---

## 2. Creating a Vehicle

### Step 1: Create the Pawn

1. Create a new **Blueprint Class**.
2. Search for and select **Pawn** (or `OptimizedTickPawn` for C++ extension).
3. Name it (e.g., `BP_ModularCar`).

### Step 2: Setup the Chassis (Root)

*The system relies on the physical body being the Root Component.*

1. Open the Blueprint.
2. Drag your chassis **Static Mesh** to replace the `DefaultSceneRoot`.
3. **Details Panel settings:**
   - **Simulate Physics:** `True`
   - **Mass (kg):** Set a realistic mass (e.g., 1500 for a car).
   - **Collision:** Use `PhysicsActor` or `BlockAll`.

### Step 3: Add the Core

1. Add the `ModularVehicleCore` component.
2. This component automatically scans for attached parts (Wheels, Engines, Rotors) at `BeginPlay`.
3. **Key Settings:**
   - **Auto Calibrate Suspension:** Automatically calculates spring stiffness based on vehicle mass.
   - **Arcade Drift Assist:** Aligns the vehicle with velocity during drifts (`0.0` = Realism, `1.0` = Arcade).
   - **Center Of Mass Offset:** Adjust the center of mass — lower Z values improve stability.

---

## 3. Modular Components

### A. Modular Engine (Powertrain & Audio)

Adds torque and manages gear ratios.

- **Add Component:** `ModularEngineComponent`
- **Audio:** Assign an `EngineSound`. Pitch automatically adjusts based on simulated RPM.
- **Transmission:** Configurable Gear Ratios, Differential Ratio, and automatic/manual shifting.
- **Setup:** Usually one engine, but you can add multiple for hybrid setups.

### B. Modular Wheels (Suspension & Traction)

Handles raycasting, suspension physics, and friction.

- **Add Component:** `ModularWheelComponent`
- **Visuals:** Assign a Static Mesh to `Wheel Mesh`. Use `Flip Visual Rotation` if it faces the wrong way.
- **Configuration:**
  - `bIsSteerWheel` — Check for front wheels.
  - `bIsDriveWheel` — Check to apply engine torque (e.g., rear wheels only for RWD).
  - `bIsHandbrakeWheel` — Check for wheels affected by the handbrake.
- **Placement:** Position the component exactly where the wheel hub should be.

### C. Modular Rotors (Flight)

Adds vertical lift and flight control torque.

- **Add Component:** `ModularRotorComponent`
- **Visuals:** Assign a propeller mesh. Handles spool-up rotation and audio automatically.
- **Key Settings:**
  - `LiftPowerMultiplier` — Vertical thrust efficiency (default `2.5x` gravity).
  - `MaxRotationSpeed` — Visual blade rotation speed.

### D. Modular Steering Wheel (Interior)

Purely visual component that rotates based on steering input.

- **Add Component:** `ModularSteeringWheelComponent`
- **Setup:** Assign your steering wheel mesh.
- **Axis:** Select which axis rotates (Roll, Pitch, or Yaw) to match your mesh's pivot.

### E. Modular Damage (Vehicle Destruction)

Adds impact-based mesh deformation and part detachment.

- **Add Component:** `ModularDamageComponent`
- **Key Settings:**
  - `DeformationRadius` — Radius around impact point to deform (default `40 cm`).
  - `DeformationStrength` — Intensity multiplier (default `1.0`).
  - `MaxDeformationDepth` — Maximum deformation depth (default `15 cm`).
  - `MinImpactForceThreshold` — Minimum force to trigger damage (default `5000`).
  - `bUseAsyncDeformation` — Async processing to prevent frame hitches (default `true`).
  - `MaxVerticesPerDeform` — Vertex budget per impact (default `300`, range `50–2000`).
- **Part Detachment:**
  - `bEnablePartDetachment` — Enable the part state system.
  - `DamageToHangThreshold` — Damage % to enter "hanging" state (default `60%`).
  - `DamageToDetachThreshold` — Damage % to fully detach (default `90%`).
  - `HangingSwingLimit` — Max swing angle for hanging parts (default `45°`).
  - `DetachedPartLifetime` — Seconds before detached parts are removed (default `10s`).
- **Damage Zones:** Define named zones (hood, doors, bumper) with independent damage tracking.
- **Events:** `OnVehicleDamaged` and `OnPartStateChanged` delegates for gameplay integration.

---

## 4. Vehicle Types & Configurations

### Standard 4-Wheeled Car

- 1x Chassis (Root), 1x Core, 1x Engine, 4x Wheels (2x Steer, 2x Drive/Handbrake)

### Motorbikes

1. Setup a vehicle with exactly **2 Modular Wheels**.
2. Check `bIsBikeConfiguration` on the Core component.
3. **Tuning:**
   - `BikeMaxLeanAngle` — How far the bike tilts when turning (default `40°`).
   - `BikeLowSpeedStiffness` — Keeps the bike upright when stopped (default `250`).
   - `BikeUprightStiffness` — Stiffness returning to upright (default `30`).
   - `BikeMinSpeedForLean` — Minimum speed before leaning activates (default `15`).

### Hybrids (Car + Helicopter)

1. Setup a car as normal.
2. Add **Modular Rotor** components (main rotor + tail rotor, or quadcopter layout).
3. Map the `ToggleFlightModeAction` input.
4. When activated, the Core disables wheel torque and enables rotor physics.

### Drift Vehicles

The Core component has a dedicated drift system:

- `bCanDrift` — Enable drift mechanics (default `true`).
- `ArcadeDriftAssist` — Align vehicle with velocity during drifts (`0.0–1.0`).
- `AutomaticCounterSteer` — Auto counter-steer assist (default `0.0`).
- `MaxDriftAngle` — Maximum slide angle in degrees (default `50°`).
- `DriftPushForce` — Extra forward push during drifts.

---

## 5. Input Configuration

Inputs are handled via Enhanced Input directly on the `ModularVehicleCore`.

1. Select `ModularVehicleCore`.
2. Go to **Input Settings**.
3. Assign your **Input Mapping Context**.
4. Bind actions:
   - **Driving:** `ThrottleAction`, `SteeringAction`, `HandbrakeAction`
   - **Engine:** `EngineStartAction`
   - **Transmission:** `ClutchAction`, `ShiftUpAction`, `ShiftDownAction`
   - **Flight:** `ToggleFlightModeAction`, `VerticalThrustAction`, `FlightPitchAction`, `FlightRollAction`, `FlightYawAction`

---

## 6. AI System

The plugin includes a complete AI stack with Behavior Trees, Tasks, and Services for both ground and air vehicles.

### Basic Setup

1. Add `ModularVehicleAIComponent` to your Vehicle Pawn.
2. The component automatically spawns the `ModularVehicleAIController`.
3. Assign a Behavior Tree in the component settings (a default is included).

### Moving a Vehicle (Blueprint)

Right-click in your GameMode or Level Blueprint and search for **"Modular Vehicle Move To"**:

- **Target Location:** Where to go.
- **Prioritize Driving:** `True` = drive on ground, `False` = fly (if rotors available).
- **Flight Config:** Customize cruising altitude (`CruisingAltitude`, default `3000 cm`), approach radius, and `bLandOnArrival`.
- **Outputs:** Use `On Arrived` or `On Failed` pins.

### AI Behavior Tree Services

**BTService_SplineRoadPlanner** — ZoneGraph road following with intelligent pathfinding.

- Vehicles follow ZoneGraph lanes with tag-based filtering.
- Calculates safe corner speeds from road curvature using `MaxLateralAccelG` (default `0.4g`).
- Physics-based braking distances via `MaxBrakingDecelCMSS` (default `230 cm/s²`).
- Junction approach speed reduction (`JunctionApproachDistanceCM` default `3000 cm`).
- `bWanderMode` — Perpetual road following without a destination.
- Falls back to NavMesh when off the road network.
- Debug: `bDrawDebugLane`, `bDrawDebugPath`.

**BTService_TrafficFollowing** — Realistic traffic behavior with safe following.

- **IDM (Intelligent Driver Model):** Maintains safe following distances.
  - `MinJamGapCM` — Minimum bumper gap (default `350 cm`).
  - `TimeHeadwaySec` — Desired time headway (default `1.5s`).
  - `AccelCMS2` / `DecelCMS2` — Acceleration/braking comfort (default `400`/`700 cm/s²`).
- **Give-Way:** Cross-traffic detection at intersections (`JunctionCheckRadiusCM` default `650 cm`).
- **Speed Noise:** Organic speed variation (`SpeedNoiseKPH` default `±6 km/h`).
- **Leader Detection:** Corridor-based forward sweep that handles curved roads.
  - `LookAheadCM` — Detection distance (default `3000 cm`).
  - `CorridorHalfWidthCM` — Lane corridor width (default `220 cm`).
- Debug: `bDrawDebug`.

**BTService_AirPathPlanner** — Helicopter obstacle avoidance.

- 7-direction sphere sweep detection.
- Go-over/go-around decisions based on obstacle height.
- Pawn separation to prevent clustering.

### AI Behavior Tree Tasks

| Task | Purpose |
|------|---------|
| `BTTask_ModularGroundDrive` | Steering & throttle with yaw-error tracking and speed-adaptive gain |
| `BTTask_ModularFly` | Horizontal flight control with altitude maintenance |
| `BTTask_ModularTakeoff` | Spool-up phase followed by gradual ascent |
| `BTTask_ModularLand` | Descent with approach speed limiting and touchdown detection |
| `BTTask_SetRandomTrafficTarget` | Randomize next waypoint for traffic cycling |

---

## 7. Traffic System (Mass Entity Framework)

The traffic system spawns and manages hundreds of lightweight AI vehicles using Unreal's Mass Entity Framework. These are rendered with Instanced Static Meshes for maximum performance.

### Setup

1. Place an `AModularTrafficManager` in your level.
   - Set `AllowedLaneTags` to match your ZoneGraph lane tags.
   - Set `GlobalSpeedLimitKPH` (default `80`).
   - Optionally set `LanePriorities` for right-of-way at intersections.

2. Place an `AModularTrafficSpawner` in your level.
   - Assign `TrafficVehicleClasses` — one or more vehicle pawn blueprints.
   - Assign `TrafficBehaviorTree` (included default: `BT_Vehicles`).
   - Set `SpawnCount` — how many vehicles to maintain (default `30`).

3. Ensure your level has **ZoneGraph** baked from `AZoneShape` actors with lane tags matching `AllowedLaneTags`.

### Spawner Configuration

**Spawn Settings:**

| Property | Default | Description |
|----------|---------|-------------|
| `SpawnCount` | 30 | Target number of active vehicles |
| `MinSpacingCM` | 900 | Minimum distance between spawn points on same lane |
| `MinRoadLength` | 3000 | Minimum lane length to allow spawning |
| `SpawnHeightOffsetCM` | 80 | Height above lane surface to spawn |

**Optimization (distance-based spawning/despawning):**

| Property | Default | Description |
|----------|---------|-------------|
| `bOptimizeSpawning` | true | Enable dynamic spawn/despawn |
| `MaxDespawnDistance` | 20000 | Distance to despawn far vehicles |
| `MaxSpawnDistance` | 15000 | Max distance from player to spawn |
| `MinSpawnDistance` | 5000 | Min distance from player to spawn |
| `OptimizationUpdateInterval` | 1.0s | How often to run spawn/despawn logic |
| `MaxGridlockTime` | 25s | Time before stuck vehicles are removed |

**Density Balancing (prevents overcongestion):**

| Property | Default | Description |
|----------|---------|-------------|
| `NumDensitySectors` | 8 | Angular sectors around player for distribution |
| `MaxSectorFraction` | 0.4 | Max fraction of vehicles per sector |
| `LocalDensityRadius` | 4000 | Radius for local crowding check |
| `MaxLocalDensityCount` | 4 | Max vehicles within density radius |
| `bAdaptiveCongestionDespawn` | true | Aggressively thin non-visible congested areas |
| `MinCongestionDespawnDistance` | 8000 | Min distance from player for congestion despawn |

### Road Network

**ModularRoadSpline** — Place along roads to define speed limits:

- `SpeedLimitKPH` — Speed limit for this segment (default `60`).
- `LaneTagName` — Associated ZoneGraph lane tag.
- `RoadInfluenceRadius` — Snap distance for vehicles (default `3000 cm`).

### Mass Traffic Processors

The ECS runs these processors each frame:

| Processor | Frequency | Purpose |
|-----------|-----------|---------|
| LaneFollowing | Every frame | Advance vehicles along ZoneGraph lanes |
| ObstacleAvoidance | Every frame | IDM-based speed control with leader gap tracking |
| Overtake | Every frame | Lateral overtake blend |
| Movement | Every frame | Write final world position to ISM instances |
| Signal | ~5 Hz | Query traffic signal state |
| GiveWay | ~5 Hz | Junction conflict detection |

### Global Settings

In `Project Settings > Plugins > Mass Traffic`:

- `MaxTrafficEntities` — Global vehicle cap (default `500`).
- `MaxDeltaTimeSec` — Delta-time clamp to prevent physics explosions (default `0.1s`).
- `bDrawDebugTraffic` — Visualize velocity arrows and lanes.

---

## 8. Dynamic Mud Pit

Place an `AModularMudPit` actor in your level for deformable terrain.

**Key Settings:**

| Property | Default | Description |
|----------|---------|-------------|
| `PitSize` | 2000×2000 | Dimensions of the mud area (cm) |
| `MaxSinkDepth` | 35 | Maximum sink depth (cm) |
| `SurfaceNoiseMagnitude` | 15 | Amplitude of surface noise for natural look |
| `NoiseFrequency` | 0.05 | Frequency of surface noise |
| `RenderTargetResolution` | 1024 | Visual deformation texture resolution |
| `PhysicsGridResolution` | 128 | CPU physics grid resolution (keep low) |

Vehicles driving through the pit will physically sink based on mud depth and leave persistent wheel tracks.

---

## 9. Flight System

### Flight Model (ModularVehicleCore)

| Property | Default | Description |
|----------|---------|-------------|
| `MaxFlightAngle` | 35° | Maximum pitch/roll angle |
| `FlightControlResponseSpeed` | 5.0 | Input responsiveness (1–20) |
| `AttitudeStrength` | 4.0 | Hover attitude hold strength |
| `AttitudeDamping` | 2.0 | Hover attitude damping |
| `YawSpeed` | 50.0 | Yaw rotation rate |
| `HoverPositionStiffness` | 1500 | Altitude hold stiffness |
| `FlightLateralDrag` | 2.0 | Sideways resistance |
| `bEnableArcadeTiltAssist` | true | Auto-tilt for arcade feel |

### AI Flight Sequences

The AI uses a state machine for flight:

1. **Takeoff** (`BTTask_ModularTakeoff`) — Spool-up rotors, then gradually ascend to `CruisingAltitude`.
2. **Cruise** (`BTTask_ModularFly`) — Fly toward target with obstacle avoidance and pawn separation.
3. **Land** (`BTTask_ModularLand`) — Descend at `ApproachSpeedLimitKPH`, detect touchdown.

Configure via `FModularFlightConfig`:

- `CruisingAltitude` — Flight height (default `3000 cm`).
- `bLandOnArrival` — Auto-land at destination (default `true`).
- `ApproachRadius` — Distance to begin descent (default `5000 cm`).

---

## 10. Included Content

### Vehicle Blueprints (14)

**Ground:** Ambulance · City Bus · Drift Car · Firetruck · Flatbed · Garbage Truck · Police Car · Taxi · Tuk-Tuk · Motorbike

**Air:** Helicopter · Mini Helicopter · Hybrid Drone (VTOL)

### City Environment

- Full city with buildings and urban environment.
- Complete road network with pre-baked ZoneGraph lanes.
- Pre-configured traffic spawner, traffic manager, and AI vehicle fleet.

### AI Assets

- `BT_Vehicles` — Pre-built Behavior Tree for traffic.
- `BB_Vehicles` — Shared Blackboard asset.
- `E_MovementMode` — Ground/Air movement enum.

---

## 11. Troubleshooting

**Vehicle won't move:**
- Is the Engine on? (Map the `EngineStartAction` input).
- Do you have `bIsDriveWheel` checked on at least one wheel?
- Is the Handbrake engaged?

**Vehicle flips immediately:**
- Check your collision setup on the chassis mesh.
- Lower the `CenterOfMassOffset` Z value (e.g., `-60`).
- Enable `bAutoCalibrateSuspension`.

**AI vehicle doesn't follow roads:**
- Ensure ZoneGraph is baked in the level (`AZoneShape` actors).
- Verify `AllowedLaneTags` on `AModularTrafficManager` match your ZoneGraph lane tags.
- Check that `BTService_SplineRoadPlanner` is attached to the Behavior Tree.
- Enable `bDrawDebugLane` on the SplineRoadPlanner to visualize lane following.

**Traffic piles up in one area:**
- Enable density balancing on the spawner (`NumDensitySectors`, `MaxSectorFraction`).
- Lower `MaxLocalDensityCount` to reject crowded spawn candidates.
- Enable `bAdaptiveCongestionDespawn` to thin congested non-visible areas.

**AI creates path but doesn't drive:**
- Ensure a `NavMeshBoundsVolume` is present (fallback pathfinding).
- Check that `DesiredSpeed` in the Blackboard is being set (debug via Behavior Tree view).

**Bike falls over:**
- Ensure `bIsBikeConfiguration` is checked on the Core.
- Increase `BikeUprightStiffness` and `BikeLowSpeedStiffness`.

**Damage not triggering:**
- Ensure `ModularDamageComponent` is attached.
- Check `MinImpactForceThreshold` — lower it if impacts aren't strong enough.
- Verify `Simulate Physics` is enabled on the chassis.

**Vehicles not sinking in mud:**
- Ensure `AModularMudPit` is placed and scaled correctly.
- Check that `MaxSinkDepth` is greater than 0.
- Verify vehicles have `ModularWheelComponent` attached.
