# Optimized Vehicle System

Optimized Vehicle System is a UE 5.7 plugin for building and using drivable ground vehicles, helicopters, hybrid rotor vehicles, and fixed-wing airplanes in one package. It ships with ready-made example vehicles, a working AI stack, input assets, traffic tools, runway tools, and a public demo level you can use as both a starting point and a reference scene. All included vehicles can be used as player-controlled vehicles or AI-controlled vehicles.

This handbook is written for plugin users. It focuses on setup, asset usage, editor workflow, and what each important setting does in plain language.

## What the plugin includes

### Included systems

- Ground vehicle driving
- Helicopter and hybrid rotor flight
- Fixed-wing airplane flight
- Player-controlled and AI-controlled vehicle setups
- Damage-ready vehicle parts
- Vehicle AI with blackboard and behavior tree support
- Road traffic setup tools
- Runway and airport-style setup tools
- Mass traffic support for larger crowds of vehicles
- Ready-made content you can use immediately

### Included content folders

| Folder | What it contains |
| --- | --- |
| `Content/ModularVehicles` | Ready-to-use vehicle blueprints for cars, trucks, buses, bikes, helicopters, and airplanes |
| `Content/AI` | The shipped AI controller, blackboard, behavior tree, and movement enum |
| `Content/Inputs` | The shared vehicle input mapping context and the input actions used by the included vehicles |
| `Content/Blueprints` | World setup helpers such as runway, traffic spawner, helipad, and demo support blueprints |
| `Content/Levels` | The public demo level and additional internal/test level content |
| `Content/Widgets` | The controls widget used by the packaged example setup |

### Included ready-made vehicles

#### Ground vehicles

- `BP_Ambulance`
- `BP_CityBus`
- `BP_DriftCar`
- `BP_Firetruck`
- `BP_Flatbed`
- `BP_GarbageTruck`
- `BP_ModularMotorbike`
- `BP_PoliceCar`
- `BP_Taxi`
- `BP_TukTuk`
- `BP_Van`

#### Helicopters and hybrid rotor vehicles

- `BP_Heli`
- `BP_MiniHeli`
- `BP_ModularHybridRotor`

#### Fixed-wing airplanes

- `BP_Airplane`
- `BP_Airplane2`
- `BP_BushPlane`
- `BP_C130`
- `BP_Cirrus`

### Included setup assets

| Asset | Purpose |
| --- | --- |
| `L_VehiclesDemo` | The public demo level for the plugin |
| `BB_Vehicles` | The shipped vehicle AI blackboard |
| `BT_Vehicles` | The shipped vehicle AI behavior tree |
| `BP_VehiclesAiController` | The included AI controller blueprint |
| `BP_Runway` | The ready-to-place runway actor blueprint |
| `BP_Helipad` | A simple helipad helper blueprint |
| `BP_TrafficSpawner` | The ready-to-place traffic spawner blueprint |
| `BP_RoadIdentifier` | A helper blueprint used in road and traffic scenes |
| `BP_VehiclesGM` | The example game mode asset used by the included setup |
| `IMC_VehicleControls` | The shared vehicle input mapping context |
| `WBP_Controls` | The included controls display widget |

> `L_CityTests` exists in the plugin content, but it should be treated as internal or test content rather than the public demo level. The documented demo level for users is `L_VehiclesDemo`.

## Requirements and supported version

| Item | Requirement |
| --- | --- |
| Unreal Engine version | `5.7.0` |
| Plugin type | Runtime plugin with content |
| Supported module platforms | `Win64`, `Mac`, `Linux`, `iOS`, `Android` |
| Best starting point | A project with physics enabled and room to place plugin vehicles and test maps |

## Installation and plugin enablement

1. Place the plugin in your project's `Plugins` folder if it is not already there.
2. Open the Unreal Editor for your project.
3. Open **Edit > Plugins**.
4. Search for **Optimized Vehicle System**.
5. Enable the plugin if it is disabled.
6. Restart the editor when prompted.
7. After restart, verify that the plugin content appears once **Show Plugin Content** is enabled in the Content Browser.

## Required Unreal plugins and project setup

The plugin declares several built-in Unreal dependencies. In most cases they are enabled automatically when the plugin is enabled, but if part of the system is missing in your project, verify these first.

| Built-in plugin | Why it matters to users |
| --- | --- |
| `EnhancedInput` | Required for the included `IMC_VehicleControls` input setup |
| `GeometryScripting` | Used by geometry-related content and tools |
| `ZoneGraph` | Needed for lane-based traffic routing |
| `MassGameplay` | Needed for Mass traffic features |
| `ProceduralMeshComponent` | Used by deformation and damage workflows |
| `Niagara` | Used for damage and failure effects |
| `GeometryCollectionPlugin` | Required when using fracture-related content |
| `Fracture` | Required for fracture workflows used by supported content setups |

### Recommended project checks

- Make sure physics simulation is available in your project.
- Make sure plugin content is visible in the Content Browser.
- Make sure you restart the editor after first enabling the plugin.
- If you intend to use traffic, make sure your road setup includes usable lane data and consistent lane tags.
- If you intend to use the Mass traffic path, make sure ZoneGraph and Mass systems are enabled before you start building your scene.

### Plug-and-play ZoneGraph lane profile setup

To make the traffic side of the plugin easier to use, the plugin automatically creates its default ZoneGraph road setup in the editor if it is missing.

On startup, the plugin checks ZoneGraph settings and auto-adds the shared road tag plus the default road lane profiles it expects:

- `Main roads`
- `Sub roads`
- `Single lane`

It also auto-registers the supporting road-identification tags used by those profiles and adds the default routing rule the plugin needs for the main two-way road profile. In practice, this means a fresh project does not need you to manually build those default lane profiles before you start using the traffic systems, which makes the plugin much closer to plug and play.

If you already have your own ZoneGraph setup, the plugin only fills in what is missing. You can still customize or extend the generated profiles later to match your own road network.

## How to show plugin content in the Content Browser

1. Open the Content Browser.
2. Click the **Settings** menu in the browser.
3. Enable **Show Plugin Content**.
4. Find the plugin content root for **OptimizedVehicleSystem**.
5. Browse the folders listed earlier: `ModularVehicles`, `AI`, `Inputs`, `Blueprints`, `Levels`, and `Widgets`.

If you do not enable plugin content visibility, the ready-made vehicles and setup assets will look like they are missing even though the plugin is installed correctly.

## Quick start with the included assets

### Fastest way to evaluate the plugin

1. Enable the plugin.
2. Enable **Show Plugin Content**.
3. Open `L_VehiclesDemo`.
4. Explore the included example setup.
5. Duplicate one of the included vehicle blueprints into your own project folder before you begin heavy customization.

### Quick content map

| Goal | Best asset to start with |
| --- | --- |
| Try a standard road vehicle | `BP_Taxi`, `BP_PoliceCar`, `BP_Van` |
| Try a larger heavy vehicle | `BP_CityBus`, `BP_Firetruck`, `BP_GarbageTruck`, `BP_Flatbed` |
| Try a bike setup | `BP_ModularMotorbike` |
| Try helicopter-style flight | `BP_Heli`, `BP_MiniHeli` |
| Try hybrid rotor flight | `BP_ModularHybridRotor` |
| Try fixed-wing flight | `BP_Airplane`, `BP_Airplane2`, `BP_BushPlane`, `BP_C130`, `BP_Cirrus` |
| Try the AI stack | `BP_VehiclesAiController`, `BB_Vehicles`, `BT_Vehicles` |
| Try traffic spawning | `BP_TrafficSpawner` |
| Try runway setup | `BP_Runway` |

### Included input assets

| Asset | Intended use |
| --- | --- |
| `IMC_VehicleControls` | Main shared input mapping context for vehicles |
| `IA_Throttle` | Ground throttle input |
| `IA_Steering` | Ground steering input |
| `IA_HandBrake` | Handbrake input |
| `IA_EngineStartAction` | Engine on or off input |
| `IA_ShowControls` | Toggle controls display |
| `IA_ToggleFlightMode` | Toggle flight mode for supported vehicles |
| `IA_VerticalThrust` | Vertical lift input for rotorcraft |
| `IA_HeliPitch` | Helicopter pitch input |
| `IA_HeliRoll` | Helicopter roll input |
| `IA_HeliYaw` | Helicopter yaw input |
| `IA_AirplaneThrottle` | Fixed-wing throttle input |
| `IA_AirplaneBrake` | Fixed-wing brake input |
| `IA_AirplanePitch` | Fixed-wing pitch input |
| `IA_AirplaneRoll` | Fixed-wing roll input |
| `IA_AirplaneYaw` | Fixed-wing yaw or rudder input |
| `IA_LandingGear` | Landing gear toggle for airplanes with retractable gear |

## Working with the demo level

`L_VehiclesDemo` is the public demo level for the plugin. It is the fastest place to understand how the plugin's systems fit together.

### What to look for in `L_VehiclesDemo`

- Ready-made drivable vehicles
- Helicopter and fixed-wing examples
- A simple city-style environment
- Traffic setup
- Runway and airplane setup
- An example of how the shipped AI and input assets are used together

### Recommended way to study the demo level

1. Open `L_VehiclesDemo`.
2. Identify which pawn is intended for direct player control.
3. Note which world setup actors are already present, especially traffic and runway helpers.
4. Inspect the Details panels of the placed actors before changing anything.
5. Duplicate a working vehicle blueprint and experiment in your own test map instead of editing the demo copy directly.

### What the demo level is best used for

- Learning the recommended setup order
- Seeing how the included content is intended to be combined
- Checking sensible default values before custom tuning
- Comparing your custom scene against a known working example

## Core usage workflows

## Using an included ground vehicle

1. Open the `ModularVehicles` folder.
2. Drag a ground vehicle blueprint into your level.
3. Make sure the level has a player start or another way to possess the vehicle.
4. Verify the vehicle has the shared input mapping assigned through its vehicle core setup.
5. Press Play and confirm throttle, steering, braking, and engine controls respond.

### Recommended first choices

- `BP_Taxi` for a simple car
- `BP_Van` for a stable everyday vehicle
- `BP_DriftCar` if you want to tune drift behavior
- `BP_CityBus` or `BP_Firetruck` if you want to tune larger mass and turning behavior

Every included ground vehicle can be used as a player vehicle or assigned to the shipped AI setup.

## Using an included helicopter or hybrid rotor vehicle

1. Drag `BP_Heli`, `BP_MiniHeli`, or `BP_ModularHybridRotor` into your level.
2. Confirm that the vehicle is possessed by the player or a controller that can feed input to it.
3. Make sure the vehicle is using the shared input mapping context.
4. Use vertical thrust plus helicopter pitch, roll, and yaw inputs to fly.
5. If the vehicle supports flight mode switching, use the flight-mode action to move between ground-style and flight-style control where appropriate.

### Good setup habits

- Give rotorcraft enough vertical clearance for first tests.
- Tune lift and spool-up before tuning aggressive control behavior.
- If the craft feels twitchy, reduce control sensitivity before changing the basic lift setup.

Like the ground vehicles, the included rotorcraft can be used for direct player control or as AI-controlled vehicles.

## Using an included fixed-wing airplane

1. Drag a fixed-wing aircraft such as `BP_Cirrus`, `BP_BushPlane`, or `BP_C130` into your level.
2. Place it on a runway or on a long straight surface for first tests.
3. Confirm the input mapping context is active.
4. Use airplane throttle, brake, pitch, roll, and yaw inputs.
5. If the aircraft has retractable landing gear, use the landing-gear action after takeoff and before landing.

### Good setup habits

