# OptimizedVehicleSystem — Documentation & Quick Start

## Overview

The **OptimizedVehicleSystem** is a high-performance, modular vehicle plugin for Unreal Engine 5. It allows you to create complex vehicles (Cars, Trucks, Bikes, Helicopters, and Hybrids) using **Static Meshes only** — no skeletal mesh rigging required.

**What's Included:**
- **Modular Vehicle Physics** — Cars, trucks, bikes, helicopters, and hybrid VTOL vehicles.
- **Complete AI Traffic System** — Hundreds of AI vehicles driving roads, obeying traffic rules, and following each other at safe distances using the Mass Entity Framework.
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
- **Transmission:** Supports both **Automatic** and **Manual** modes. In manual mode, the player shifts gears using `ShiftUpAction` / `ShiftDownAction` and controls the `ClutchAction` for realistic clutch engagement. In automatic mode, gear changes happen based on RPM thresholds. Both modes use configurable Gear Ratios and Differential Ratio.
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

### Quick Setup (Using Default Tags)

1. Place an `AModularTrafficManager` in your level.
   - Set `AllowedLaneTags` to match your ZoneGraph lane tags.
   - Set `GlobalSpeedLimitKPH` (default `80`).
   - Optionally set `LanePriorities` for right-of-way at intersections.

2. Place an `AModularTrafficSpawner` in your level.
   - Assign `TrafficVehicleClasses` — one or more vehicle pawn blueprints.
   - Assign `TrafficBehaviorTree` (included default: `BT_Vehicles`).
   - Set `SpawnCount` — how many vehicles to maintain (default `30`).

3. Ensure your level has **ZoneGraph** baked from `AZoneShape` actors with lane tags matching `AllowedLaneTags`.

### Custom Lane Profiles & Tags

The included city level ships with pre-configured lane tags. If you want to use your own road network with different lane tags (e.g., for a highway system, a European-style road, or a custom map), you need to set up the tag pipeline from ZoneGraph all the way through to the plugin actors. Every actor in the traffic system references lane tags by **FName** — they must all match exactly.

Here is the full process, step by step:

#### Step 1: Define Your Tags in ZoneGraph Project Settings

1. Go to **Project Settings → Plugins → ZoneGraph → Zone Graph Tag List**.
2. Add your custom tag names. For example, a 2-lane road might use:
   - `Highway_Left`
   - `Highway_Right`
   - `Residential_Left`
   - `Residential_Right`

   A single-lane one-way road might just use:
   - `OneWay_Main`

   These names are entirely up to you — the plugin matches on **FName** strings, not hardcoded values.

#### Step 2: Assign Tags to AZoneShape Actors

1. Place `AZoneShape` actors in your level to define your road lanes.
2. On each ZoneShape, assign the appropriate tag from the dropdown (which now shows your custom tags from Step 1).
3. Each ZoneShape lane should have exactly **one** traffic-relevant tag. For a 2-lane road, you typically place two parallel ZoneShapes — one tagged `Highway_Left`, the other `Highway_Right`.
4. **Build ZoneGraph** in the editor toolbar to bake the lane data.

#### Step 3: Configure AModularTrafficManager

Place one `AModularTrafficManager` in the level and set:

- **`AllowedLaneTags`** — Add **every** tag that vehicles are allowed to drive on. This is the master list. For example:
  ```
  AllowedLaneTags:
    [0] Highway_Left
    [1] Highway_Right
    [2] Residential_Left
    [3] Residential_Right
    [4] OneWay_Main
  ```
  Any tag **not** in this list is invisible to the traffic system (pedestrian paths, bike lanes, etc. are automatically excluded).

- **`LanePriorities`** (optional) — Set per-tag right-of-way priority at intersections. Higher number = higher priority. For example, give highways priority over residential roads:
  ```
  LanePriorities:
    Highway_Left     → 2
    Highway_Right    → 2
    Residential_Left → 1
    Residential_Right → 1
  ```

#### Step 4: Configure AModularTrafficSpawner

Place one `AModularTrafficSpawner` in the level:

- **`AllowedLaneTags`** (on the Spawner) — **Leave this empty** to automatically inherit all tags from `AModularTrafficManager`. Only fill it in if you want this specific spawner to spawn on a subset of tags (e.g., only highway lanes).

  If you fill it in, only lanes matching these tags will receive spawn candidates. For example, to create a spawner that only populates highways:
  ```
  AllowedLaneTags:
    [0] Highway_Left
    [1] Highway_Right
  ```
  You can place **multiple spawners** in a level — each targeting different lane tags with different vehicle classes (e.g., one spawner for trucks on highways, another for taxis on residential roads).

#### Step 5: Configure AModularRoadSpline (Per-Road Speed Limits)

For each road segment where you want a specific speed limit:

1. Place an `AModularRoadSpline` actor alongside the road.
2. Set **`LaneTagName`** to the tag this speed limit applies to (e.g., `Highway_Left`).
3. Set **`SpeedLimitKPH`** (e.g., `120` for highways, `50` for residential).

If no `AModularRoadSpline` matches a given tag, vehicles fall back to the `GlobalSpeedLimitKPH` on `AModularTrafficManager` (default `80`).

#### Step 6: BTService_SplineRoadPlanner (Automatic)

