# Safespaces

This is the development / prototyping environment for 'Safespaces', a 3D/VR
desktop environment for the Arcan display server. To get the full use out of
it, first make sure that you have an OpenHMD supported headset, a working
[Arcan](https://github.com/letoram/arcan) installation along with the vrbridge
tool all setup (see the main arcan repository for instructions and configuring
and installing).

Note that this is a highly experimental young project. Prolonged use is quite
likely unhealthy in a number of ways, eye strain guaranteed while debugging.
Tread carefully and try with non-VR device profiles first. Keep a vomit-bucket
nearby.

The project and related development is discussed in the #arcan IRC channel on
the freenode IRC network.

## Setting up

You can start simply with:

    arcan /path/to/safespaces

Or for specifying profiles other than those labeled as 'default' with:

    arcan /path/to/safespaces space=spacename device=devicename

The space profile match to the corresponding spacename (.lua) in the spaces/
subfolder which defines environment layout, visuals, workflow and input
response. The device profile controls how the rendering pipe will be
configured. There are a few default device presets to chose from:

* basic - takes whatever the arcan vr bridge provides as the default
* monoscopic - normal "3D" with mouse and keyboard
* psvr - waits for a PSVR HMD display to appear before activating
* simulated - works like monoscopic but outputs with a faked profile

The next section, configuration, goes into detail on how these can be extended
or complemented.

# Configuration

The config.lua file determines the reserved meta keys, reported client display
properties, font and so on. The configuration is further split up into the
(empty) default autorun.lua, the command-line controllable device profile and
the currently active 'Space'.

## Spaces

A space is simply a synchronized set of activated command paths that follow a
/path/to/command or /path/to/key=value for non-binary settings.  The full set
of possible such paths are covered in API.md, including the option to load
other spaces (with no protection against recursion, mind you). Every window
management option and interaction model will be provided in this way,
optionally via a mountable filesystem to make it easier to discover.

# Devices

The device profile determines rendering mode, target output and input devices,
arguments to the VR device bridge and possible overrides for properties such
as oversampling and distortion parameters, along with device- specific input
bindings. Note that any collisions between bindings defined for a device vs.
bindings defined for the global config will be biased in favor of the global
config and a warning printed to stdout for each collision.

## Troubleshooting

There are a number of moving parts that can go wrong here, depending on the
lower details of your system. For VR use, always make sure that the HMD itself
is working via OpenHMD and their simple text examples first. Then make sure
that your user actually has access to the input and output devices that you
wish to use.

The next big part (for VR use) is the presence of the 'arcan\_vr' binary. Not
only do you have to build it manually (arcan git repository, tools/vrbridge)
but you also have to set the path to it in the arcan configuration database,
see the README.md for the vrbridge tool on details for that.

## Roadmap / Status

Milestone 1:

- [ ] Devices
  - [x] Simulated (3D-SBS)
  - [x] Simple (Monoscopic 3D)
  - [x] Single HMD
  - [ ] Distortion Model
    - [p] Shader Based
    - [ ] Mesh Based
    - [ ] Validation
  - [x] Mouse
  - [x] Keyboard

- [ ] Models
  - [x] Primitives
    - [ ] Cube
      - [x] Basic mesh
      - [x] 1 map per face
      - [ ] cubemapping
    - [x] Sphere
      - [x] Basic mesh
      - [x] Hemisphere
    - [x] Cylinder
      - [x] Basic mesh
      - [x] half-cylinder
    - [x] Rectangle
		- [x] Custom Mesh (.ctm)
    - [ ] GlTF2
      - [ ] Simple/Textured Mesh
      - [ ] Skinning/Morphing/Animation
      - [ ] Physically Based Rendering
    - [x] Stereoscopic Mapping
      - [x] Side-by-Side
      - [x] Over-and-Under
      - [x] Swap L/R Eye

- [ ] Basic Layouter ("Window Manager")
  - [x] Circular Layers
  - [x] Swap left / right
  - [x] Cycle left / right
  - [ ] Minimize motion on rebuild
  - [x] Transforms (spin, nudge, scale)
  - [x] Curved Planes
  - [x] Billboarding
  - [x] Fixed "infinite" Layers
  - [x] Vertical hierarchies
  - [x] Connection- activated models

- [ ] Clients
  - [x] Built-ins (terminal/external connections)
	- [ ] Launch targets
  - [x] Xarcan
  - [ ] Wayland-simple (toplevel/fullscreen only)

Milestone 2:

- [ ] Advanced Layouters
  - [ ] Room Scale
  - [ ] Portals / Space Switching

- [ ] Improved Rendering
  - [ ] Equi-Angular Cubemaps
  - [ ] Stencil-masked Composition
  - [ ] Surface- projected mouse cursor

- [ ] Devices
  - [ ] Gloves
  - [ ] Eye Tracker
  - [ ] Video Capture Analysis
  - [ ] Positional Tracking / Tools
  - [ ] Dedicated Handover/Leasing
  - [ ] Reprojection
	- [ ] Mouse
	  - [ ] Gesture Detection
		- [ ] Sensitivity Controls
	- [ ] Keyboard
	  - [ ] Repeat rate controls
		- [ ] Runtime keymap switching
  - [ ] Multiple- HMDs
    - [ ] Passive
    - [ ] Active

- [ ] Clients
  - [ ] Full Wayland-XDG
    - [ ] Custom cursors
    - [ ] Multiple toplevels
    - [ ] Popups
    - [ ] Positioners
  - [ ] Full LWA (3D and subsegments)
    - [ ] Native Nested 3D Clients
    - [ ] Adoption (Crash Recovery, WM swapping)
    - [ ] Clipboard support

- [ ] Convenience
  - [ ] Streaming / Recording

Milestone 3:

- [ ] Devices
  - [ ] Haptics
  - [ ] Multiple, Concurrent HMDs
  - [ ] Advanced Gesture Detection
  - [ ] Kinematics Predictive Sensor Fusion

- [ ] Networking
  - [ ] Share Space
	- [ ] Dynamic Resource Streaming
	- [ ] Avatar Synthesis
	- [ ] Filtered sensor state to avatar mapping
	- [ ] Voice Chat

- [ ] Clients
  - [ ] Alternate Representations
  - [ ] Dynamic LoD

- [ ] Rendering
  - [ ] Culling
  - [ ] Physics / Collision Response
  - [ ] Multi-Channel Signed Distance Fields