- Test takeoff on a long straight runway first.
- Tune takeoff speed, control sensitivity, and stability before adjusting AI flight behavior.
- If the airplane struggles to rotate, start with the `TakeoffSpeedKPH` and propeller or thrust settings before changing path-planning values.

The included fixed-wing aircraft can also be used either as player-controlled aircraft or AI-controlled aircraft.

## Hooking up player controls

The plugin ships with `IMC_VehicleControls` plus all of the input actions needed by the included vehicles. In most cases the fastest path is to duplicate one of the included vehicles and keep its input asset assignments.

### Recommended control workflow

1. Start from a working included vehicle.
2. Check the vehicle's `ModularVehicleCore` input settings.
3. Keep `IMC_VehicleControls` assigned unless you are intentionally building your own mapping setup.
4. Rebind keys in the input assets if your project needs a different control layout.
5. Keep the action references aligned with the correct vehicle type.

### Common input mistakes

- Ground vehicles responding to airplane inputs because the wrong input action reference is assigned
- A vehicle not responding at all because no mapping context is being added
- Airplane landing gear not toggling because the landing-gear action was left unassigned
- Flight mode controls not working because the flight toggle action was not assigned

## Using the shipped AI setup

The included AI stack centers around:

- `BP_VehiclesAiController`
- `BB_Vehicles`
- `BT_Vehicles`
- `ModularVehicleAIComponent`

The same vehicle blueprints you use for player driving or flying can also be switched into AI use, which means you do not need separate vehicle libraries for player vehicles and AI traffic or aircraft.

### Recommended AI workflow

1. Start from an included vehicle that is already close to the behavior you want.
2. Make sure the pawn has a `ModularVehicleAIComponent`.
3. Assign `BP_VehiclesAiController` as the AI controller class if your blueprint does not already use it.
4. Set the AI component's default behavior tree to `BT_Vehicles` if it is not already set.
5. Keep the blackboard keys used by the behavior tree consistent with the shipped setup.
6. For aircraft, place runways in the scene before testing AI takeoff and landing behavior.
7. For road traffic, make sure your lane tags, allowed tags, and traffic-light phase tags all match.

### Important blackboard reminder

If you rename or replace blackboard keys used by the included behavior tree services and tasks, you must update those node assignments as well. The shipped AI setup expects the same blackboard layout used by `BB_Vehicles`.

## Setting up roads, traffic, and runways

### Road and traffic setup

At a high level, the traffic system needs:

- Driveable roads or lane data
- Consistent lane tags
- A traffic manager
- One or more spawners
- Optional traffic lights

### Runway and aircraft setup

At a high level, the aircraft AI system needs:

- A placed runway actor such as `BP_Runway`
- Correct runway orientation
- Enough clear takeoff and landing space
- An aircraft with the airplane and AI components configured

### Lane-tag consistency matters

Keep these tag-driven systems aligned:

- `ModularRoadSpline` lane tags
- `ModularTrafficManager` allowed lane tags
- `ModularTrafficSpawner` allowed lane tags
- `TrafficLightIntersection` phase lane tags
- AI denied-lane lists and traffic roaming rules

If those names do not match, traffic will appear broken even when the scene looks correct.

## Building your own vehicle and traffic or airport setups

## Recommended customization path

The fastest and safest path is usually:

1. Duplicate the closest included blueprint.
2. Rename it into your own project folder.
3. Replace visuals and meshes.
4. Tune only the relevant groups of settings.

Build from scratch only when you truly need a new layout.

## Building your own ground vehicle

### Minimum recommended setup

- A pawn or vehicle blueprint with a valid physics root
- `ModularVehicleCore`
- At least one `ModularEngineComponent`
- At least two `ModularWheelComponent` instances
- Steering wheels marked correctly if the vehicle should steer
- Drive wheels marked correctly if the vehicle should move under power

### Recommended build order

1. Create the root vehicle blueprint.
2. Add the visual or physics body.
3. Add `ModularVehicleCore`.
4. Add wheels in the correct positions.
5. Mark which wheels steer, drive, and handbrake.
6. Add an engine component.
7. Assign input assets.
8. Test straight-line movement first.
9. Tune braking, suspension, then steering.

## Building your own helicopter or hybrid rotor vehicle

### Minimum recommended setup

- A valid vehicle root and `ModularVehicleCore`
- One or more `ModularRotorComponent` instances
- A working input setup for vertical thrust, pitch, roll, and yaw

### Recommended build order

1. Start with the vehicle core.
2. Add rotors and align them correctly.
3. Test lift and spool-up first.
4. Tune pitch, roll, yaw, and stability after the craft can hold flight.
5. Add damage and effects last.

## Building your own fixed-wing airplane

### Minimum recommended setup

- A valid vehicle root and `ModularVehicleCore`
- At least one `ModularPropellerComponent` or supported propulsion setup
- One `ModularAirplaneComponent`
- Wheels or `ModularLandingGearComponent` instances for takeoff and landing
- Airplane input actions assigned

### Recommended build order

1. Add the core.
2. Add propulsion.
3. Add the airplane component.
4. Add landing gear or wheels.
5. Tune takeoff behavior.
6. Tune in-flight control sensitivity and stability.
7. Tune AI flight behavior and path-planning values after manual flight feels correct.

## Building your own traffic scene

### Minimum recommended setup

- Road or lane layout
- `ModularTrafficManager`
- One or more `ModularTrafficSpawner` instances
- Vehicles suitable for AI traffic
- Matching lane tags across the full traffic stack

### Recommended build order

1. Build the roads and lane network.
2. Decide your lane tag naming before placing traffic systems.
3. Place the traffic manager.
4. Fill in allowed lane tags and speed rules.
5. Place traffic spawners.
6. Add optional traffic lights at intersections.
7. Test a small spawn count first.
8. Increase density only after the route logic works reliably.

## Building your own runway or airport scene

### Minimum recommended setup

- One or more `BP_Runway` actors
- Clear runway direction and spacing
- Aircraft with `ModularVehicleAIComponent`
- Enough approach and rollout distance

### Recommended build order

1. Place the runway blueprint.
2. Rotate it so the approach and takeoff directions face open airspace.
3. Test manual takeoff and landing first.
4. After manual flight works, test AI move-to-runway behavior.
5. Add more runways only after the first one works reliably.

## Full settings reference

The reference below is grouped by system and is written around what you see in the Unreal Details panel.

## ModularVehicleCore

### Physics and handling

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `CoefficientOfDrag` | How much air resistance the vehicle body creates | The vehicle should lose speed faster at high speed | The vehicle feels too slow or struggles to coast | Strongest effect at higher speeds |
| `AirDensity` | Strength of the air used in drag calculations | You want stronger overall aerodynamic resistance | You want less aerodynamic slowdown | Usually left near default unless your tuning style needs it |
| `CrossSectionArea` | Approximate frontal area used for drag | The body should feel broader or more resistance-heavy | The body feels too draggy for its shape | Larger vehicles often need more than small cars |
| `CenterOfMassOffset` | Moves the vehicle's weight balance | You want more planted feel or better anti-roll behavior | The vehicle feels too nose-heavy, tail-heavy, or unstable after an offset change | One of the most important stability settings |
| `DriftTorque` | Extra rotational help while drifting | You want easier rotation in slides | The vehicle spins too eagerly | Mainly useful for drift-oriented builds |
| `ArcadeDriftAssist` | Automatic help that keeps a drift going | You want accessible arcade drifting | You want more demanding, natural drifting | Use with `bCanDrift` and counter-steer settings |
| `bCanDrift` | Enables drift-oriented behavior | The build is meant to slide | The build should stay grip-focused | Turn off for more disciplined road vehicles |
| `SwayBarStiffness` | How strongly left and right suspension movement is linked | The body rolls too much in corners | The vehicle feels harsh or too stiff over uneven surfaces | Higher values improve flat cornering but can reduce compliance |
| `AckermannGeometry` | Steering angle difference between inner and outer wheels | You want sharper, more natural front-wheel turning | The steering feels too aggressive or odd in tight corners | Helpful on cars with front steering axles |
| `bUseDynamicAngularDamping` | Applies extra rotation damping when needed | The vehicle spins or oscillates too easily | The vehicle feels overly damped or numb | Often helpful on arcade-focused vehicles |
| `DriftAngularDamping` | Damping applied during drift control | The drift needs more body control | The drift loses energy too quickly | Only matters when drift support is used |
| `AutomaticCounterSteer` | Auto-corrects steering during slides | You want easier drift recovery | You want full manual steering control | Higher values make drifting easier to manage |
| `MaxDriftAngle` | Maximum supported drift angle | You want bigger, more dramatic slides | The vehicle feels too loose or unrealistic | Higher values suit showy drift cars |
| `DriftPushForce` | Extra sideways push during drift behavior | You want stronger slide continuation | The rear steps out too violently | Best used sparingly |
| `ExternalResistanceForce` | Constant extra resistance fighting motion | The vehicle accelerates too easily | The vehicle feels sluggish everywhere | Acts like broad extra drag on the build |
| `bUseBrakeAsReverse` | Uses brake input to also reverse when appropriate | You want simple arcade-style reversing | You want separate reverse behavior | Useful for accessible ground vehicles |
| `BrakingIntensity` | Overall brake strength calibration | The vehicle takes too long to stop | The vehicle locks up or stops unrealistically fast | Affects automatic wheel tire calibration too |
| `MaxSpeedInReverse` | Reverse speed cap | You want stronger reversing ability | You want safer or more believable reverse speed | Helpful on service vehicles and heavy vehicles |
| `SteeringSensitivity` | How quickly steering input turns the vehicle | The steering feels lazy | The steering feels twitchy | Start small for fast vehicles |
| `bUseSensitivityClamp` | Adds extra steering limiting behavior | You want safer steering at speed | You want direct full-range steering feel | Useful for unstable setups |

