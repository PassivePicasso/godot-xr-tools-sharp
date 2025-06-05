# Godot XR Tools C# Migration Research Notes

## Project Overview
Godot XR Tools is a comprehensive XR (VR/AR) toolkit for Godot Engine that provides:
- Player movement and physics system
- Hand tracking and interaction system
- Various locomotion methods (teleport, direct movement, climbing, etc.)
- Interactable objects and user interface components
- Audio and visual effects for XR experiences

## Current Project Structure

### Core Architecture
The project follows a modular architecture with the following key components:

#### 1. Core Classes Hierarchy
```
Node
├── XRToolsMovementProvider (abstract base for movement)
├── XRToolsSceneBase (base for scene management)
├── XRHelpers (static utility functions)
└── XRTools (main utility and settings class)

Node3D
├── XRToolsHand (hand animation and tracking)
├── XRToolsClimbable (objects that can be climbed)
├── XRToolsInteractableHandleDriven (base for handle-driven interactions)
└── XRToolsGrabPoint (defines grab positions)

CharacterBody3D
└── XRToolsPlayerBody (main player physics body)

RigidBody3D
└── XRToolsPickable (objects that can be picked up)

AnimatableBody3D
└── XRToolsForceBody (physics body that can push other bodies)

Area3D
└── XRToolsTeleportArea (teleportation destination)

Resource
└── XRToolsGroundPhysicsSettings (physics configuration)
```

#### 2. Movement System
- **Base Class**: `XRToolsMovementProvider`
- **Core Body**: `XRToolsPlayerBody` (CharacterBody3D)
- **Movement Types**:
  - Direct Movement (`XRToolsMovementDirect`)
  - Teleportation (`XRToolsMovementTeleport`)
  - Climbing (`XRToolsMovementClimb`) 
  - Flight (`XRToolsMovementFlight`)
  - Jumping (`XRToolsMovementJump`)
  - World Grabbing (`XRToolsMovementWorldGrab`)
  - Grappling (`XRToolsMovementGrapple`)
  - And many more specialized movement types

#### 3. Interaction System
- **Pickup System**: `XRToolsFunctionPickup` works with `XRToolsPickable` objects
- **Pointer System**: `XRToolsFunctionPointer` for UI interaction
- **Hand System**: `XRToolsHand` with pose detection and physics hands
- **Interactables**: Various UI controls (sliders, buttons, joysticks, etc.)

#### 4. Key Patterns Observed
1. **Class Registration**: All classes use `class_name` for registration
2. **Type Checking**: Custom `is_xr_class()` method for type checking
3. **Node Finding**: Extensive use of helper functions to find related nodes
4. **Signal-Based Communication**: Heavy use of signals for loose coupling
5. **Physics Integration**: Deep integration with Godot's physics system
6. **Resource-Based Configuration**: Settings stored as Resources

### Directory Structure Analysis

#### `/addons/godot-xr-tools/`
- **`functions/`**: Movement providers and function nodes
- **`player/`**: Player body and related systems
- **`hands/`**: Hand tracking and interaction
- **`interactables/`**: UI controls and interactive objects
- **`objects/`**: Reusable objects (pickable, climbable, etc.)
- **`overrides/`**: Physics and behavior overrides
- **`misc/`**: Utility classes and helpers
- **`staging/`**: Scene management system
- **`desktop-support/`**: Desktop (non-VR) compatibility
- **`effects/`**: Visual and audio effects
- **`assets/`**: Meshes, materials, and other assets

## C# Migration Considerations

### 1. Language-Specific Adaptations

#### Signal Handling
- GDScript: `signal_name.connect(callable)`
- C#: Use `[Signal]` attribute and events pattern

#### Node Access Patterns
- GDScript: `@onready var node := get_node("path") as Type`
- C#: `public Type Node { get; private set; }`

#### Export Variables
- GDScript: `@export var property : Type`
- C#: `[Export] public Type Property { get; set; }`

#### Class Registration
- GDScript: `class_name ClassName`
- C#: No direct equivalent, use namespace organization

### 2. Type System Differences
- GDScript has dynamic typing with optional static typing
- C# requires full static typing
- Need to handle Variant types carefully
- Generic types need explicit specification

### 3. Resource System
- GDScript Resources work well with C#
- Configuration classes should inherit from Resource
- Custom Resource types need `[GlobalClass]` attribute

### 4. Physics Integration
- Direct physics API calls need C# equivalents
- CharacterBody3D, RigidBody3D work similarly
- Physics queries need proper C# syntax

## Key Dependencies

1. **Godot XR/OpenXR Plugin**: Required for XR functionality
2. **Godot Physics**: Core physics system integration
3. **Godot Audio**: 3D audio positioning and effects
4. **Godot UI**: Control nodes for interfaces
5. **Godot Animation**: Hand poses and transitions

## Migration Challenges

### 1. Dynamic Code Patterns
- Extensive use of `get_node()` with dynamic paths
- String-based action/input mapping
- Runtime type checking with `is_xr_class()`

### 2. Editor Integration
- Tool scripts (`@tool`) for editor functionality
- Custom property editors and inspectors
- Scene instantiation and modification

### 3. Complex Node Hierarchies
- Deep node tree traversal
- Parent-child relationship management
- Cross-node communication patterns

### 4. Platform-Specific Code
- Desktop support vs VR support
- Input handling differences
- Performance optimizations

## Namespace Organization Strategy

```
XRTools
├── Core (XRTools, XRHelpers, base classes)
├── Movement (all movement providers)
├── Player (PlayerBody, hands, etc.)
├── Interaction (pickup, pointer, interactables)
├── Objects (pickable, climbable, etc.)
├── Effects (audio, visual effects)
├── Physics (ground physics, force body)
├── Desktop (desktop support classes)
└── Staging (scene management)
```

## Key Files for Initial Migration Priority

1. **Core Infrastructure**:
   - `xr_tools.gd`
   - `misc/xr_helpers.gd`
   - `functions/movement_provider.gd`

2. **Player System**:
   - `player/player_body.gd`
   - `hands/hand.gd`

3. **Basic Movement**:
   - `functions/movement_direct.gd`
   - `functions/movement_teleport.gd`

4. **Basic Interaction**:
   - `functions/function_pickup.gd`
   - `objects/pickable.gd`

## Testing Strategy

1. **Unit Tests**: Test core utility functions and mathematical operations
2. **Integration Tests**: Test node relationships and signal connections
3. **Scene Tests**: Test complete scenes with multiple systems
4. **Platform Tests**: Test desktop and VR modes
5. **Performance Tests**: Ensure C# performance meets VR requirements
