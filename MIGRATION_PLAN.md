# Godot XR Tools C# Migration Plan

## Overview
This document outlines the structured approach to migrating Godot XR Tools from GDScript to C#. The migration is organized into phases that can be executed in parallel while maintaining dependencies and compatibility.

## Migration Phases

### Phase 1: Core Infrastructure & Utilities
**Goal**: Establish the foundational C# infrastructure that other systems depend on.

#### Task 1.1: Core Utilities and Helpers
**Files to Create**:
- `addons/godot-xr-tools-sharp/Core/XRTools.cs`
- `addons/godot-xr-tools-sharp/Core/XRHelpers.cs`
- `addons/godot-xr-tools-sharp/Core/XRToolsUserSettings.cs`

**Dependencies**: None (foundational)
**Compatibility Requirements**: 
- Must maintain same static method signatures as GDScript versions
- Settings and configuration compatibility with existing user settings
- Preserve all utility functions for node finding and XR detection

**Deliverables**:
- [ ] Core XRTools static class with project settings management
- [ ] XRHelpers static class with node finding utilities
- [ ] User settings management with existing compatibility
- [ ] Unit tests for all utility functions

#### Task 1.2: Plugin Infrastructure
**Files to Create**:
- `addons/godot-xr-tools-sharp/Plugin.cs`
- `addons/godot-xr-tools-sharp/plugin.cfg` (updated for C# support)

**Dependencies**: Task 1.1
**Compatibility Requirements**:
- Editor plugin functionality must match GDScript version
- Project settings registration and management
- OpenXR configuration helpers

**Deliverables**:
- [ ] EditorPlugin implementation with menu integration
- [ ] Project settings definition and registration
- [ ] OpenXR enablement and physics layer configuration

### Phase 2: Physics and Resource Systems
**Goal**: Implement the physics configuration and resource systems that movement and interaction depend on.

#### Task 2.1: Ground Physics System
**Files to Create**:
- `addons/godot-xr-tools-sharp/Physics/GroundPhysicsSettings.cs`
- `addons/godot-xr-tools-sharp/Physics/GroundPhysics.cs`

**Dependencies**: Task 1.1
**Compatibility Requirements**:
- Resource serialization compatibility with existing .tres files
- Same physics calculation methods and properties
- Enum flags system preservation

**Deliverables**:
- [ ] GroundPhysicsSettings Resource class with all properties
- [ ] GroundPhysics node for attaching to ground objects
- [ ] Static helper methods for physics calculations
- [ ] Compatibility with existing physics resource files

#### Task 2.2: Force Body and Physics Utilities
**Files to Create**:
- `addons/godot-xr-tools-sharp/Physics/ForceBody.cs`
- `addons/godot-xr-tools-sharp/Physics/VelocityAverager.cs`
- `addons/godot-xr-tools-sharp/Physics/VelocityAveragerLinear.cs`

**Dependencies**: Task 1.1
**Compatibility Requirements**:
- Same physics behavior for body pushing and collision
- Compatible velocity averaging for smooth movement
- Preserved mathematical precision for VR applications

**Deliverables**:
- [ ] ForceBody class extending AnimatableBody3D
- [ ] Velocity averaging utilities for movement smoothing
- [ ] Physics query and collision handling systems

### Phase 3: Movement Provider Base System
**Goal**: Create the base movement system that all locomotion methods extend from.

#### Task 3.1: Movement Provider Base Class
**Files to Create**:
- `addons/godot-xr-tools-sharp/Movement/MovementProvider.cs`

**Dependencies**: Tasks 1.1, 2.1
**Compatibility Requirements**:
- Same movement provider ordering system
- Compatible with existing movement provider group system
- Preserve enabled/disabled state management

**Deliverables**:
- [ ] Abstract MovementProvider base class
- [ ] Movement provider ordering and execution system
- [ ] PlayerBody node creation and management
- [ ] Configuration warning system

#### Task 3.2: Player Body Core
**Files to Create**:
- `addons/godot-xr-tools-sharp/Player/PlayerBody.cs`

**Dependencies**: Tasks 1.1, 2.1, 3.1
**Compatibility Requirements**:
- Same physics behavior for player movement
- Compatible collision detection and ground sensing
- Preserve all signals and their parameters
- Maintain height calibration and override system

**Deliverables**:
- [ ] PlayerBody class extending CharacterBody3D
- [ ] Ground detection and physics integration
- [ ] Height calibration and override system
- [ ] Movement provider integration and execution order
- [ ] All player body signals (jumped, teleported, moved, bounced)

### Phase 4: Basic Interaction System
**Goal**: Implement the core interaction system for picking up objects and basic hand functionality.

#### Task 4.1: Basic Hand System
**Files to Create**:
- `addons/godot-xr-tools-sharp/Hands/Hand.cs`
- `addons/godot-xr-tools-sharp/Hands/HandPoseSettings.cs`

**Dependencies**: Tasks 1.1, 1.2
**Compatibility Requirements**:
- Same hand scaling and pose animation system
- Compatible with existing hand models and animations
- Preserve pose override and blend tree integration

**Deliverables**:
- [ ] Hand class with pose animation and scaling
- [ ] Hand pose settings resource class
- [ ] Target override system for hand positioning
- [ ] World scale change handling

#### Task 4.2: Pickable Objects
**Files to Create**:
- `addons/godot-xr-tools-sharp/Objects/Pickable.cs`
- `addons/godot-xr-tools-sharp/Objects/GrabPoint.cs`
- `addons/godot-xr-tools-sharp/Objects/GrabPointHand.cs`
- `addons/godot-xr-tools-sharp/Objects/GrabPointSnap.cs`

**Dependencies**: Tasks 1.1, 4.1
**Compatibility Requirements**:
- Same pickup behavior and physics freeze/unfreeze
- Compatible with existing grab point configurations
- Preserve all pickup-related signals

**Deliverables**:
- [ ] Pickable class extending RigidBody3D
- [ ] Grab point system with hand and snap zone support
- [ ] Ranged pickup methods (snap/lerp)
- [ ] Highlight system integration

#### Task 4.3: Pickup Function
**Files to Create**:
- `addons/godot-xr-tools-sharp/Functions/FunctionPickup.cs`

**Dependencies**: Tasks 1.1, 3.1, 4.1, 4.2
**Compatibility Requirements**:
- Same pickup detection and grab distance calculations
- Compatible controller input handling
- Preserve pickup collision masks and range detection

**Deliverables**:
- [ ] FunctionPickup class for controller attachment
- [ ] Pickup detection with collision mask support
- [ ] Grab distance and range calculations
- [ ] Integration with Pickable objects and GrabPoints

### Phase 5: Basic Movement Providers
**Goal**: Implement essential movement methods for basic locomotion.

#### Task 5.1: Direct Movement
**Files to Create**:
- `addons/godot-xr-tools-sharp/Movement/MovementDirect.cs`

**Dependencies**: Tasks 1.1, 3.1, 3.2
**Compatibility Requirements**:
- Same thumbstick/trackpad input handling
- Compatible speed and strafe controls
- Preserve ground control velocity system

**Deliverables**:
- [ ] Direct movement with controller input
- [ ] Speed and strafe configuration
- [ ] Ground control velocity integration

#### Task 5.2: Turn Movement
**Files to Create**:
- `addons/godot-xr-tools-sharp/Movement/MovementTurn.cs`

**Dependencies**: Tasks 1.1, 3.1, 3.2
**Compatibility Requirements**:
- Same turn step and smooth turn options
- Compatible input action handling
- Preserve turn speed configurations

**Deliverables**:
- [ ] Snap and smooth turning systems
- [ ] Turn step configuration and accumulation
- [ ] Input action integration

#### Task 5.3: Jump Movement
**Files to Create**:
- `addons/godot-xr-tools-sharp/Movement/MovementJump.cs`

**Dependencies**: Tasks 1.1, 3.1, 3.2
**Compatibility Requirements**:
- Same jump velocity and ground detection
- Compatible button input handling
- Preserve jump height calculations

**Deliverables**:
- [ ] Jump movement with button trigger
- [ ] Ground state validation for jumping
- [ ] Jump velocity application

### Phase 6: Teleportation System
**Goal**: Implement the teleportation locomotion system.

#### Task 6.1: Function Teleport
**Files to Create**:
- `addons/godot-xr-tools-sharp/Functions/FunctionTeleport.cs`

**Dependencies**: Tasks 1.1, 3.1, 3.2
**Compatibility Requirements**:
- Same arc calculation and collision detection
- Compatible with existing teleport areas and valid layers
- Preserve teleport validation and visual feedback

**Deliverables**:
- [ ] Teleport function with arc casting
- [ ] Collision detection and valid surface checking
- [ ] Integration with teleport areas and snap zones

#### Task 6.2: Teleport Area
**Files to Create**:
- `addons/godot-xr-tools-sharp/Objects/TeleportArea.cs`

**Dependencies**: Tasks 1.1, 3.2
**Compatibility Requirements**:
- Same area-based teleportation behavior
- Compatible with player body teleport system

**Deliverables**:
- [ ] TeleportArea class extending Area3D
- [ ] Player body detection and teleportation
- [ ] Target position and orientation handling

### Phase 7: Advanced Movement Systems
**Goal**: Implement complex movement methods like climbing and flight.

#### Task 7.1: Climbing System
**Files to Create**:
- `addons/godot-xr-tools-sharp/Movement/MovementClimb.cs`
- `addons/godot-xr-tools-sharp/Objects/Climbable.cs`

**Dependencies**: Tasks 1.1, 3.1, 3.2, 4.2, 4.3
**Compatibility Requirements**:
- Same climbing detection and grab mechanics
- Compatible with climbable object setup
- Preserve velocity averaging and fling mechanics

**Deliverables**:
- [ ] Climbing movement with dual-hand support
- [ ] Climbable object class with grab point integration
- [ ] Velocity averaging for fling mechanics
- [ ] Integration with pickup system for climb detection

#### Task 7.2: Flight Movement
**Files to Create**:
- `addons/godot-xr-tools-sharp/Movement/MovementFlight.cs`

**Dependencies**: Tasks 1.1, 3.1, 3.2
**Compatibility Requirements**:
- Same flight control schemes (controller/camera based)
- Compatible flight physics (acceleration, drag, guidance)
- Preserve exclusive movement behavior

**Deliverables**:
- [ ] Flight movement with multiple control schemes
- [ ] Flight physics with acceleration and drag
- [ ] Guidance system for realistic flight behavior

#### Task 7.3: Advanced Movement Providers
**Files to Create**:
- `addons/godot-xr-tools-sharp/Movement/MovementGrapple.cs`
- `addons/godot-xr-tools-sharp/Movement/MovementWorldGrab.cs`
- `addons/godot-xr-tools-sharp/Movement/MovementGlide.cs`
- `addons/godot-xr-tools-sharp/Movement/MovementWallWalk.cs`

**Dependencies**: Tasks 1.1, 3.1, 3.2, 4.3
**Compatibility Requirements**:
- Each must maintain specific movement behaviors
- Compatible with existing controller input schemes
- Preserve physics calculations and integrations

**Deliverables**:
- [ ] Grappling hook movement system
- [ ] World grab movement for pulling environment
- [ ] Gliding movement with physics simulation
- [ ] Wall walking with gravity manipulation

### Phase 8: Interaction and UI Systems
**Goal**: Implement pointer system and interactable UI components.

#### Task 8.1: Pointer System
**Files to Create**:
- `addons/godot-xr-tools-sharp/Functions/FunctionPointer.cs`
- `addons/godot-xr-tools-sharp/Functions/FunctionGazePointer.cs`

**Dependencies**: Tasks 1.1, 3.1
**Compatibility Requirements**:
- Same pointer collision detection and UI interaction
- Compatible with Godot's Control node system
- Preserve laser and target visual feedback

**Deliverables**:
- [ ] Pointer function with laser visualization
- [ ] Gaze pointer for head-based pointing
- [ ] UI interaction and click detection
- [ ] Visual feedback system

#### Task 8.2: Interactable Components
**Files to Create**:
- `addons/godot-xr-tools-sharp/Interactables/InteractableHandleDriven.cs`
- `addons/godot-xr-tools-sharp/Interactables/InteractableHandle.cs`
- `addons/godot-xr-tools-sharp/Interactables/InteractableSlider.cs`
- `addons/godot-xr-tools-sharp/Interactables/InteractableButton.cs`
- `addons/godot-xr-tools-sharp/Interactables/InteractableJoystick.cs`
- `addons/godot-xr-tools-sharp/Interactables/InteractableHinge.cs`

**Dependencies**: Tasks 1.1, 4.1, 4.2, 4.3
**Compatibility Requirements**:
- Same handle-driven interaction mechanics
- Compatible with existing interactable configurations
- Preserve constraint and movement calculations

**Deliverables**:
- [ ] Base handle-driven interactable system
- [ ] Individual interactable components (slider, button, etc.)
- [ ] Handle grab and manipulation system
- [ ] Constraint-based movement calculations

### Phase 9: Advanced Hand and Physics Systems
**Goal**: Complete the hand system with physics hands and advanced features.

#### Task 9.1: Physics Hands
**Files to Create**:
- `addons/godot-xr-tools-sharp/Hands/PhysicsHand.cs`
- `addons/godot-xr-tools-sharp/Hands/HandPhysicsBone.cs`
- `addons/godot-xr-tools-sharp/Hands/CollisionHand.cs`

**Dependencies**: Tasks 1.1, 4.1
**Compatibility Requirements**:
- Same physics bone behavior and collision detection
- Compatible with existing hand models and skeletons
- Preserve collision mask and group settings

**Deliverables**:
- [ ] Physics hand with bone collision detection
- [ ] Hand physics bone system for individual finger collisions
- [ ] Collision hand for force-based interactions
- [ ] Integration with hand scaling and pose systems

#### Task 9.2: Pose Detection System
**Files to Create**:
- `addons/godot-xr-tools-sharp/Functions/FunctionPoseDetector.cs`
- `addons/godot-xr-tools-sharp/Objects/HandPoseArea.cs`

**Dependencies**: Tasks 1.1, 4.1
**Compatibility Requirements**:
- Same pose detection and area-based triggering
- Compatible with existing pose configurations
- Preserve pose transition and priority systems

**Deliverables**:
- [ ] Pose detector function for hand pose changes
- [ ] Hand pose area for environment-based pose changes
- [ ] Pose priority and transition management

### Phase 10: Effects and Audio Systems
**Goal**: Implement visual and audio effects for enhanced XR experience.

#### Task 10.1: Audio Systems
**Files to Create**:
- `addons/godot-xr-tools-sharp/Audio/SurfaceAudioType.cs`
- `addons/godot-xr-tools-sharp/Movement/MovementFootstep.cs`
- `addons/godot-xr-tools-sharp/Effects/RumbleArea.cs`

**Dependencies**: Tasks 1.1, 3.1, 3.2
**Compatibility Requirements**:
- Same surface-based audio triggering
- Compatible with existing audio file formats and configurations
- Preserve spatial audio positioning

**Deliverables**:
- [ ] Surface audio type system for material-based sounds
- [ ] Footstep movement provider with audio integration
- [ ] Rumble effects for haptic feedback

#### Task 10.2: Visual Effects
**Files to Create**:
- `addons/godot-xr-tools-sharp/Player/Fade/FadeCollision.cs`
- `addons/godot-xr-tools-sharp/Effects/HighlightRing.cs`
- `addons/godot-xr-tools-sharp/Effects/WindArea.cs`

**Dependencies**: Tasks 1.1, 3.2
**Compatibility Requirements**:
- Same collision-based fading behavior
- Compatible visual highlight system
- Preserve wind effect physics

**Deliverables**:
- [ ] Collision-based fade system for comfort
- [ ] Highlight ring for object selection
- [ ] Wind area effects for immersion

### Phase 11: Desktop Support and Compatibility
**Goal**: Implement desktop (non-VR) compatibility features.

#### Task 11.1: Desktop Movement Providers
**Files to Create**:
- `addons/godot-xr-tools-sharp/Desktop/MovementDesktopDirect.cs`
- `addons/godot-xr-tools-sharp/Desktop/MovementDesktopTurn.cs`
- `addons/godot-xr-tools-sharp/Desktop/MovementDesktopJump.cs`
- `addons/godot-xr-tools-sharp/Desktop/MovementDesktopFlight.cs`

**Dependencies**: Tasks 1.1, 3.1, 3.2, Phase 5
**Compatibility Requirements**:
- Same keyboard/mouse input handling
- Compatible fallback behavior when VR is not active
- Preserve movement speed and control schemes

**Deliverables**:
- [ ] Desktop versions of core movement providers
- [ ] Keyboard and mouse input integration
- [ ] VR/Desktop mode detection and switching

#### Task 11.2: Desktop Support Utilities
**Files to Create**:
- `addons/godot-xr-tools-sharp/Desktop/ControllerHider.cs`
- `addons/godot-xr-tools-sharp/Desktop/MovementGravityZones.cs`

**Dependencies**: Tasks 1.1, 3.1
**Compatibility Requirements**:
- Same controller visibility management
- Compatible gravity zone behavior

**Deliverables**:
- [ ] Controller hiding for desktop mode
- [ ] Gravity zone system for advanced movement

### Phase 12: Scene Management and Staging
**Goal**: Complete the scene management system for demo and game integration.

#### Task 12.1: Scene Base System
**Files to Create**:
- `addons/godot-xr-tools-sharp/Staging/SceneBase.cs`
- `addons/godot-xr-tools-sharp/Staging/Staging.cs`

**Dependencies**: Tasks 1.1
**Compatibility Requirements**:
- Same scene transition and loading behavior
- Compatible with existing demo scenes
- Preserve user data passing between scenes

**Deliverables**:
- [ ] Base scene class for XR scenes
- [ ] Staging system for scene management
- [ ] Scene transition and data passing

#### Task 12.2: Demo Scene Integration
**Files to Create**:
- `addons/godot-xr-tools-sharp/Scenes/DemoSceneBase.cs`

**Dependencies**: Tasks 1.1, 12.1
**Compatibility Requirements**:
- Compatible with existing demo scene structure
- Preserve WebXR and input configuration handling

**Deliverables**:
- [ ] Demo scene base class
- [ ] WebXR compatibility layer
- [ ] Input configuration management

### Phase 13: Advanced Objects and Utilities
**Goal**: Complete remaining specialized objects and utility systems.

#### Task 13.1: Snap Zones and Advanced Objects
**Files to Create**:
- `addons/godot-xr-tools-sharp/Objects/SnapZone.cs`
- `addons/godot-xr-tools-sharp/Objects/Viewport2DIn3D.cs`
- `addons/godot-xr-tools-sharp/Objects/InteractableBody.cs`

**Dependencies**: Tasks 1.1, 4.2, 8.1
**Compatibility Requirements**:
- Same snap zone behavior and object filtering
- Compatible 2D viewport in 3D space interaction
- Preserve pointer event handling

**Deliverables**:
- [ ] Snap zone system for object placement
- [ ] 2D viewport integration in 3D space
- [ ] Interactable body base class

#### Task 13.2: Utility Objects
**Files to Create**:
- `addons/godot-xr-tools-sharp/Misc/HoldButton.cs`
- `addons/godot-xr-tools-sharp/Misc/MoveTo.cs`
- `addons/godot-xr-tools-sharp/Misc/VRCommonShaderCache.cs`

**Dependencies**: Tasks 1.1
**Compatibility Requirements**:
- Same utility behavior for UI and movement
- Compatible shader caching for performance
- Preserve hold button mechanics

**Deliverables**:
- [ ] Hold button utility for UI interactions
- [ ] MoveTo utility for object animation
- [ ] VR shader cache for performance optimization

## Implementation Guidelines

### Code Style and Structure
1. **Namespace Organization**: Follow the planned namespace structure
2. **File Organization**: Match GDScript file structure where possible
3. **Class Naming**: Maintain XRTools prefix for consistency
4. **Method Signatures**: Preserve public API compatibility
5. **Signal Patterns**: Use C# events following Godot conventions

### Testing Requirements
1. **Unit Tests**: Each task must include unit tests for core functionality
2. **Integration Tests**: Test interaction between systems
3. **Compatibility Tests**: Verify behavior matches GDScript version
4. **Performance Tests**: Ensure VR performance requirements are met

### Documentation Requirements
1. **XML Documentation**: All public APIs must have XML documentation
2. **Migration Notes**: Document any behavior changes or limitations
3. **Usage Examples**: Provide C# examples for complex systems
4. **Performance Notes**: Document any performance considerations

### Delivery Requirements
Each task must deliver:
- [ ] Complete C# implementation with full functionality
- [ ] Unit tests with minimum 80% code coverage
- [ ] Integration tests verifying compatibility
- [ ] XML documentation for all public APIs
- [ ] Migration notes documenting any changes
- [ ] Performance validation for VR scenarios

## Success Criteria
1. **Functional Parity**: All GDScript functionality available in C#
2. **Performance Parity**: C# version meets or exceeds GDScript performance
3. **Compatibility**: Existing scenes and configurations work with minimal changes
4. **Maintainability**: C# codebase is maintainable alongside GDScript version
5. **Documentation**: Complete documentation enables easy adoption

## Risk Mitigation
1. **Parallel Development**: Tasks within phases can be developed in parallel
2. **Incremental Testing**: Each phase includes comprehensive testing
3. **Rollback Strategy**: GDScript version remains functional throughout migration
4. **Performance Monitoring**: Continuous performance validation during development
5. **Community Feedback**: Regular feedback collection from users during migration