### Suspension setup

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bAutoCalibrateSuspension` | Auto-fills suspension from vehicle weight targets | You want quicker setup with less manual tuning | You want full manual control | Recommended for first-time setup |
| `TargetSuspensionSag` | How much suspension travel is used at rest | You want the vehicle to sit lower into its suspension | You want a taller, firmer resting stance | Only used when auto calibration is on |
| `SuspensionDampingRatio` | How strongly the suspension resists bounce | The vehicle keeps bouncing after bumps | The suspension feels dead or over-damped | Useful for stabilizing larger vehicles |

### Bike physics

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bIsBikeConfiguration` | Switches the core into bike behavior | The build is a bike or bike-like vehicle | The build is not a bike | Enables the bike-only settings below |
| `BikeMaxLeanAngle` | Maximum allowed lean angle | You want deeper lean in turns | The bike tips too far and feels unstable | Core handling identity for bikes |
| `bInvertLeanDirection` | Reverses the lean response direction | Your bike leans the wrong way for the setup | The lean direction is already correct | Mostly a correction setting |
| `BikeUprightStiffness` | How strongly the bike tries to stand up | The bike falls over too easily | The bike feels too rigid and unnatural | Important for low-speed stability |
| `BikeLowSpeedStiffness` | Extra upright help at low speed | The bike is hard to control when moving slowly | Slow-speed movement feels artificially locked upright | Strongly affects takeoff and stop behavior |
| `BikeUprightDamping` | Dampens upright corrections | The bike wobbles while recovering | The bike feels too slow to react | Use with upright stiffness |
| `BikePitchStiffness` | How strongly front-back pitch is corrected | The bike dives or rears too much | The bike feels too flat and lifeless | Helps under braking and acceleration |
| `BikePitchDamping` | Dampens pitch correction movement | Pitch oscillation is too visible | Pitch response feels too dull | Balances pitch stiffness |
| `BikeMinSpeedForLean` | Speed needed before full lean behavior starts | You want leaning to begin later | You want leaning available earlier | Helpful for balancing city bikes vs sport bikes |
| `BikeDirectYawStrength` | Direct turning help for the bike | The bike refuses to point into turns | The bike feels too artificial in steering | Stronger values make low-speed turning easier |
| `BikeLeanYawStrength` | How much lean contributes to yaw | The bike should turn more from leaning | Leaning turns too aggressively | Important for natural bike feel |
| `BikeLockedSpeedKPH` | Speed below which lock-style stabilization is used | The bike should stand up more firmly when almost stopped | The bike feels too artificial near standstill | Helps idle stability |
| `BikeLockedAngularDamping` | Damping used in the low-speed locked state | The bike shakes while nearly stopped | The steering feels frozen near zero speed | Pair with `BikeLockedSpeedKPH` |
| `BikeGyroscopicStrength` | Gyroscopic stabilization from wheel spin | The bike needs more stability at speed | The bike resists leaning too much | Helps fast bikes feel planted |
| `BikeGyroFullSpeedKPH` | Speed where full gyro strength is reached | You want full stabilization to ramp in later | You want stronger support earlier | Controls the speed ramp of gyro help |
| `BikeCamberThrustCoeff` | Side force created by lean | The bike should carve turns more strongly | The bike slides or darts sideways too much | Strongly shapes cornering feel |
| `BikeMaxRecoverAngle` | Largest angle the bike will try to recover from | You want more forgiving recovery | The bike recovers from unrealistically extreme angles | Use carefully to avoid arcade over-forgiveness |
| `bEnableBikeDebugLogs` | Turns on bike debug logging | You are diagnosing bike behavior | You are done tuning | Debug only |
| `BikeDebugLogInterval` | How often bike debug logs print | You want more frequent debugging updates | You want fewer logs | Debug only |

### Flight model

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `MaxFlightAngle` | Maximum allowed flight tilt | You want more aggressive tilt | The craft tips too far and feels unstable | Important for rotorcraft and flight-mode vehicles |
| `MaxBrakingPitchScale` | Extra pitch influence during braking in flight | You want stronger nose behavior during braking or decel | Braking causes too much pitch movement | Helps shape arcade-style flight feel |
| `FlightControlResponseSpeed` | How quickly flight inputs respond | The craft feels sluggish | The craft feels twitchy | One of the first flight feel settings to tune |
| `AttitudeStrength` | How strongly the craft tries to hold attitude | You want stronger stabilization | The craft feels too self-correcting | Higher values create safer-feeling flight |
| `AttitudeDamping` | How much flight attitude movement is damped | The craft wobbles after correction | The craft feels too heavy or unresponsive | Works with `AttitudeStrength` |
| `YawSpeed` | Available yaw authority in flight | The craft turns too slowly on yaw | The craft spins too quickly | Important for rotorcraft control feel |
| `RollYawCoupling` | How much roll creates turning help | You want banked turns to steer more naturally | Banking causes too much unwanted yaw | Helps arcade readability |
| `HoverPositionStiffness` | How strongly hover-style vehicles resist drift | Hovering feels loose or floaty | Hovering feels too locked in place | Mainly for vertical-lift craft |
| `FlightLateralDrag` | Sideways drag while flying | The craft slides sideways too much | The craft loses too much momentum in turns | Good for cleaning up messy hover feel |
| `bEnableArcadeTiltAssist` | Extra tilt help for easier flight control | You want more accessible arcade flight | You want more raw manual control | Good default for user-friendly flight |

### Input settings

| Setting | What it controls | Use this asset for | Notes |
| --- | --- | --- | --- |
| `DefaultMappingContext` | The mapping context added for the vehicle | `IMC_VehicleControls` | Usually leave assigned unless you are replacing the whole input layer |
| `ThrottleAction` | Ground throttle input reference | `IA_Throttle` | Ground vehicles |
| `SteeringAction` | Ground steering input reference | `IA_Steering` | Ground vehicles |
| `HandbrakeAction` | Ground handbrake input reference | `IA_HandBrake` | Drift cars and road vehicles |
| `EngineStartAction` | Ground engine on or off input | `IA_EngineStartAction` | Can also be used as a general engine toggle |
| `ClutchAction` | Manual clutch input reference | Your clutch input asset | Only needed for manual-transmission workflows |
| `ShiftUpAction` | Manual upshift input reference | Your shift-up input asset | Used when manual transmission is enabled |
| `ShiftDownAction` | Manual downshift input reference | Your shift-down input asset | Used when manual transmission is enabled |
| `VerticalThrustAction` | Rotorcraft lift input reference | `IA_VerticalThrust` | Helicopters and hover-capable craft |
| `FlightRollAction` | Rotorcraft roll input reference | `IA_HeliRoll` | Flight-mode craft |
| `FlightPitchAction` | Rotorcraft pitch input reference | `IA_HeliPitch` | Flight-mode craft |
| `FlightYawAction` | Rotorcraft yaw input reference | `IA_HeliYaw` | Flight-mode craft |
| `ToggleFlightModeAction` | Flight-mode toggle reference | `IA_ToggleFlightMode` | Needed on vehicles that switch modes |
| `AirplaneThrottleAction` | Airplane throttle reference | `IA_AirplaneThrottle` | Fixed-wing aircraft |
| `AirplaneBrakeAction` | Airplane brake reference | `IA_AirplaneBrake` | Fixed-wing aircraft |
| `AirplanePitchAction` | Airplane pitch reference | `IA_AirplanePitch` | Fixed-wing aircraft |
| `AirplaneRollAction` | Airplane roll reference | `IA_AirplaneRoll` | Fixed-wing aircraft |
| `AirplaneRudderAction` | Airplane yaw or rudder reference | `IA_AirplaneYaw` | Fixed-wing aircraft |
| `AirplaneEngineStartAction` | Airplane engine toggle reference | `IA_EngineStartAction` or a project variant | Fixed-wing aircraft |
| `AirplaneToggleLandingGearAction` | Airplane landing gear toggle reference | `IA_LandingGear` | Required for retractable gear |

## ModularEngineComponent

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `EngineTorque` | Pulling power from the engine | The vehicle accelerates too slowly | The vehicle launches too hard | Main acceleration strength setting |
| `MaxSpeed` | Top-speed target from the engine setup | The vehicle tops out too early | The vehicle is too fast | Works with gearing |
| `IdleRPM` | Engine idle speed | Idle sounds or low-speed response feel too weak | Idle feels too high or unnatural | Also affects sound behavior |
| `MaxRPM` | Highest intended engine speed | You want a higher-revving engine character | You want earlier upshifts or calmer engine behavior | Important with shift thresholds |
| `RevLimiterBounce` | How much the limiter kicks back RPM | You want a more noticeable limiter feel | The limiter is too intrusive | Mostly feel and sound |
| `EngineMomentOfInertia` | How fast the engine revs rise and fall | You want a heavier, smoother engine response | You want quicker rev changes | Shapes engine character |
| `EngineInternalFriction` | Internal engine drag | The engine should lose revs faster off-throttle | The engine drops revs too quickly | Helps control free-rev feel |
| `EngineSound` | Sound asset for the engine | Assign when you want audible engine feedback | Clear only if you do not want engine audio | Purely presentation |
| `AudioPitchMultiplier` | Global pitch scaling for engine sound | The engine audio sounds too low | The engine audio sounds too high | Audio tuning only |
| `bManualTransmission` | Switches between manual and automatic gearing | You want full manual shifting | You want the vehicle to shift itself | Changes which transmission settings matter |
| `GearRatios` | Ratio for each forward gear | You want stronger low-end pull or a different top-end spread | The gearing feels too short or too tall | Large effect on drive feel |
| `DifferentialRatio` | Final drive multiplier | You want more acceleration across all gears | You want calmer revs and more top-end bias | Changes the whole gearing stack |
| `WheelRadiusForGearing` | Wheel size used by transmission math | Your effective gearing feels wrong for the vehicle size | The gearing already matches wheel size correctly | Keep aligned with your actual drive wheels |
| `ShiftUpRPMThreshold` | Auto upshift point as a fraction of max RPM | The auto box hangs onto gears too long | The auto box shifts too early | Only used in automatic mode |
| `ShiftDownRPMThreshold` | Auto downshift point as a fraction of max RPM | The auto box bogs down too much | The auto box downshifts too aggressively | Only used in automatic mode |
| `GearShiftDelay` | Time spent shifting automatically | You want softer, slower auto shifts | You want quicker, sportier shifts | Automatic mode only |
| `ManualShiftDelay` | Time spent shifting manually | You want deliberate manual gear changes | You want snappier manual shifts | Manual mode only |
| `bCanStall` | Allows the engine to stall in manual use | You want a more demanding manual setup | You want easier user-friendly driving | Manual mode only |

## ModularWheelComponent

### Setup, suspension, and steering

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bIsSteerWheel` | Marks this wheel as a steering wheel | The wheel should turn with steering | The wheel should stay fixed | Front wheels usually use this |
| `bIsDriveWheel` | Marks this wheel as powered | The wheel should receive engine drive | The wheel is free-rolling only | Important for front, rear, or all-wheel-drive layouts |
| `bIsHandbrakeWheel` | Marks this wheel as affected by the handbrake | Rear lock-up should come from this wheel | The wheel should ignore handbrake input | Usually rear wheels |
| `bFlipVisualRotation` | Reverses the visible spin direction | The wheel mesh appears to rotate backward | The visible rotation already looks correct | Visual only |
| `ExtensionLength` | Suspension travel length | The vehicle bottoms out too easily | The vehicle stands too tall or feels floaty | Big effect on ride height and travel |
| `Stiffness` | Spring stiffness | The suspension collapses too easily | The suspension feels harsh and rigid | Often auto-managed if suspension is calibrated globally |
| `Damping` | Suspension damping | The wheel keeps bouncing | The suspension feels dead or too stiff | Balance this after stiffness |
| `SuspensionPreloadRatio` | Extra preload holding the vehicle up | The wheel sits too compressed at rest | The suspension feels artificially lifted | Especially useful for landing gear |
| `TyreFriction` | Tire grip level | The wheel slides too much | The wheel grips unrealistically hard | Primary grip control |
| `MudDifficulty` | How badly mud hurts this wheel | Mud should slow this wheel more | Mud effects are too punishing | Terrain-specific behavior |
| `MaxSteeringAngle` | Max angle this wheel can steer | The vehicle needs tighter turning | The steering becomes too sharp or unstable | Important for front axles |
| `bInvertSteering` | Reverses steering for this wheel | The wheel turns the wrong way | The steering direction is already correct | Useful for custom rigs |
| `bAutoInvertRearSteering` | Automatically inverts rear steering when needed | The rear steering setup should counter-steer automatically | You need a custom rear steering direction | Helpful on special steering layouts |
| `WheelMesh` | Visual mesh used by the wheel | Assign when building or replacing visuals | Clear only if visuals are handled another way | Visual only |
| `CollisionChannel` | Channel used for the wheel's ground query | The wheel must read a different collision setup | Default ground detection already works | Keep aligned with your level collision |

### Damage

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bEnableDamage` | Enables wheel failure behavior | The wheel should be damageable | The wheel should never fail | Turn off for indestructible demo builds |
| `FailureDamageThreshold` | Damage needed before the wheel fails | The wheel breaks too easily | The wheel never seems to fail | Main wheel failure threshold |
| `DamageColliderRadius` | Size of the wheel damage collider | Impacts are not being detected reliably | The collider is too large and triggers too often | Leave at auto-feeling values unless needed |
| `FailedGripMultiplier` | Remaining grip after failure | A failed wheel should still hold more grip | A failed wheel should feel much worse | Lower values mean stronger failure penalty |
| `FailedBrakeMultiplier` | Remaining brake strength after failure | A damaged wheel should still brake better | A failed wheel should barely brake | Important for believable failure handling |
| `FailedDriveForceMultiplier` | Remaining drive force after failure | A failed driven wheel should still pull a bit | A failed wheel should contribute very little | Very noticeable on driven axles |
| `FailedRollingResistance` | Extra drag from a failed wheel | Failure should slow the vehicle more | Failure feels too punishing | Helps sell wheel damage |
| `FailedStaticMesh` | Visual mesh shown after failure | Assign when you want a broken-wheel visual | Clear if you do not use a broken mesh | Presentation only |