The road planner service **automatically** reads `AllowedLaneTags` from `AModularTrafficManager` at runtime. You do not need to configure lane tags on the Behavior Tree service — it picks them up from the Traffic Manager.

The planner uses `FindBestLaneForVehicle()` which iterates all allowed tags, finds the nearest lane for each, and selects the one pointing most toward the vehicle's destination. This means vehicles naturally pick the correct side of the road.

#### Summary: Where Each Tag Goes

| Actor | Property | What to Set |
|-------|----------|-------------|
| **ZoneGraph Settings** | Tag List | Define all your custom tag names |
| **AZoneShape** | Lane Tag | One tag per lane shape |
| **AModularTrafficManager** | `AllowedLaneTags` | Every driveable tag (master list) |
| **AModularTrafficManager** | `LanePriorities` | Optional: per-tag junction priority |
| **AModularTrafficSpawner** | `AllowedLaneTags` | Empty = inherit from manager, or subset |
| **AModularRoadSpline** | `LaneTagName` | One tag per road segment (for speed limit) |
| **BTService_SplineRoadPlanner** | *(automatic)* | Reads from TrafficManager at runtime |

#### Example: Custom 3-Road-Type Setup

Suppose you want highways, city streets, and alleys:

**ZoneGraph Tags:**
`Highway_L`, `Highway_R`, `City_L`, `City_R`, `Alley`

**TrafficManager:**
```
AllowedLaneTags: [Highway_L, Highway_R, City_L, City_R, Alley]
GlobalSpeedLimitKPH: 60
LanePriorities: { Highway_L: 3, Highway_R: 3, City_L: 2, City_R: 2, Alley: 1 }
```

**RoadSplines:**
- Highway segments → `LaneTagName: Highway_L`, `SpeedLimitKPH: 120`
- Highway segments → `LaneTagName: Highway_R`, `SpeedLimitKPH: 120`
- City streets → `LaneTagName: City_L`, `SpeedLimitKPH: 50`
- City streets → `LaneTagName: City_R`, `SpeedLimitKPH: 50`
- Alleys → `LaneTagName: Alley`, `SpeedLimitKPH: 20`

**Spawners:**
- Spawner 1 (trucks): `TrafficVehicleClasses: [BP_Flatbed, BP_GarbageTruck]`, `AllowedLaneTags: [Highway_L, Highway_R]`
- Spawner 2 (city cars): `TrafficVehicleClasses: [BP_Taxi, BP_PoliceCar]`, `AllowedLaneTags: []` *(empty = all tags)*

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
| Movement | Every frame | Write final world position to ISM instances |
| GiveWay | ~5 Hz | Junction conflict detection |

### Global Settings

In `Project Settings > Plugins > Mass Traffic`:

- `MaxTrafficEntities` — Global vehicle cap (default `500`).
- `MaxDeltaTimeSec` — Delta-time clamp to prevent physics explosions (default `0.1s`).
- `bDrawDebugTraffic` — Visualize velocity arrows and lanes.

---

## 8. Flight System

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

## 9. Included Content

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

## 10. Troubleshooting

**Vehicle won't move:**
- Is the Engine on? (Map the `EngineStartAction` input).
- Do you have `bIsDriveWheel` checked on at least one wheel?
- Is the Handbrake engaged?

**Vehicle flips immediately:**
- Check your collision setup on the chassis mesh.
- Lower the `CenterOfMassOffset` Z value (e.g., `-60`).
- Enable `bAutoCalibrateSuspension`.

**AI vehicle doesn't follow roads:**
- Ensure ZoneGraph is baked in the level (`AZoneShape` actors). Rebuild via the editor toolbar if needed.
- Verify `AllowedLaneTags` on `AModularTrafficManager` match your ZoneGraph lane tags **exactly** (case-sensitive FName match).
- Check that `BTService_SplineRoadPlanner` is attached to the Behavior Tree.
- Enable `bDrawDebugLane` on the SplineRoadPlanner to visualize lane following.

**Custom lane tags not working:**
- Verify the tag exists in **Project Settings → Plugins → ZoneGraph → Zone Graph Tag List**.
- Verify the tag is assigned to at least one `AZoneShape` lane in your level.
- Verify the **exact same FName** appears in `AModularTrafficManager::AllowedLaneTags`.
- If using `AModularRoadSpline`, verify `LaneTagName` matches.
- Rebuild ZoneGraph after any tag changes.
- The spawner's `AllowedLaneTags` can be left **empty** to inherit from the Traffic Manager — only fill it if you want a subset.

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

---

## Features Under Development

The following features are included in the plugin source but are **still under active development**. They are not yet stable and **should not be used in a production project** at this time. They will be documented fully once they are ready:

- **Vehicle Damage System** (`ModularDamageComponent`) — Real-time mesh deformation, damage zones, and part detachment.
- **Dynamic Mud Pit** (`ModularMudPit`) — Deformable terrain with wheel sinking and persistent tracks.
- **Traffic Overtaking** (`MassTrafficOvertakeProcessor`) — Lateral lane-change overtake maneuvers for mass traffic entities.
- **Traffic Signals** (`ATrafficLightIntersection`) — Signalized intersection control with multi-phase signal cycles.

These systems are functional in isolation but may have edge cases, missing polish, or API changes in upcoming updates. Use at your own risk if experimenting, but do not ship with them enabled.