## ModularSteeringWheelComponent

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `MaxSteeringAngle` | Maximum visual steering-wheel rotation | You want more dramatic cockpit movement | The wheel spins too far visually | Visual only |
| `RotationAxis` | Axis used for the wheel mesh rotation | The wheel rotates around the wrong axis | The wheel already turns correctly | Must match your steering wheel mesh orientation |
| `bInvertRotation` | Reverses steering-wheel visual rotation | The wheel turns the wrong way visually | The visual direction is already correct | Visual only |
| `RotationInterpSpeed` | How quickly the wheel mesh catches up | The wheel should move more responsively | The wheel should move more smoothly and softly | Visual feel only |
| `SteeringWheelMesh` | Steering wheel visual mesh | Assign your cockpit wheel mesh | Clear only if handled elsewhere | Visual only |
| `MeshScale` | Scale of the steering wheel mesh | The mesh is too small | The mesh is too large | Visual only |

## ModularRotorComponent

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `LiftPowerMultiplier` | Lift output from the rotor | The craft cannot lift reliably | The craft shoots upward too easily | Main rotor lift strength |
| `MaxRotationSpeed` | Rotor spin cap | You want faster rotor visual and lift ramp ceiling | The rotor feels too aggressive | Works with spool-up |
| `SpoolUpTime` | Time needed to reach active rotor speed | You want a heavier startup feel | You want faster response off the ground | Strongly affects takeoff feel |
| `bFlipRotation` | Reverses rotor spin direction visually | The rotor spins the wrong way | The visual direction is correct | Visual only |
| `RotorSound` | Rotor sound asset | Assign for audible rotor feedback | Clear if you do not want rotor audio | Presentation only |
| `MinPitch` | Lowest rotor audio pitch | You want lower idle rotor sound | The idle sound is too low | Audio only |
| `MaxPitch` | Highest rotor audio pitch | You want a sharper high-power rotor sound | The top end is too shrill | Audio only |
| `bEnableFlightLogs` | Enables rotor debug logging | You are diagnosing flight behavior | You are done tuning | Debug only |
| `LogInterval` | How often flight logs are written | You want more frequent logs | You want fewer logs | Debug only |
| `RotorMesh` | Rotor visual mesh | Assign when building the craft | Clear only if another visual system is used | Visual only |
| `RotorScale` | Rotor mesh scale | The mesh size is wrong | The mesh is oversized | Visual only |
| `bEnableDamage` | Enables rotor damage | Rotor strikes should matter | Rotor should never fail | Gameplay choice |
| `FailureDamageThreshold` | Damage needed to fail the rotor | The rotor breaks too easily | The rotor feels indestructible | Main rotor failure threshold |
| `DamageColliderRadius` | Size of the rotor damage collider | Rotor strikes are missed | Collisions trigger too easily | Tune only if needed |
| `AutoBreakMinPowerRatio` | Minimum power needed before obstacle strikes can auto-break the rotor | Slow-moving blades should still break on contact | You want auto-break only at more dangerous speeds | Damage realism control |
| `bBlockSpinOnObstacleContact` | Stops spinning when the rotor is blocked | You want obstacle contact to jam the rotor | You want the rotor to keep spinning through minor contacts | Strong effect on failure behavior |
| `WorldContactDamageInterval` | Delay between world-contact damage applications | Repeated contact should damage faster | Repeated contact is too punishing | Helps control continuous collision damage |
| `FailedStaticMesh` | Broken rotor mesh | Assign if you want a failure visual | Leave empty if not needed | Presentation only |

## ModularPropellerComponent

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `MaxThrustForce` | Maximum forward thrust | The aircraft struggles to accelerate or climb | The aircraft has too much raw thrust | Main propulsion strength |
| `ReverseThrustFactor` | How much reverse thrust is available | You want stronger reverse or braking thrust | Reverse thrust feels too strong or unrealistic | Not all aircraft should use much |
| `MaxRotationSpeed` | Propeller spin cap | You want more top-end propeller activity | The propeller feels too aggressive | Works with spool-up and audio |
| `SpoolUpTime` | Time to reach operating speed | You want a slower engine or prop response | You want faster throttle response | Shorter values feel more immediate |
| `IdlePowerRatio` | Power held at idle | The plane loses too much propulsion at idle | Idle thrust keeps the plane moving too much | Affects taxi and low-throttle flight |
| `bUseAutomaticAirborneIdleRatio` | Auto-calculates airborne idle power | You want easier setup for mixed flight conditions | You want full manual control of airborne idle | Recommended for most users |
| `ManualAirborneIdlePowerRatio` | Fixed airborne idle power when auto mode is off | The plane should keep more thrust with throttle released in the air | The plane should settle more when throttle is off | Only used when auto airborne idle is disabled |
| `bFlipRotation` | Reverses propeller spin direction visually | The propeller spins the wrong way | The visual direction is correct | Visual only |
| `VisualRotationAxis` | Spin axis for the propeller visual | The propeller rotates around the wrong axis | The axis already matches | Visual only |
| `PropellerSound` | Propeller sound asset | Assign if you want propeller audio | Clear if not needed | Presentation only |
| `MinPitch` | Lowest propeller audio pitch | The low-power sound is too high | The sound is already low enough | Audio only |
| `MaxPitch` | Highest propeller audio pitch | You want a brighter high-power sound | The top end is too sharp | Audio only |
| `PropellerMesh` | Propeller visual mesh | Assign when building visuals | Clear only if another system handles visuals | Visual only |
| `PropellerScale` | Scale of the propeller mesh | The mesh is too small | The mesh is too large | Visual only |
| `bEnableDamage` | Enables propeller damage | Prop strikes should cause failure | The propeller should not fail | Gameplay choice |
| `FailureDamageThreshold` | Damage needed for failure | Propellers break too easily | Propellers are too hard to damage | Main failure threshold |
| `DamageColliderRadius` | Size of the damage collider | Hits are not detected | The collider triggers too widely | Tune only if needed |
| `AutoBreakMinPowerRatio` | Minimum power before auto-break from strikes | Slow blades should still auto-break | Auto-break should only happen at stronger rotation | Damage realism control |
| `bBlockSpinOnObstacleContact` | Stops prop spin when blocked | You want visible obstruction behavior | You want softer reactions to contact | Strong failure-behavior setting |
| `WorldContactDamageInterval` | Rate of repeated contact damage | Continuous contact should damage faster | Continuous contact is too punishing | Useful when testing tight airport scenes |
| `FailedStaticMesh` | Broken propeller mesh | Assign for visual feedback | Leave empty if not needed | Presentation only |

## ModularAirplaneComponent

### Manual handling

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `TakeoffSpeedKPH` | Target speed where takeoff should feel comfortable | The plane lifts too early and feels too floaty | The plane needs too much speed to rotate | One of the most important airplane values |
| `PitchControlSensitivity` | Direct pitch input sharpness | The nose feels slow to respond | Pitch feels too twitchy | Player feel tuning |
| `RollControlSensitivity` | Direct roll input sharpness | Banking feels sluggish | Roll feels too twitchy | Player feel tuning |
| `YawControlSensitivity` | Direct yaw input sharpness | Rudder response feels weak | Yaw feels too snappy | Player feel tuning |
| `PitchHandlingResponsiveness` | Overall pitch agility | The aircraft feels heavy in pitch | The aircraft feels nervous | Bigger aircraft usually need calmer values |
| `RollHandlingResponsiveness` | Overall roll agility | The wings respond too slowly | Roll response is too aggressive | Strong effect on general flight feel |
| `YawHandlingResponsiveness` | Overall yaw agility | Rudder authority feels weak | Yaw feels too strong | Balance with stability values |
| `StabilityAssist` | Automatic help against wobble and unwanted motion | The aircraft is too hard to keep smooth | The aircraft feels overly self-correcting | Good first stop for user-friendliness |
| `bUseArcadeFlightAssist` | Enables extra arcade flight helpers | You want easy, forgiving flight | You want a more manual flight model | Recommended for broader user accessibility |

### Debug

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bDebugFlightLogs` | Writes fixed-wing debug logs | You are diagnosing AI or manual flight | You are done tuning | Debug only |
| `bDebugFlightPathSplines` | Draws debug flight splines | You want to see planned air paths | Debug visuals are no longer needed | Very useful when tuning AI routes |
| `DebugFlightLogInterval` | Log and debug refresh timing | You want more frequent updates | You want less visual or log noise | Debug only |

### AI flight behavior

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `AITaxiSpeedKPH` | Taxi speed used by AI aircraft | Taxiing is too slow | Taxiing is too aggressive | Ground behavior only |
| `AIRotationSpeedMultiplier` | Speed target around takeoff rotation | AI rotates too late | AI rotates too early | Multiplies against the airplane's flight characteristics |
| `AIClimbSpeedMultiplier` | Target speed during climb | AI struggles to climb cleanly | AI accelerates away too aggressively after takeoff | Helps define early flight behavior |
| `AICruiseSpeedMultiplier` | Target speed during cruise | AI cruises too slowly | AI feels too fast or unstable in cruise | Main cruise pacing control |
| `AIApproachSpeedMultiplier` | Target speed on approach | AI approaches too slowly and stalls | AI arrives too hot | Important for runway reliability |
| `AIFlareSpeedMultiplier` | Target speed during flare | AI drops too hard | AI floats too much | Fine landing behavior control |
| `AIApproachDescentScale` | Aggressiveness of approach descent | AI stays too high or descends too gently | AI dives too hard | Strong approach tuning control |
| `AIBankedDescentPitchOverrideScale` | How much bank lift can fight downward descent pitch | Banked descents lose too much altitude | The AI refuses to descend cleanly while turning | Useful on heavy or sluggish planes |
| `AIBankedRudderAltitudeScale` | Rudder help for holding altitude in banked turns | The AI loses altitude during turns | Turns feel too forceful or odd | Good for heavier aircraft |
| `bAIDisableSideSlipYawSuppression` | Allows full rudder even when side-slip protection would reduce it | Heavy planes need maximum rudder authority | You want cleaner anti-side-slip behavior | Use carefully |
| `AIFinalMaxDescentAngleDegrees` | Max descent angle during final | Final approach stays too high | Final approach is too steep | Set to `0` to rely on the broader descent scale |
| `AICruiseMaxClimbAngleDeg` | Max climb angle during cruise corrections | Cruise altitude recovery is too weak | Cruise corrections are too sharp | Helps smooth cruise altitude changes |
| `AICruiseMaxDescentAngleDeg` | Max descent angle during cruise corrections | Cruise descent is too weak | Cruise descent corrections dive too much | Cruise smoothing control |
| `AIApproachMaxClimbAngleDeg` | Max climb angle during approach recovery | Approach recovery is too weak | Approach climb corrections are too steep | Helps recover from low approach lines |
| `AIApproachMaxDescentAngleDeg` | Max descent angle during approach | The AI takes too long to come down | The approach is too steep | Key landing reliability setting |
| `AICruiseAltitudePitchScale` | Pitch authority for cruise altitude correction | Altitude drift is corrected too softly | Cruise pitch corrections overreact | Lower values smooth cruise behavior |
| `AIApproachAltitudePitchScale` | Pitch authority for approach altitude correction | Approach altitude correction is too weak | Approach corrections are too abrupt | Helps create a smoother glide path |
| `AICruiseBankAngleDegrees` | Allowed bank angle in cruise | The AI turns too slowly | Cruise turns are too sharp | Larger values tighten route turns |
| `AIApproachBankAngleDegrees` | Allowed bank angle on approach | Approach turns need more authority | Approach turns should be gentler | Helps keep approaches believable |
| `AIFinalBankAngleDegrees` | Allowed bank angle during final | Final alignment needs more turning authority | Final should stay flatter and calmer | Lower values help more stable finals |
| `AITurnAnticipationScale` | How early the AI prepares for turns | The AI turns too late | The AI begins turns too early | Route-shaping helper |
| `AIFlareHeightCM` | Height where flare begins | Touchdowns are too hard | The aircraft floats too long | Main flare trigger height |
| `AIRolloutBrakeStrength` | Brake strength after touchdown | Runway rollout is too long | The aircraft stops too abruptly | Ground landing behavior |
| `AIMaxAirBrakeInput` | Max AI air-brake usage | Approaches stay too fast | Air brakes are too strong | Works with descent tuning |
| `AIApproachSplineAirBrakeCutoffDistanceCM` | Distance from touchdown where AI stops using approach air brake | Air brake holds too close to landing | Air brake ends too early | Fine-tunes the last part of approach |
| `AIGroundFlareTriggerHeightCM` | Ground-detected height where touchdown pull-back begins | The flare begins too late | The flare begins too high | Important for soft landing feel |
| `AIGroundFlarePitchInput` | Extra nose-up pitch used in the ground flare | Touchdown is too flat or firm | The nose rises too aggressively | Fine landing tuning |
| `AIGroundFlareAirBrakeInput` | Extra air brake during the ground flare | The aircraft floats too much | The aircraft slows too much before touchdown | Helps control float vs sink |
| `AIGroundFlareTraceLengthCM` | How far down the AI checks for ground | Ground detection misses too often | The trace is longer than needed | Important when gear positions differ a lot |
| `AirBrakeMaxDecelerationG` | Max deceleration from air brake | Air brake effect is too weak | Air brake effect is too strong | Balances approach energy |

### Path planning

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bUseCustomPathPlanningSettings` | Allows the plane to use its own path-planning values | This aircraft needs custom route shaping | You want shared or default planner behavior | Recommended for very different aircraft types |
| `bAutoCalibratePathPlanningNow` | Rebuilds path-planning values from the current aircraft setup | You want a fresh baseline quickly | You have already hand-tuned the path | One-time calibration helper |
| `ClimboutBendAngleDeg` | Sharpness of early departure turns | The plane needs tighter early turns | The plane should leave the runway on wider turns | Smaller angles create wider turns |
| `DeparturePhaseLengthCM` | Length of the climbout phase | The plane needs more distance before broader routing | Departure feels too long | Helps heavy aircraft settle after takeoff |
| `PostClimboutStraightDistanceCM` | Extra straight flight after climbout | Heavy aircraft need more time to build speed | The plane waits too long before turning | Good for large fixed-wing aircraft |
| `NormalBendsMaxAngleDeg` | Sharpness of cruise turns | The route needs tighter cruise turns | Cruise routing should be wider and smoother | Smaller angles make larger arcs |
| `CruisePhaseLengthCM` | Length budget for cruise shaping | The route needs more room to settle | Cruise paths feel too stretched out | Shapes mid-flight routing |
| `ApproachEntryMaxAngleDeg` | Sharpness allowed at approach entry | Approach alignment needs tighter turns | Final setup should be wider and calmer | Good landing stability control |
| `ApproachPhaseLengthCM` | Length of the approach phase | Approaches need more room to line up | Approach starts too far away | Strong runway-approach control |
| `DescentAngleDeg` | Planned descent angle | The aircraft should descend faster over shorter distance | The aircraft should use a longer, flatter descent | Very important for runway spacing |

## ModularLandingGearComponent

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bIsRetractable` | Whether this gear can retract | The aircraft should retract its gear | The gear should always stay deployed | Turn off for fixed gear |
| `bStartRetracted` | Starting retracted state | The aircraft should begin with gear up | You want the gear down at spawn | Mostly used for special spawn cases |
| `RetractionTime` | Time to retract or deploy | You want slower, heavier animation | You want quicker gear action | Visual and gameplay pacing |
| `DeployedLocalTransform` | Gear transform when deployed | The gear sits wrong in the open state | The deployed pose already fits | Placement and animation base |
| `RetractedLocalTransform` | Gear transform when retracted | The gear does not fold into the body correctly | The retracted pose already works | Only matters for retractable gear |
| `RetractedVisualOffset` | Extra visual offset when retracted | The folded wheel still clips or sits wrong | The folded mesh moves too far | Visual cleanup |
| `RetractedVisualScale` | Visual scale when retracted | The folded wheel should appear more compact | The folded wheel is too tiny | Visual only |
| `bHideWheelWhenRetracted` | Hides wheel visuals when retracted | The retracted gear should disappear fully | You want to see the folded wheel | Visual preference |
| `DeployedDragMultiplier` | Extra drag while gear is down | Gear-down flight should feel slower | Gear-down penalty feels too strong | Important for airplane behavior |
| `DeployedControlAuthorityMultiplier` | Reduction in control authority while gear is down | Gear-down handling still feels too clean | Gear-down handling feels too weak | Helps distinguish landing configuration |
| `bDebugGroundForces` | Enables landing-gear debug logs | You are diagnosing touchdown or rollout issues | Tuning is done | Debug only |
| `GroundDebugLogInterval` | Landing-gear debug log frequency | You want more frequent logs | You want fewer logs | Debug only |

## ModularDamageComponent

### Failure effects

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `RotorFailureNiagaraSystem` | Niagara effect for failed rotors | Assign when rotor failure needs a burst effect | Clear if not using one | Presentation only |
| `PropellerFailureNiagaraSystem` | Niagara effect for failed propellers | Assign when propeller failure needs a burst effect | Clear if not using one | Presentation only |
| `WheelFailureNiagaraSystem` | Niagara effect for failed wheels | Assign when wheel failure needs a burst effect | Clear if not using one | Presentation only |
| `RotorFailureSmokeNiagaraSystem` | Persistent smoke for failed rotors | You want lingering damage feedback | Clear if not needed | Presentation only |
| `PropellerFailureSmokeNiagaraSystem` | Persistent smoke for failed propellers | You want lingering damage feedback | Clear if not needed | Presentation only |
| `WheelFailureSmokeNiagaraSystem` | Persistent smoke for failed wheels | You want lingering damage feedback | Clear if not needed | Presentation only |
| `RotorFailureSound` | Audio played on rotor failure | Assign when you want failure sound | Clear if not needed | Presentation only |
| `PropellerFailureSound` | Audio played on propeller failure | Assign when you want failure sound | Clear if not needed | Presentation only |
| `WheelFailureSound` | Audio played on wheel failure | Assign when you want failure sound | Clear if not needed | Presentation only |
| `bUseBuiltInFailureExplosion` | Uses the built-in procedural explosion style | You want quick setup without custom effects | You already have your own effect package | Expands the explosion settings below |

### Built-in failure explosion

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `Preset` | Built-in effect style preset | You want a different overall effect style | Current preset already fits | Overall presentation choice |
| `Flash.Duration` | How long the flash lasts | The flash disappears too quickly | The flash lingers too long | Visual only |
| `Flash.InitialScale` | Starting size of the flash | The flash is too small | The flash is too large | Visual only |
| `Flash.ExpansionRate` | Speed the flash grows | The flash should expand faster | The flash should stay tighter | Visual only |
| `Flash.FadeOutStartTime` | When the flash begins to fade | The flash should stay brighter longer | The flash should fade earlier | Visual only |
| `Flash.Tint` | Flash color | You want a different flash tone | Current tint is already right | Visual only |
| `Flash.LightIntensity` | Flash light brightness | The explosion light is too dim | The light is too intense | Visual only |
| `Flash.LightRadius` | Flash light reach | The light should affect a larger area | The light reaches too far | Visual only |
| `Fireball.Duration` | How long the fireball lasts | The fireball ends too quickly | The fireball lingers too long | Visual only |
| `Fireball.InitialScale` | Starting fireball size | The fireball is too small | The fireball starts too large | Visual only |
| `Fireball.ExpansionRate` | Fireball growth speed | The fireball should swell faster | The fireball should stay tighter | Visual only |
| `Fireball.FadeOutStartTime` | When the fireball begins fading | The fireball should stay visible longer | The fireball should fade earlier | Visual only |
| `Fireball.Tint` | Fireball color | You want a different heat tone | Current tint works | Visual only |
| `Fireball.LightIntensity` | Fireball light brightness | You want extra fireball lighting | The effect is already bright enough | Visual only |
| `Fireball.LightRadius` | Fireball light reach | You want a wider lit area | The light spreads too far | Visual only |
| `Smoke.Duration` | How long smoke lasts | Smoke should linger longer | Smoke should clear faster | Visual only |
| `Smoke.InitialScale` | Starting smoke size | Smoke starts too small | Smoke starts too large | Visual only |
| `Smoke.ExpansionRate` | Smoke growth speed | Smoke should spread faster | Smoke should stay tighter | Visual only |
| `Smoke.FadeOutStartTime` | When smoke starts fading | Smoke should stay dense longer | Smoke should fade sooner | Visual only |
| `Smoke.Tint` | Smoke color | You want lighter or darker smoke | Current tint fits | Visual only |
| `Smoke.LightIntensity` | Smoke light brightness | Rarely increased unless stylized | Usually lower or zero | Visual only |
| `Smoke.LightRadius` | Smoke light reach | Rarely increased unless stylized | Usually lower or zero | Visual only |
| `RotorScaleMultiplier` | Explosion scale boost for rotor failures | Rotor explosions look too small | Rotor explosions look too big | Part-specific presentation |
| `PropellerScaleMultiplier` | Explosion scale boost for propeller failures | Propeller explosions look too small | Propeller explosions look too big | Part-specific presentation |
| `WheelScaleMultiplier` | Explosion scale boost for wheel failures | Wheel explosions look too small | Wheel explosions look too big | Part-specific presentation |
| `bSpawnPointLight` | Spawns a temporary light with the built-in effect | You want brighter visual impact | You want a cheaper or darker effect | Presentation only |

### Deformation, performance, and collision

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `DeformationRadius` | Size of the denting area | Impacts affect too small an area | Dents spread too widely | Visual damage size |
| `DeformationStrength` | Strength of mesh deformation | Damage dents are too weak | Damage looks too extreme | Main dent intensity control |
| `DeformationFalloffPower` | How sharply deformation fades from the impact center | You want a tighter concentrated dent | You want a broader softer dent | Shapes the dent profile |
| `MaxDeformationDepth` | Maximum dent depth | Damage is too shallow | Meshes cave in too much | Helps prevent extreme visual collapse |
| `bUseAsyncDeformation` | Performs deformation work asynchronously | You want smoother runtime performance | You want simpler synchronous behavior | Usually good for performance |
| `SpatialGridCellSize` | Cell size used for deformation search work | You want coarser, broader search cells | You want finer deformation targeting | Performance and precision tradeoff |
| `MaxVerticesPerDeform` | Max vertices changed per impact | Dents are not detailed enough | Performance cost is too high | Higher values cost more |
| `MaxFacesPerDeform` | Max faces affected per impact | Dents need more spread or detail | Performance is too expensive | Higher values cost more |
| `MinImpactForceThreshold` | Minimum impact needed to cause collision damage | Minor bumps should also damage | Too many tiny bumps are causing damage | Helps filter noise |
| `CollisionForceMultiplier` | Scales impact force into damage | Collisions do too little damage | Collisions do too much damage | One of the main collision-damage controls |
| `DamageZones` | Named damage regions for grouped damage tracking | Add or expand zones when you need local damage feedback | Remove unused zones to simplify setup | Helpful for custom body sections |

## DamageInflictorComponent

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `DamageAmount` | How much damage this inflictor applies | The hit should damage parts more strongly | The hit should be less punishing | Main per-hit damage amount |
| `TargetPartType` | Which part type this hit is aimed at | You want to target a specific system | You want generalized behavior instead | Useful for specialized hazards |
| `bAutoApplyOnHit` | Applies damage automatically on hit events | Physical hits should damage automatically | You want manual control over when damage is applied | Good for simple hazards |
| `bAutoApplyOnOverlap` | Applies damage automatically on overlap events | Overlap volumes should inflict damage | You want only direct hits to damage | Good for danger zones and moving hazards |

## ModularVehicleAIComponent

### Core AI setup

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `DefaultBehaviorTree` | Behavior tree the AI should run by default | Assign when using the shipped or custom AI tree | Clear only if something else starts the tree | Use `BT_Vehicles` for the included setup |
| `GroundLookAheadDistance` | Forward look-ahead distance used by ground AI | Ground AI reacts too late | Ground AI looks too far ahead and cuts corners badly | Ground route feel |
| `bEnableFixedWingParallelSplineRecovery` | Helps AI fixed-wing aircraft recover around missed spline paths | Aircraft need extra recovery help after route drift | Recovery feels too forgiving or unnecessary | Best kept on for most AI airplanes |

### Traffic roaming

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bAutoCycleTrafficTargets` | Lets the AI keep selecting new roaming targets | You want endless traffic roaming | You want single-destination AI only | Useful for ambient traffic |
| `TrafficTargetDistanceCM` | Distance used when picking random traffic targets | Traffic should roam farther away | Traffic should stay more local | Roaming range |
| `TrafficRandomAngleHalfConeDeg` | Spread of random target direction | Traffic should choose a wider variety of directions | Traffic should stay more focused forward | Higher values create more randomness |
| `TrafficTargetZOffsetCM` | Vertical offset used for target generation | Targets need to sit higher or lower | Vertical offset is causing wrong destinations | Useful for multi-level scenes |
| `DeniedTrafficLaneProfiles` | Lane profile names the AI should avoid | You want to block this vehicle from certain lane types | The AI should be allowed on more lanes | Keep aligned with your lane-tag system |

### Flight config values used by move requests

These values are chosen when you tell an AI aircraft where to go.

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `CruisingAltitude` | Cruise height target | Aircraft need more clearance over terrain or scenery | The aircraft flies higher than needed | Important for airports near obstacles |
| `bLandOnArrival` | Whether the AI should land at the destination | The trip should end with a landing | The aircraft should remain airborne on arrival | Core travel intent |
| `LandingTaxiDistance` | Taxi distance after landing | Aircraft should roll farther after touchdown | Taxi rollout should finish sooner | Ground arrival pacing |
| `AcceptanceRadius` | Distance considered close enough to the destination | The AI should accept a looser arrival | The AI needs tighter precision | Important for scene scale |
| `ApproachSpeedLimitKPH` | Speed limit for approach behavior | The aircraft approaches too hot | The aircraft approaches too slowly | Landing stability tuning |
| `CruiseLookAheadDistance` | Forward planning distance during cruise | Cruise steering reacts too late | Cruise steering feels too broad | Helps route following |
| `ApproachFlareLookAheadDistance` | Planning distance before flare | Flare begins too late | Flare begins too early | Works with airplane flare settings |
| `GroundTaxiLookAheadDistance` | Planning distance while taxiing | Taxiing reacts too late | Taxiing cuts corners too much | Ground runway behavior |
| `bAutoAssignRandomRunwayOnArrival` | Lets the AI choose a random runway after arrival | You want continuing airport traffic behavior | You need a fixed runway assignment | Good for ambient airport scenes |
| `AutoAssignRandomRunwayDelaySeconds` | Delay before a random runway is assigned | You want more pause before the next assignment | You want faster turnaround | Only used when random runway assignment is enabled |

## ModularRunwayActor

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `TaxiSeparationBufferCM` | Safety gap between taxiing aircraft on the runway taxi spline | Aircraft queue too close together | Taxi spacing feels too wide or cautious | Main anti-collision spacing value for taxi traffic |

### Runway setup notes

- The runway actor also contains approach, takeoff, and taxi references that need correct placement and orientation in the level.
- Always point the runway so approach and takeoff directions face usable airspace.
- Test one runway thoroughly before adding several.

## ModularRoadSpline

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `SpeedLimitKPH` | Speed limit for this road spline | Traffic on this spline should move faster | Traffic on this spline should move slower | Local road pacing |
| `LaneTagName` | Tag name used to identify the road or lane type | Use a more specific lane profile name | Simplify only if fewer lane types are needed | Must stay consistent with traffic systems |
| `RoadInfluenceRadius` | How far this road's influence reaches | Vehicles should detect or associate with the road from farther away | Overlapping road influence is causing confusion | Useful for broad roads and junction areas |
| `AssociatedZoneShape` | Editor helper linking the road to a zone shape | Use when organizing or tracing editor setup | Ignore if your workflow does not need it | Editor hint only |

## ModularTrafficManager

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `AllowedLaneTags` | Which lane tags the manager should consider valid | More lane profiles should be active in the scene | Fewer lane profiles should be used | Keep aligned with the road and light setup |
| `GlobalSpeedLimitKPH` | Default traffic speed cap | The whole traffic scene should move faster | The scene should feel calmer or more urban | Global fallback speed |
| `LaneProfilePriorities` | Relative priority for lane profiles | Certain roads should be favored more strongly | Priority differences are too strong | Affects route preference and junction behavior |
| `bDriveOnLeft` | Sets left-side driving rules | The project uses left-side traffic | The project uses right-side traffic | A foundational traffic-direction choice |
| `bDrawJunctionDebugSpheres` | Draws junction markers | You are diagnosing intersection detection | Debug is no longer needed | Debug only |
| `bDrawJunctionFlowDebug` | Draws live junction state debugging | You are diagnosing traffic flow at junctions | Debug is no longer needed | Debug only |
| `JunctionClearanceTimeoutSec` | Max wait before the manager forces movement through deadlock | Vehicles wait too long in deadlocks | Vehicles force through too aggressively | Deadlock breaker |
| `JunctionMinClearanceGapSec` | Minimum spacing between granted clearances | Vehicles tailgate through junctions | Junction throughput feels too slow | Safety vs flow tradeoff |
| `JunctionStarvationOverrideSec` | Wait time before low-priority traffic gets help | Low-priority traffic starves too long | Priority rules should stay stronger longer | Fairness control |
| `JunctionSimultaneousArrivalWindowSec` | Window used to treat arrivals as simultaneous | More vehicles should be judged as arriving together | Arrival resolution should be stricter | Helps right-of-way decisions |
| `JunctionClearDistMultiplier` | Extra distance required to count a junction as cleared | Vehicles release junction ownership too early | Vehicles hold junction ownership too long | Important for overlap-heavy scenes |
| `JunctionExitStallTimeoutSec` | Time before a stalled exiting vehicle stops blocking the junction | Exiting traffic blocks too long | Vehicles stop blocking too soon | Helps gridlock recovery |
| `JunctionExitStallSpeedCMS` | Speed below which exiting traffic counts as stalled | Slow vehicles should be treated as stalled earlier | Only almost-stopped vehicles should count as stalled | Fine-tunes exit recovery behavior |

## ModularTrafficSpawner

### Main traffic setup

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `TrafficVehicleEntries` | Weighted list of vehicle classes and spawn rules | You want more variety or different class weighting | You want a tighter curated pool | Main traffic-vehicle list |
| `TrafficBehaviorTree` | Behavior tree run by spawned traffic | Assign when using a specific traffic AI tree | Leave only if traffic is initialized some other way | Use the shipped AI tree when appropriate |
| `SpawnCount` | Number of vehicles to spawn | The scene needs denser traffic | The scene is too crowded or heavy | Start low, then scale up |
| `MinSpacingCM` | Minimum spacing between spawn points | Spawned traffic appears too close together | Spawn density is too low | Important for clean starts |
| `MinRoadLength` | Minimum road length eligible for spawning | More short roads should receive traffic | Tiny roads are producing awkward spawns | Good traffic-quality filter |
| `SpawnHeightOffsetCM` | Vertical offset applied at spawn | Vehicles clip into the ground on spawn | Vehicles float too high at spawn | Quick placement correction |
| `InitialTargetDistanceCM` | First target distance for spawned traffic | Traffic should start with longer routes | Traffic should stay more local | Initial roaming range |
| `AllowedLaneTags` | Which lane tags the spawner may use | More road types should be used for spawns | Spawns should stay to fewer lane types | Must match your traffic naming |
| `bDrawDebugSpawns` | Draws spawn debug visuals | You are diagnosing spawn placement | Debug is no longer needed | Debug only |

### Optimization

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bOptimizeSpawning` | Enables distance and density optimization | You want smarter traffic upkeep | You want simpler always-on spawning | Recommended for most scenes |
| `MaxDespawnDistance` | Distance beyond which traffic is removed | Far vehicles should stay alive longer | Far vehicles should be cleaned up sooner | Performance control |
| `MaxSpawnDistance` | Furthest distance where new traffic may spawn | You want traffic appearing farther from the player | You want tighter spawn bands | Balances coverage and visibility |
| `MinSpawnDistance` | Closest distance where new traffic may spawn | You want traffic to appear closer | You want to hide pop-in by spawning farther away | Very important for presentation |
| `OptimizationUpdateInterval` | How often optimization runs | Spawner reacts too slowly to scene changes | Optimization checks are too frequent | Performance tradeoff |
| `MaxGridlockTime` | How long a jammed vehicle can persist before optimization reacts | Traffic should tolerate longer jams | Gridlock should be cleaned up sooner | Useful in busy city scenes |
| `GridlockDistanceThreshold` | Distance used to judge gridlock movement | Small movement should still count as stuck | Tiny movement should not count as stuck | Jam detection sensitivity |

### Density balancing

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `NumDensitySectors` | Number of sectors used for local density balancing | You want finer density control around the player | Simpler balancing is enough | More sectors means more granularity |
| `MaxSectorFraction` | Max share of traffic allowed in one sector | You want a denser hot spot in a direction | You want more even distribution around the player | Helps keep traffic balanced |
| `LocalDensityRadius` | Radius used for local density checks | Density control should consider a larger area | Density control should stay more local | Balance scene feel vs responsiveness |
| `MaxLocalDensityCount` | Max nearby vehicles before local spawning is restricted | You want heavier local density | You want tighter anti-clumping behavior | Good for preventing crowded pockets |
| `bAdaptiveCongestionDespawn` | Despawns adaptively when congestion builds | You want automatic cleanup in jams | You want traffic to persist even in congestion | Scene-stability control |
| `MinCongestionDespawnDistance` | Closest distance where congestion-based despawn is allowed | Despawn should happen farther from the player only | Despawn can happen somewhat closer | Helps hide cleanup from the player |

## TrafficLightIntersection

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `Phase1LaneTags` | Lane tags that are green during phase 1 | More lanes should flow in phase 1 | Fewer lanes should flow in phase 1 | Must match your road tag naming |
| `Phase2LaneTags` | Lane tags that are green during phase 2 | More lanes should flow in phase 2 | Fewer lanes should flow in phase 2 | Must match your road tag naming |
| `GreenPhaseSec` | Length of the green phase | Intersections should keep one direction moving longer | Light changes should happen more often | Major traffic rhythm setting |
| `YellowPhaseSec` | Length of the yellow phase | Drivers need more transition time | The intersection spends too long changing | Safety timing control |
| `IntersectionRadiusCM` | Area around the light treated as the intersection | More space should belong to the light-controlled zone | The light influences too wide an area | Keep scaled to the actual junction size |

## MassTrafficSettings

These settings appear in **Project Settings > Plugins > Mass Traffic**.

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `bDrawDebugTraffic` | Draws Mass traffic debug visuals | You are diagnosing Mass traffic behavior | Debug is no longer needed | Debug only |
| `DebugArrowScale` | Size of Mass traffic debug arrows | Debug arrows are too small | Debug arrows are too large | Debug only |
| `MaxTrafficEntities` | Global cap on Mass traffic entities | You want more Mass traffic in the scene | Performance needs a lower cap | Primary Mass traffic scale control |
| `MaxDeltaTimeSec` | Maximum step size the Mass traffic update will process | You want more tolerance for frame hiccups | You want tighter update stepping | Helps keep behavior stable under load |

## MassTrafficVehicleTrait

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `MaxSpeedOverrideKPH` | Per-entity speed override | This Mass vehicle type should drive faster | This Mass vehicle type should drive slower | `0` means no override |
| `MinFollowGapCM` | Minimum following gap | Vehicles should keep more space | Vehicles should pack more tightly | Mass traffic spacing |
| `TimeHeadwaySec` | Time-based following gap | Traffic should react more cautiously | Traffic should feel denser and more aggressive | Works with follow gap |
| `JunctionApproachCM` | Distance used before junction behavior starts | Vehicles should prepare earlier for junctions | Vehicles should wait longer before reacting | Junction pacing |
| `MaxYieldTimeSec` | Max time spent yielding | Vehicles should wait longer before forcing progress | Vehicles should resolve sooner | Flow vs caution |
| `DirectionBias` | Direction preference strength | Traffic should favor one direction more strongly | Traffic should spread more evenly | Route-bias control |
| `VehicleMesh` | Visual mesh for this Mass traffic type | Assign when creating a traffic type | Clear only if visuals are provided elsewhere | Presentation only |

## MassTrafficSpawnPointsGenerator

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `SpawnMinGapCM` | Minimum gap between generated Mass spawn points | Mass traffic starts too tightly packed | You want denser initial placement | Initial spawn spacing |
| `SpawnMaxGapCM` | Maximum gap between generated Mass spawn points | Spawn points feel too sparse | Spawn points spread too widely | Helps control large-lane spawn distribution |

## Behavior tree tuning used by the shipped AI setup

## BTService_SelectMovementMode

| Setting | What it controls | Notes |
| --- | --- | --- |
| `MovementModeKey` | Blackboard key that stores whether the AI should be in ground or air behavior | Must match the blackboard used by the behavior tree |
| `PrioritizeDrivingKey` | Blackboard key that decides whether the AI should prefer staying on the ground | Useful for multi-mode vehicles and airport logic |

## BTService_GroundPathPlanner

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `TargetLocationKey` | Destination key | Keep matched to the shipped blackboard |  | Blackboard binding |
| `LookAheadDistanceKey` | Look-ahead distance key | Keep matched to the shipped blackboard |  | Blackboard binding |
| `LookAheadLocationKey` | Look-ahead position key | Keep matched to the shipped blackboard |  | Blackboard binding |
| `DesiredSpeedKey` | Desired speed key | Keep matched to the shipped blackboard |  | Blackboard binding |
| `BaseLookAheadDistance` | Base forward planning distance | Steering reacts too late | Steering looks too far ahead | Ground feel |
| `LookAheadTimeSeconds` | Time-based extension of look-ahead | Faster vehicles need more preview | Paths feel too broad | Ground feel |
| `CornerCheckDistance` | Distance used to inspect upcoming corners | The AI sees turns too late | The AI slows too early | Corner planning |
| `SharpTurnThreshold` | How sharp a turn must be before special slowdown behavior applies | More turns should count as sharp | Only very hard turns should count | Corner classification |
| `MinCorneringSpeed` | Lowest desired speed in corners | Corners should still carry more speed | Corners should be taken more cautiously | Ground speed shaping |
| `DefaultMaxSpeed` | Fallback desired speed | Ground AI is too slow without other speed input | Ground AI is too fast by default | Fallback only |
| `BrakingStartDistance` | Distance to begin slowing for a destination | The AI brakes too late | The AI brakes too early | Arrival behavior |
| `bDrawDebugPath` | Draws debug path visuals | Use when diagnosing routes | Turn off after tuning | Debug only |

## BTService_SplineRoadPlanner

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `TargetLocationKey` | Destination key | Keep matched to the blackboard |  | Blackboard binding |
| `LookAheadDistanceKey` | Look-ahead distance key | Keep matched to the blackboard |  | Blackboard binding |
| `LookAheadLocationKey` | Look-ahead point key | Keep matched to the blackboard |  | Blackboard binding |
| `DesiredSpeedKey` | Desired speed key | Keep matched to the blackboard |  | Blackboard binding |
| `HasJunctionReservationKey` | Junction clearance state key | Keep matched to the blackboard |  | Traffic AI binding |
| `TrafficMustYieldKey` | Yield state key | Keep matched to the blackboard |  | Traffic AI binding |
| `OnRoadQueryRadius` | Distance used to find a road lane | The AI struggles to detect nearby lanes | Lane queries snap too broadly | Road reacquire control |
| `LaneDriftReQueryRadius` | Requery radius when drifting away from a lane | Lane reacquire happens too late | Reacquire happens too often | Recovery control |
| `DepartureToleranceCM` | Allowed lane departure before the system reacts | Slight drift should be tolerated more | The AI should react sooner when leaving a lane | Lane discipline |
| `LaneTransitionThresholdCM` | Distance before switching lane segments | Lane transitions happen too late | Lane transitions happen too early | Route smoothness |
| `MaxRouteLanes` | Max number of route lanes planned ahead | Routes should look farther ahead | Planning should stay shorter and simpler | Route complexity |
| `bWanderMode` | Lets the planner roam instead of heading to a fixed target | You want ambient wandering traffic | You want strict destination travel | Useful for roaming traffic |
| `BaseLookAheadDistance` | Base road preview distance | Steering reacts too late | Steering is too broad | Ground feel |
| `CornerCheckDistance` | Distance used to inspect corners | Turns are detected too late | Turns are treated too early | Corner planning |
| `SharpTurnThreshold` | Corner sharpness threshold | More bends should count as sharp | Only harder turns should count | Corner classification |
| `MinCorneringSpeed` | Minimum speed through corners | Corners should be faster | Corners should be more cautious | Speed shaping |
| `DefaultMaxSpeed` | Fallback road speed | Traffic should move faster by default | Traffic should move slower by default | Fallback only |
| `CornerPreviewDistanceCM` | Preview distance for corner behavior | The AI needs more room to set up corners | The AI slows too early | Corner planning depth |
| `JunctionApproachDistanceCM` | Distance at which junction logic starts | Junction reaction begins too late | Junction reaction begins too early | Major traffic behavior control |
| `JunctionTTCThresholdSec` | Time-to-collision threshold for junction caution | The AI should yield earlier | The AI should commit more readily | Intersection safety |
| `JunctionApproachSpeedCMS` | Speed target when approaching a junction | Approaches are too fast | Approaches are too hesitant | Junction pacing |
| `MaxLateralAccelG` | Maximum lateral acceleration the planner allows | The AI should corner faster | The AI should corner more safely | Turning realism |
| `MaxBrakingDecelCMSS` | Strongest allowed braking deceleration | The AI brakes too weakly | The AI brakes too harshly | Arrival and cornering |
| `MaxCornerSpeedCMS` | Highest speed allowed in corner calculations | Corners should support higher speed | Cornering should stay more conservative | Corner cap |
| `MinCornerSpeedCMS` | Lowest speed allowed in corner calculations | Corners should never get too slow | Corners should be able to slow more | Corner floor |
| `bEnableOvertaking` | Enables overtaking logic | Traffic should pass slower vehicles | Traffic should stay in lane | Scene complexity control |
| `OvertakeTriggerGapCM` | Gap that triggers overtake consideration | Overtakes should start sooner | Overtakes should happen less often | Overtake aggressiveness |
| `OvertakeSpeedDiffCMS` | Speed difference needed to consider passing | Smaller speed differences should trigger overtakes | Only obvious slowdowns should trigger overtakes | Overtake sensitivity |
| `OvertakeLateralOffsetCM` | Side offset used while passing | Passing needs more clearance | Passing drifts too far out | Overtake path width |
| `OvertakeBlendSpeedCMPerSec` | Speed of moving into and out of the pass offset | Lane change feels too slow | Lane change feels too abrupt | Presentation and safety |
| `OvertakeCooldownDuration` | Delay before another overtake is allowed | Traffic overpasses too often | Traffic should be freer to pass again sooner | Behavior pacing |
| `OncomingMinGapCM` | Minimum oncoming gap required to overtake | Overtakes should be more cautious | Overtakes should happen with less empty road | Safety control |
| `OncomingMinTimeSec` | Minimum safe oncoming time for overtakes | Overtakes should be more cautious | Overtakes are too restricted | Safety control |
| `OvertakePassMarginCM` | Extra margin after passing the leader | Merge-back happens too tightly | Traffic takes too long to merge back | Safety and smoothness |
| `OncomingUrgentMergeSec` | Time threshold for urgent merge-back | Vehicles should escape oncoming traffic sooner | Vehicles should stay in the pass longer | Safety control |
| `bDrawDebugLane` | Draws lane debug | Use while diagnosing lane selection | Turn off after tuning | Debug only |
| `bDrawDebugPath` | Draws path debug | Use while diagnosing routing | Turn off after tuning | Debug only |
| `bDrawDebugOvertake` | Draws overtake debug | Use while diagnosing passing logic | Turn off after tuning | Debug only |

## BTService_TrafficFollowing

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `DesiredSpeedKey` | Desired speed key | Keep matched to the blackboard |  | Blackboard binding |
| `TrafficSpeedLimitKey` | Speed limit key | Keep matched to the blackboard |  | Blackboard binding |
| `TrafficMustYieldKey` | Yield key | Keep matched to the blackboard |  | Blackboard binding |
| `HasJunctionReservationKey` | Junction reservation key | Keep matched to the blackboard |  | Blackboard binding |
| `LookAheadCM` | Distance used to look for leaders | The AI reacts too late to traffic ahead | The AI reacts too early | Main following preview distance |
| `CorridorHalfWidthCM` | Width of the detection corridor | Adjacent traffic should still count | Nearby traffic is being confused too often | Traffic sensing width |
| `SameDirDotThreshold` | How closely another vehicle must match direction to count as same-lane traffic | More loosely aligned traffic should count | Only clearly same-direction vehicles should count | Detection filter |
| `MinJamGapCM` | Minimum gap in jam conditions | Traffic should leave larger stop gaps | Traffic should pack tighter | Jam spacing |
| `TimeHeadwaySec` | Time-based following distance | Traffic should be more cautious | Traffic should feel denser | Major following control |
| `AccelCMS2` | Desired acceleration rate | Traffic should recover speed faster | Traffic should accelerate more gently | Flow pacing |
| `DecelCMS2` | Desired braking rate | Traffic should stop more firmly | Traffic should brake more gently | Braking feel |
| `BrakeReactionTimeSec` | Simulated reaction delay | Traffic should react earlier | Traffic should feel more human and delayed | Safety vs realism |
| `ConservativeStopDecelScale` | Extra caution near full stop | Stopping should feel safer | Stopping is too hesitant | Queue behavior |
| `YieldQueueExtraBufferCM` | Extra spacing while yielding in queues | Yielding vehicles stop too close | Queues become too stretched | Helpful at busy intersections |
| `bAutoDetectVehicleLength` | Automatically measures vehicle length | You want simpler setup | You want to force a manual length | Good default for varied traffic fleets |
| `VehicleLengthCM` | Manual length used when auto detection is off | The vehicle should reserve more road space | The vehicle should reserve less road space | Manual override |
| `DefaultSpeedLimitKPH` | Fallback speed limit | Roads without speed data should move faster | Roads without speed data should move slower | Fallback only |
| `JunctionCheckRadiusCM` | Radius used for junction conflict checks | The AI should watch a larger junction area | Junction checks are too broad | Intersection sensing |
| `JunctionCheckOffsetCM` | Forward offset for junction checks | The AI should look farther ahead into the junction | Checks begin too early | Intersection sensing |
| `JunctionTTCThresholdSec` | Time-to-collision threshold at junctions | The AI should yield earlier | The AI should commit sooner | Intersection safety |
| `bEnableSpeedNoise` | Adds small speed variation | Traffic should feel less robotic | Traffic should be more uniform | Ambient realism control |
| `SpeedNoiseKPH` | Size of the random speed variation | Variation is too subtle | Variation is too strong | Ambient realism control |
| `SpeedNoiseIntervalSec` | How often speed noise changes | Variation should change less often | Variation should change more often | Ambient realism control |
| `bDrawDebug` | Draws follow debug | Use while diagnosing traffic following | Turn off after tuning | Debug only |

## BTService_AirPathPlanner

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `ProbeDistance` | Obstacle probe reach | The AI sees obstacles too late | The AI overreacts to distant obstacles | Air obstacle detection |
| `ProbeRadius` | Width of obstacle probes | Thin obstacles are being missed | Too many objects are being treated as blockers | Detection width |
| `LookAheadDistance` | Forward planning distance in the air | Aircraft react too late | Aircraft plan too broadly | Air route preview |
| `AvoidanceWeight` | Strength of avoidance steering | The AI does not avoid strongly enough | Avoidance pushes routes too far off course | Obstacle reaction force |
| `GoOverThreshold` | Obstacle size threshold for choosing to go over | More obstacles should be climbed over | More obstacles should be routed around instead | Vertical vs lateral avoidance choice |
| `ObstacleClearanceMargin` | Extra space kept around obstacles | Aircraft pass too close to objects | Avoidance routes become too wide | Safety margin |
| `AvoidanceCommitTime` | How long the AI sticks to one avoidance choice | The AI changes its mind too often | The AI stays committed too long | Stability vs flexibility |
| `AIVehicleSeparationRadius` | Air-to-air avoidance radius | Aircraft fly too close together | Separation keeps traffic too far apart | Multiplayer or ambient-aircraft spacing |
| `SeparationWeight` | Strength of air-to-air separation | Aircraft need stronger spacing | Separation pulls paths too much | Balances obstacle vs traffic avoidance |
| `bDrawDebug` | Draws air-path debug | Use while diagnosing air AI | Turn off after tuning | Debug only |

## BTTask_SetRandomTrafficTarget

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `TargetLocationKey` | Blackboard key for the generated target | Keep matched to the blackboard |  | Blackboard binding |
| `TargetDistanceCM` | Distance to the random target | Traffic should roam farther | Traffic should stay more local | Roaming range |
| `RandomAngleHalfConeDeg` | Random direction spread | Traffic should pick a wider variety of directions | Traffic should stay more forward-focused | Higher values add more randomness |
| `TargetZOffsetCM` | Vertical offset of the generated target | Targets should sit higher or lower | Vertical offset is causing unwanted destinations | Useful in non-flat spaces |

## BTTask_ModularGroundDrive

| Setting | What it controls | Increase when | Decrease when | Notes |
| --- | --- | --- | --- | --- |
| `LookAheadLocationKey` | Blackboard key for the steering target | Keep matched to the blackboard |  | Blackboard binding |
| `TargetLocationKey` | Blackboard key for the destination | Keep matched to the blackboard |  | Blackboard binding |
| `TrafficSpeedLimitKey` | Blackboard speed limit key | Keep matched to the blackboard |  | Blackboard binding |
| `TrafficMustYieldKey` | Blackboard yield key | Keep matched to the blackboard |  | Blackboard binding |
| `DesiredSpeedKey` | Blackboard desired speed key | Keep matched to the blackboard |  | Blackboard binding |
| `TrafficManagerOvertakeTorqueBoost` | Extra torque boost while overtaking | Passing takes too long | Overtake bursts feel too strong | Overtake aid |
| `SteeringInterpSpeed` | How quickly steering changes are applied | AI steering reacts too slowly | Steering snaps too hard | Main steering smoothness control |
| `SteeringGainDivisor` | Scaling that softens steering demand | Steering is too weak | Steering is too aggressive | Core turning feel |
| `HighSpeedSteeringKPH` | Speed where high-speed steering logic becomes important | High-speed steering reduction should start later | High-speed steering should calm down sooner | Speed-based steering control |
| `HighSpeedSteeringGainScale` | Additional steering reduction or scaling at speed | High-speed stability needs more help | High-speed steering feels too numb | Important for fast AI vehicles |
| `YawRateDampingGain` | Damps steering based on yaw rate | The AI oscillates in turns | The AI feels too restrained | Helps stabilize turning |
| `CorneringSpeedMinKPH` | Minimum speed the driver tries to keep in corners | AI corners too slowly | AI corners too fast | General driving pace |
| `CorneringFullAngleDeg` | Turn angle at which full corner slowdown is applied | More corners should count as major turns | Only hard bends should get strong slowdown | Corner sensitivity |
| `YawErrorFilterRate` | Smoothing for heading error | Steering corrections are jittery | Steering reacts too slowly | Balance smoothness and responsiveness |
| `ThrottleBrakeRampKPH` | Speed range used to blend throttle and brake behavior | Speed transitions feel too abrupt | Throttle and brake overlap too softly | Helps clean control handoff |
| `AvoidanceSweepRadius` | Width of obstacle sweeps | AI should notice more surrounding obstacles | Sweeps feel too broad and overcautious | Obstacle sensing |
| `MinFollowGapCM` | Minimum following gap | AI should stop farther back | AI should queue more tightly | Local obstacle following |
| `FollowTimeHeadwaySec` | Time-based follow distance | Following should be more cautious | Following should be tighter | Traffic spacing |
| `SwarmBrakeReactionTimeSec` | Reaction delay during swarm-like avoidance | AI should brake earlier | AI should feel less instantly reactive | Crowd handling |
| `SwarmBrakeDecelCMS2` | Braking force during avoidance | AI should stop more strongly | AI should brake more gently | Obstacle avoidance strength |
| `SwarmCriticalTTCSec` | Critical time-to-collision threshold | AI should react earlier to danger | AI should commit a bit longer before braking | Safety control |
| `RearObstacleClearanceCM` | Clearance wanted behind the vehicle | Reverse maneuvers need more room | Reverse maneuvers are too limited | Recovery behavior |
| `QueueRecognitionHoldSec` | Time needed to decide a queue is real | AI should detect queues faster | AI should avoid reacting to brief slowdowns | Queue stability |
| `MaxReverseSpeedKPH` | Reverse speed cap during recovery | Recoveries need to back up faster | Reverse behavior is too aggressive | Recovery pacing |
| `HeadOnYieldTriggerSec` | Time-to-conflict threshold for head-on yielding | AI should yield sooner | AI should wait longer before yielding | Head-on safety |
| `HeadOnYieldHoldSec` | How long yield behavior is held | Yielding ends too quickly | Yielding holds too long | Head-on flow control |
| `HeadOnMinYieldDistanceCM` | Minimum spacing kept when yielding head-on | Vehicles should stop farther apart | Vehicles can stop closer | Head-on spacing |
| `HeadOnYieldDistanceScale` | Scaling for yield distance | More head-on clearance is needed | Head-on clearance is too conservative | Head-on spacing |
| `HeadOnYieldPassingBufferCM` | Extra spacing while passing after yield | Vehicles pass too tightly | Vehicles wait too long to pass | Head-on cleanup |
| `HeadOnMaxWaitSec` | Max head-on wait time | Vehicles should tolerate longer waits | Vehicles should resolve standoffs sooner | Deadlock control |
| `HeadOnHolderCreepSec` | Time before the holding vehicle creeps forward | Held vehicles should wait longer before creeping | Held vehicles should start creeping sooner | Head-on polish |
| `SpeedPersonalityScale` | Overall speed personality for this task | AI should drive more boldly | AI should drive more conservatively | Quick personality control |
| `ThrottleInterpSpeed` | How quickly throttle changes are applied | AI acceleration reacts too slowly | Throttle changes feel too abrupt | Acceleration smoothness |

## Troubleshooting

### I cannot see the plugin assets

- Enable **Show Plugin Content** in the Content Browser.
- Restart the editor after enabling the plugin.
- Verify the plugin is enabled in **Edit > Plugins**.

### The vehicle spawns but does not respond to controls

- Check that the vehicle has a valid `ModularVehicleCore`.
- Verify `DefaultMappingContext` is assigned.
- Verify the expected input actions are assigned in the vehicle core.
- Make sure the pawn is actually possessed by the player.

### Traffic is not moving

- Check that your lane or road setup exists and is valid.
- Make sure lane tags are consistent across roads, traffic manager, spawner, and traffic lights.
- Reduce the traffic setup to one manager and one spawner first.
- Start with a small spawn count and scale upward only after movement works.

### Aircraft AI is not taking off or landing correctly

- Verify the runway orientation is correct.
- Make sure the aircraft has the airplane component plus AI component configured.
- Start with manual takeoff and landing before tuning AI.
- Use the airplane debug path options when tuning route behavior.

### Vehicles are too unstable

- Lower steering sensitivity first.
- Check the center of mass offset.
- Revisit suspension sag and damping.
- For airplanes, reduce control sensitivity before changing path planning.
- For rotorcraft, tune lift and spool-up before aggressive response values.

## Setup checklist

- Plugin enabled
- Required built-in plugins enabled
- Editor restarted
- Plugin content visible
- `L_VehiclesDemo` opens correctly
- Included vehicles appear in `Content/ModularVehicles`
- `IMC_VehicleControls` and input actions are visible
- `BP_VehiclesAiController`, `BB_Vehicles`, and `BT_Vehicles` are visible
- `BP_Runway` and `BP_TrafficSpawner` are visible
- Lane tags are consistent across your traffic scene
- You duplicated a working vehicle before heavy customization

## Final recommendation

For the smoothest setup, begin with `L_VehiclesDemo`, duplicate the closest included vehicle blueprint, and only then start replacing meshes and tuning behavior. That approach lets you spend your time shaping the vehicle you want instead of rebuilding working setup decisions from scratch.
