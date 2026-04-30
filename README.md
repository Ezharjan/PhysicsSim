# Physics Simulation & Visualization Engine

[![DOI](https://zenodo.org/badge/270335361.svg)](https://doi.org/10.5281/zenodo.19326666)

A unified project combining a log-quaternion physics engine (Space-Time Field Reactor Physics) with interactive WebGL visualization demos. This repository explores rotation mathematics, Lorentz transformations, quantum probability, and data visualization techniques.

## Table of Contents

- [Overview](#overview)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Physics Engine (3D Demos)](#physics-engine-3d-demos)
- [Math Simulations](#math-simulations)
- [WebGL Visualization Demos](#webgl-visualization-demos)
- [Core Libraries](#core-libraries)
- [Free-Space Test Environment](#free-space-test-environment)
- [Hardware (FPGA)](#hardware-fpga)
- [AI Research](#ai-research)
- [Documentation & Papers](#documentation--papers)
- [API Reference](#api-reference)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Overview

This project began as an investigation into keeping rotations as rotation vectors using log-quaternions, eventually expanding to cover quantum probability, Lorentz transformations, relativity, Hopf fibrations, and more. It also includes a collection of WebGL data visualization demos from the "Data Visualization Techniques with WebGL" chapter.

### Key Concepts

- **Log-Quaternions (lnQuat)**: Natural logarithm mapping of quaternions enabling additive rotation composition, unbounded angle representation (beyond 360 degrees), and proper intrinsic vs. extrinsic rotation handling. Unlike standard quaternions that multiply to compose rotations, log-quaternions simply add — this makes interpolation, integration, and angular velocity calculations far more intuitive.

- **Rotation Vectors**: Coordinates in rotation space where the normal vector is the axis of rotation and the magnitude represents angular velocity. A rotation vector `(x, y, z)` encodes a rotation of `||(x,y,z)||` radians around the unit axis `(x,y,z)/||(x,y,z)||`.

- **Intrinsic vs. Extrinsic Rotations**: A single flag controls whether rotation forces are applied relative to the body's own frame (intrinsic) or the global frame (extrinsic), distinguished by a cross-product sign in the Rodrigues Rotation Formula. Intrinsic rotations (like an airplane's pitch/yaw/roll) rotate around the body's axes; extrinsic rotations (like a satellite tracked from ground) rotate around the world axes.

- **Dual Quaternions (dlnQuat)**: Extend log-quaternions with translation coordinates, providing a unified representation of both rotation and position in a single mathematical object.

## Project Structure

```
CG Effects/
|
|-- index.html                 Root demo: stereographic quaternion projection
|-- LICENSE                    MIT License
|-- _config.yml                Jekyll / GitHub Pages config
|-- .nojekyll                  Disable Jekyll processing
|-- .gitignore                 Git ignore rules
|
|-- 3d/                        3D physics demos (29 HTML pages)
|   |-- src/                   Core JavaScript source
|   |   |-- lnQuatSq.js       PRIMARY log-quaternion implementation (1979 lines)
|   |   |-- lnQuat.js         Legacy log-quaternion (kept for older demos)
|   |   |-- Quat.js           Standard quaternion
|   |   |-- three.js/          Three.js r119 library + extensions
|   |   |-- lnQuatTest*.js     Per-demo driver scripts
|   |   |-- geometrybuffer.js  Mesh generation
|   |   |-- marchingtetrahedra*.js   Isosurface algorithms (4 versions)
|   |   |-- dual-marching-tetrahedra-3D*.js  Dual-mesh algorithms (2 versions)
|   |   |-- surfacenets.js, marchingcubes.js  Alternative surface generators
|   |   |-- customShaderLoader*.js   WebGL shader utilities
|   |   +-- images/            Texture layers for demos
|   |-- index.html ... index7.html   Rotation curve visualizations
|   |-- indexArm*.html               Robot arm simulations (5 variants)
|   |-- indexSphereMap*.html          Grid-to-sphere mapping (4 variants)
|   |-- index-gravity-field.html     Gravity field visualization
|   |-- index-Orbits.html            Orbital mechanics
|   |-- index-thornson.html          Thomson scattering
|   |-- Isosurface Toolbox.html      3D surface generation tool
|   +-- *.md, *.png                  Per-demo documentation and screenshots
|
|-- math/                      Math & physics simulations (63 HTML demos)
|   |-- bellTest*.js           Bell inequality / quantum correlation (7 variants)
|   |-- interferometer*.js     Interferometer simulations (7 variants)
|   |-- lightSpeed*.js         Speed of light experiments (23 scripts)
|   |-- gravity*.js            Gravity field / wave simulations
|   |-- index*.html            One HTML page per simulation
|   |-- Quat.js, lnQuat.js    Local math library copies
|   |-- visualization.css      Shared styles
|   +-- *.png, *.md            Supporting diagrams and documentation
|
|-- lib/                       Reusable ES6 library modules (12 files)
|   |-- lnQuatSq.js / .mjs    Log-quaternion module exports
|   |-- math_config.mjs        Shared math configuration
|   |-- joint.mjs / testJoint.mjs  Joint/constraint system
|   |-- entityFrame.js         Entity frame management
|   |-- motionFrame.js         Motion frame calculations
|   |-- motiveFrame.js         Motive frame (intent-based)
|   |-- relativeFrame.js       Relative frame computations
|   |-- collisionFrame.js      Collision detection frame
|   |-- lnQuat.js              Legacy quaternion (module form)
|   +-- dual-quat.js           Dual quaternion (compact, 6K)
|
|-- src/                       Root-level source (2 files)
|   |-- dual-quat-webpage.js   Dual quaternion web UI (used by index.html)
|   +-- dual-quat.js           Dual quaternion full implementation (34K)
|
|-- demos/webgl/               WebGL data visualization demos (15 demos)
|   |-- color-cube/            Video color decomposition (RGB/Chroma/Hue)
|   |-- fractile/              Space-filling tiling slideshow
|   |-- hopf/                  Hopf fibration mapping (4D sphere)
|   |-- indraspearls/          Mobius transformations & Apollonian gaskets
|   |-- temperature-anomalies/ Climate data visualization
|   |-- wind/                  Wind pattern visualization (deck.gl)
|   |-- euclidean/             Euclidean geometry
|   |-- hyperbolic1/, hyperbolic2/  Hyperbolic geometry
|   |-- hyperfloors/           Hyperbolic floor patterns
|   |-- spherical1/, spherical2/    Spherical geometry
|   |-- solid1/                Solid geometry
|   |-- myriahedral/           Myriahedral projections
|   +-- dual_layer_poly/       Dual-layer polygon visualization
|
|-- docs/                      Documentation & research (36 files)
|   |-- research/              27 research documents (markdown)
|   |   |-- whitepaper.md      Core: log-quaternion theory & operations
|   |   |-- Curvature.md       Curvature and differential rotation
|   |   |-- Development.md     Intrinsic vs. extrinsic rotation modes
|   |   |-- Rotations.md       Rotation formalism overview
|   |   |-- Twister.md         Rodrigues Rotation Formula
|   |   |-- SpinProbabilities.md / QuantizedProbability.md  Quantum models
|   |   |-- FirstPrinciples.md  Foundational physics
|   |   |-- LightSpeedMath.md / LightSpeedNotes.md  Speed of light
|   |   |-- LorentzVsReality.md / Relativity.FAQ.md  Relativity
|   |   +-- OnRotationMatrices.md / 4D-cross-product.md  Algebra
|   |-- notes/                 4 development notes and raw data files
|   +-- *.tex                  4 LaTeX source files
|
|-- assets/                    Media assets (110 files)
|   |-- images/                105 diagrams, screenshots, visualizations
|   |-- papers/                4 research papers (.docx)
|   +-- presentations/         1 slide deck (.pptx)
|
|-- paper/                     Light Speed Paper v2 (21 chapter files)
|   +-- LSP2-*.md              Abstract, Introduction, Experiment, Results,
|                              Appendices A-J, References
|
|-- hardware/                  FPGA hardware designs (417 files)
|   +-- fpga/                  Verilog/VHDL + Xilinx Vivado project
|       |-- *.v                Clock modules, counters, simulation params
|       +-- vivado/            Cmod-A7 (Artix-7) board project
|
|-- AI/                        AI-assisted research (70 files)
|   |-- claud-rotations/       Rotation group monographs (6 files)
|   |-- Revised/               Revised research papers (24 files, md+pdf)
|   |-- space-web/             Voronoi / Three.js simulations (12 files)
|   |-- *.html                 12 standalone simulation demos
|   +-- *.md, *.py, *.pdf      Research documents and scripts
|
|-- freeSpaceTest/             Free-space physics test environment (107 files)
|   |-- index*.html / *.js     6 interactive test demos
|   |-- automaton/             Blockly-based automaton board (72 files)
|   |-- models/                9 3D model files (JSON, OBJ, glTF)
|   +-- jsox/                  JSOX serialization library
|
|-- warp/                      Spacetime warp visualization (2 files)
|   +-- index.html / index.js  Spin 1/2 simulation (Three.js + CDN)
|
|-- SoME1/                     Summer of Math Exposition (4 files)
|   +-- README.md + 3 PNGs    Rodrigues rotation research
|
+-- attach/media/              11 images referenced by paper/ chapters
```

## Getting Started

### Prerequisites

**Required:**

- A modern web browser with **WebGL2** support. Tested and confirmed working on Chrome 90+, Firefox 88+, and Edge 90+. Safari has partial WebGL2 support — some demos may not render correctly on Safari versions before 16.4.

- A local HTTP server. The demos use ES6 modules (`<script type="module">`) and cross-origin resource loading, which browsers block when opening files directly from disk (`file://` protocol). You must serve them through HTTP.

**Optional:**

- **Node.js** (v14 or later): Required only if you want to run the standalone math scripts (Bell tests, light speed calculations) from the command line.

- **npm**: Required only for the `freeSpaceTest/` demos, which reference `node_modules/@d3x0r/popups/styles.css`. Run `npm install` in the project root if you want those demos to render with proper styling.

- **Xilinx Vivado** (2020.2 or later): Required only for the FPGA hardware designs in `hardware/fpga/`. Not needed for any web-based demos.

- **LaTeX distribution** (e.g. TeX Live, MiKTeX): Required only if you want to compile the `.tex` formula documents in `docs/`.

### Step-by-Step Setup

**1. Clone the repository:**

```bash
git clone https://github.com/d3x0r/STFRPhysics.git
cd STFRPhysics
```

**2. Start a local web server** from the project root directory. Choose any of these methods:

```bash
# Python 3 (built-in, no install needed)
python -m http.server 8080

# Python 2
python -SimpleHTTPServer 8080

# Node.js (install serve globally first)
npm install -g serve
npx serve . -p 8080

# PHP (if installed)
php -S localhost:8080

# VS Code: install the "Live Server" extension, then right-click any
# HTML file and select "Open with Live Server"
```

**3. Open a demo in your browser.** Navigate to any of these entry points:

| URL | What It Opens |
|-----|---------------|
| `http://localhost:8080/index.html` | Root demo: stereographic dual quaternion projection |
| `http://localhost:8080/3d/index.html` | First 3D rotation demo (twist around lat/long) |
| `http://localhost:8080/3d/index4.html` | Hopf fibration curve generator |
| `http://localhost:8080/3d/indexArm.html` | 5-segment robot arm (additive rotations) |
| `http://localhost:8080/3d/Isosurface%20Toolbox.html` | Isosurface generation toolbox |
| `http://localhost:8080/math/index.html` | Math simulations hub (canvas-based) |
| `http://localhost:8080/math/indexBellInequality.html` | Bell inequality test |
| `http://localhost:8080/math/indexLightSpeed.html` | Speed of light experiment |
| `http://localhost:8080/demos/webgl/hopf/index.html` | Hopf fibration WebGL visualization |
| `http://localhost:8080/demos/webgl/indraspearls/index.html` | Indra's Pearls (Mobius transforms) |
| `http://localhost:8080/warp/index.html` | Spin 1/2 belt trick simulation |
| `http://localhost:8080/freeSpaceTest/index.html` | Free-space physics test environment |

**4. (Optional) Install npm dependencies** for `freeSpaceTest/` popup styles:

```bash
npm install @d3x0r/popups
```

**5. (Optional) Run math scripts from the command line:**

```bash
node math/bellTest.js         # Bell inequality correlation test
node math/lightSpeed.js       # One-way speed of light calculations
node math/interferometer.js   # Interferometer simulation output
```

These scripts output numerical results to the console and do not require a browser.

### Interaction Controls

Most 3D demos (`3d/`) share these controls:

- **Mouse drag**: Rotate the camera around the scene
- **Mouse wheel / scroll**: Zoom in and out
- **Sliders (A, B, C, D, E, T)**: Each demo has a set of on-screen sliders that control rotation parameters. Slider labels and functions vary per demo — the on-screen info panel at the top of each page explains what each slider does for that specific demo.
- **Fullscreen**: Press `F` or click the fullscreen button (provided by THREEx.FullScreen.js)
- **Screenshot**: Press `P` to capture the current frame (provided by THREEx.screenshot.js)

The `math/` demos use a 1024×1024 HTML canvas element with sliders for adjusting parameters like offset X/Y/Z/T, angles, and number of iterations. Results are drawn directly on the canvas.

## Physics Engine (3D Demos)

Interactive Three.js-based demos exploring rotation mathematics. All 29 HTML files are in the `3d/` directory. Each HTML page loads its own driver script from `3d/src/` (e.g., `index.html` loads `lnQuatTest.js`, `indexArm.html` loads `lnQuatTestArm.js`).

### Demo Reference

| Demo | File | Description | Key Concepts |
|------|------|-------------|--------------|
| Twist Around Lat/Long | `index.html` | Bundles of rotations around resulting curves | First rotation test; 3D curve generation |
| Sub-Arc | `index2.html` | Sub-arc of interest for rotation paths | Uses legacy `lnQuat.js` |
| Curvature Plots | `index3.html` | Free angle choice and normal vector selection | Uses legacy `lnQuat.js` |
| Hopf Fibration | `index4.html` | Hopf fibration curve generator | 4D sphere → 3D mapping |
| Spin Evolution | `index5.html` | Rotation state evolution over time | Continuous rotation tracking |
| Quantum Spin | `index5-spin.html` | Quantum spin probability visualization | Spin measurement probabilities |
| Extended Rotations | `index6.html`, `index7.html` | Advanced rotation demos | Multi-turn rotations |
| Robot Arm (Additive) | `indexArm.html` | 5-segment arm with additive rotations | Isosurface mesh + Perlin noise |
| Robot Arm (Proper) | `indexArmProper.html` | 5-segment arm with relative frames | Frame-relative rotation |
| Robot Arm (Proper 2) | `indexArmProper2.html` | Second proper arm variant | Uses legacy `lnQuat.js` |
| Arm (Extrinsic) | `indexArmProper-x.html` | Arm with extrinsic rotation mode | Extrinsic vs. intrinsic comparison |
| Arm Integration | `indexArmProper-Integration.html` | Internal/external rotation integration test | Angular velocity integration |
| Arm Integration 2 | `indexArmProper-Integration2.html` | Second integration variant | Extended integration tests |
| Relative Rotations | `indexRelative.html` | Differential rotation visualization | Inter-frame rotation differences |
| Sphere Mapping | `indexSphereMap.html` | Regular grid projected onto sphere | Grid-to-sphere distortion |
| Sphere Map 2 | `indexSphereMap2.html` | Second sphere mapping variant | Alternative projections |
| Sphere Map 3 | `indexSphereMap3.html` | Third sphere mapping variant | Additional mapping modes |
| Sphere Map Covers | `indexSphereMap-covers.html` | Sphere covering patterns | Covering-space topology |
| Orbits | `index-Orbits.html` | Orbital mechanics simulation | Gravity + rotation |
| Gravity Field | `index-gravity-field.html` | Gravity field visualization | Field lines and potentials |
| Thomson Scattering | `index-thornson.html` | Thomson scattering simulation | Electron-photon interaction |
| Black Voxel | `index-blackvoxel.html` | Voxel rendering test | Volumetric rendering |
| Normals | `index-normals.html` | Surface normal visualization | Normal vector display |
| Blank Template | `index-blank.html` | Empty template for new demos | Starter code |
| GC Navigation | `indexGCNav.html` | Great circle navigation | Uses legacy `lnQuat.js` |
| Harmonics | `indexHarmonics.html` | Harmonic rotation patterns | Frequency analysis |
| Random Normals | `indexRandomNormals.html` | Random normal vector distributions | Statistical rotation sampling |
| Isosurface Toolbox | `Isosurface Toolbox.html` | 3D isosurface generation tool | Marching tetrahedra/cubes/surfacenets |

### Legacy vs. Current Implementation

Most demos use `lnQuatSq.js` (the primary, current implementation). Four demos still depend on the legacy `lnQuat.js`: `index2.html`, `index3.html`, `indexArmProper2.html`, and `indexGCNav.html`. Both files must remain in `3d/src/` for all demos to function.

## Math Simulations

The `math/` directory contains 63 HTML demo pages with physics experiment simulations. These are canvas-based 2D visualizations (not Three.js) that render directly onto a 1024×1024 HTML canvas element. Each demo has sliders for adjusting parameters.

### Categories

**Bell Inequality Tests** (7 variants): `bellTest.js`, `bellTest-cos.js`, `bellTest2.js`, `bellTest2_polarizers.js`, `bellTest3.js`, `bellTest3_polarizers.js`, `bellTest_GHZ.js`

These simulate quantum correlation experiments. Each variant tests different polarizer configurations and compares quantum vs. classical predictions. The HTML pages include `indexBellInequality.html`, `indexBellInequality-cos.html`, `indexBellInequality2.html`, `indexBellInequality2-a.html`, `indexBellInequality2_polarizers.html`, `indexBellInequality3_polarizers.html`, and `indexBellInequality_GHZ.html`.

**Interferometer Simulations** (7 variants): `interferometer.js` through `interferometer4-all.js`

Model light interference in various optical configurations: standard Michelson, isosceles triangle, 4-arm, and composite arrangements. Pages include `indexInterferometer.html` through `indexInterferometer4-all.html`.

**Speed of Light Experiments** (23 scripts): `lightSpeed.js` and related files

Explore one-way speed of light measurements, clock synchronization paradoxes, radial emission patterns, wheel experiments, and relativistic scenarios. Over 20 HTML pages cover different aspects: basic one-way tests (`indexLightSpeed.html`), clock synchronization (`indexLightSpeed-Clocks.html`, `indexLightSpeed-Clocks2.html`), space-time diagrams (`indexLightSpeed-xt.html`), radial propagation (`indexLightSpeed-radial.html`), and special relativity comparisons (`indexLightSpeed-SR.html`).

**Gravity Simulations**: `gravityField.js`, `gravityFields.js`, `gravityFields.nbody.js`, `gravityRayTrace.js`, `gravity-wave*.js`

N-body gravitational simulations, gravity wave propagation (4 versions), orbital mechanics, and ray tracing through gravitational wells. Pages: `gravityFields.html`, `gravityFields.nbody.html`, `gravityOrbits.html`, `gravityOrbits-moving.html`, `gravityRayTrace.html`, and `indexGravityWave*.html`.

**Quantum Games**: `CHSH_Game.html`, `CHSH_Game2.html`, `GHZ_Game.html`

Interactive implementations of the CHSH and GHZ quantum game theory experiments.

**Other Simulations**: Curvature tests (`indexCurvature.html`), galaxy aberration (`indexGalaxyAberration.html`), electromagnetic propagation (`mathEMPropagation.html`), sphere decay (`mathSphereDecay.html`), relativistic acceleration (`indexRelativisticAccel.html`), balance wheel/beam mechanics (`indexBalanceWheel.html`, `indexBeamBalance.html`), and mathematical function tests (`indexSinCos.html`, `indexZeta.html`, `indexMatTest.html`, `indexAverage.html`).

## WebGL Visualization Demos

The `demos/webgl/` directory contains 15 self-contained visualization demos from the "Data Visualization Techniques with WebGL" chapter ([WebGL-Insights](http://webglinsights.blogspot.com/)). Each demo is a self-contained directory with its own `index.html` and bundled assets — no dependencies on the rest of the project.

| Demo | Directory | Description |
|------|-----------|-------------|
| Hopf Fibrations | `hopf/` | Interactive 4D sphere mapping via stereographic projection |
| Indra's Pearls | `indraspearls/` | Mobius transformations, Schottky groups, Apollonian gaskets |
| Temperature Anomalies | `temperature-anomalies/` | Climate data rendered in WebGL |
| Wind | `wind/` | Real-time wind pattern visualization (deck.gl) |
| Color Cube | `color-cube/` | Video color space decomposition (RGB, Chroma, Hue) |
| Fractile | `fractile/` | Space-filling tiling serving as slideshow |
| Euclidean | `euclidean/` | Euclidean geometry visualization |
| Hyperbolic | `hyperbolic1/`, `hyperbolic2/` | Hyperbolic geometry (2 variants) |
| Hyperfloors | `hyperfloors/` | Hyperbolic floor patterns |
| Spherical | `spherical1/`, `spherical2/` | Spherical geometry (2 variants) |
| Solid | `solid1/` | Solid geometry visualization |
| Myriahedral | `myriahedral/` | Myriahedral map projections |
| Dual Layer Poly | `dual_layer_poly/` | Dual-layer polygon rendering |

To open any WebGL demo, navigate to its subdirectory:

```
http://localhost:8080/demos/webgl/hopf/index.html
http://localhost:8080/demos/webgl/indraspearls/index.html
http://localhost:8080/demos/webgl/temperature-anomalies/index.html
```

## Core Libraries

### lnQuatSq.js — Primary Log-Quaternion Implementation

**Location:** `3d/src/lnQuatSq.js` (1979 lines)

This is the main log-quaternion library used by all newer demos. It represents rotations as natural log mappings of quaternions, allowing rotation composition through addition instead of quaternion multiplication.

**Basic usage:**

```javascript
// Import (when used as ES6 module)
import { lnQuat } from './lnQuatSq.js';

// Create a rotation: 90 degrees around the Y axis
const angle = Math.PI / 2;
const axis = { x: 0, y: 1, z: 0 };
const rotation = new lnQuat(angle, axis);

// Compose two rotations by addition
const rotation2 = new lnQuat(Math.PI / 4, { x: 1, y: 0, z: 0 });
rotation.add(rotation2);  // rotation now holds the combined rotation

// Apply rotation to a 3D point
const point = { x: 1, y: 0, z: 0 };
const rotated = rotation.apply(point);
// rotated.x, rotated.y, rotated.z contain the new coordinates

// Get the inverse rotation applied to a point
const unrotated = rotation.applyInv(rotated);

// Convert to standard quaternion (w, x, y, z)
const quat = rotation.exp();

// Get orientation as basis vectors
const basis = rotation.getBasis();
// basis.forward, basis.right, basis.up — each is {x, y, z}

// Reduce to principal rotation (remove excess 360° turns)
rotation.principal();

// Apply angular velocity (torque)
rotation.torque([pitchAngle, yawAngle, rollAngle], turns);

// Convenience methods for single-axis rotations
rotation.pitch(angle);   // rotate around X axis
rotation.yaw(angle);     // rotate around Y axis
rotation.roll(angle);    // rotate around Z axis

// Read back Euler-like angles
const p = rotation.getPitch();
const y = rotation.getYaw();
const r = rotation.getRoll();

// Get directional vectors
const fwd = rotation.forward();  // {x, y, z}
const rt  = rotation.right();    // {x, y, z}
const u   = rotation.up();       // {x, y, z}

// Spherical linear interpolation
const halfway = rotation.slerp(rotation2, 0.5);

// Differential rotation (apply a small rotation delta)
rotation.applyDel(direction, scale);

// Acceleration-based rotation (physics integration)
rotation.accel(angularVelocity, steps, internal);
// 'internal' flag: true = intrinsic (body frame), false = extrinsic (world frame)
```

**Intrinsic vs. Extrinsic rotation:**

The `internal` parameter in `accel()` and the cross-product sign in the Rodrigues formula control whether rotations are applied in the body's own reference frame (intrinsic, like an airplane's controls) or in the world frame (extrinsic, like a satellite tracked from the ground). This distinction is encoded as a sign difference in the cross product during the Rodrigues rotation step.

### lnQuat.js — Legacy Implementation

**Location:** `3d/src/lnQuat.js` (1104 lines)

The original log-quaternion implementation. Marked as obsolete in its first line, but still required by 4 demos: `index2.html`, `index3.html`, `indexArmProper2.html`, and `indexGCNav.html`. The API is similar to `lnQuatSq.js` but predates several improvements. Do not use for new code.

### Quat.js — Standard Quaternion

**Location:** `3d/src/Quat.js`

Standard quaternion representation `(w, x, y, z)` used as an intermediate form. `lnQuat.exp()` returns a `Quat` object. Provides multiplication, normalization, and conversion to rotation matrix.

### lib/ — ES6 Module Library

The `lib/` directory provides ES6 module versions (`*.mjs`) for use in modern JavaScript projects, plus a physics framework for articulated bodies:

| Module | Purpose |
|--------|---------|
| `lnQuatSq.js` / `lnQuatSq.mjs` | Log-quaternion core (ES6 module export) |
| `math_config.mjs` | Shared constants and configuration |
| `joint.mjs` | Joint constraint system for articulated bodies (robot arms, skeletal rigs) |
| `testJoint.mjs` | Joint system test harness |
| `entityFrame.js` | Manages entity state (position + orientation) |
| `motionFrame.js` | Tracks velocity and acceleration |
| `motiveFrame.js` | Intent-based motion planning (AI/control targets) |
| `relativeFrame.js` | Frame-of-reference transformations between entities |
| `collisionFrame.js` | Collision detection framework |
| `lnQuat.js` | Legacy quaternion (module form) |
| `dual-quat.js` | Dual quaternion for combined rotation + translation (compact, 6K) |

**Using as ES6 modules:**

```javascript
import { lnQuat } from './lib/lnQuatSq.mjs';
import { Joint } from './lib/joint.mjs';
import { EntityFrame } from './lib/entityFrame.js';
```

### Dual Quaternions

Two implementations exist for different use cases:

- `src/dual-quat.js` (34K): Full implementation with web UI bindings, used by the root `index.html` demo for stereographic projection visualization.
- `lib/dual-quat.js` (6K): Compact module version for integration into other projects.

Dual quaternions combine rotation and translation into a single mathematical object, useful for rigid body transformations, skeletal animation, and screw motion representation.

### Isosurface Algorithms

The `3d/src/` directory includes multiple isosurface extraction algorithms, each generating 3D mesh surfaces from scalar fields:

- **marchingtetrahedra.js** through **marchingtetrahedra4.js**: Four progressive versions of the marching tetrahedra algorithm with different optimization strategies.
- **dual-marching-tetrahedra-3D.js** and **dual-marching-tetrahedra-3D2.js**: Dual-mesh variants that produce smoother surfaces.
- **marchingcubes.js**: Classic marching cubes algorithm.
- **surfacenets.js**: Surface nets algorithm for smoother mesh output.

These are loaded by the `Isosurface Toolbox.html` and the robot arm demos for real-time mesh generation.

## Free-Space Test Environment

**Location:** `freeSpaceTest/` (107 files)

A self-contained physics sandbox for testing log-quaternion mechanics in a 3D environment with object loading, collision detection, and neural network-based control.

| Demo | File | Description |
|------|------|-------------|
| Main Test | `index.html` | Core free-space rotation vector test with neural network control |
| Cross Product | `index-cross-product.html` | Cross product visualization and testing |
| Geometry | `index-geometry.html` | Geometry primitive testing |
| Orbit Test | `index-orbit-test.html` | Orbital mechanics in free space |
| Orbit Test 3 | `index-orbit-test3.html` | Extended orbital test |
| Rotor Field | `index-rotor-field.html` | Rotor field visualization |

**Dependencies:** These demos reference `node_modules/@d3x0r/popups/styles.css` for popup/UI styling and import from `../3d/src/three.js/three.module.js` and `../3d/src/lnQuatSq.js`. Run `npm install @d3x0r/popups` in the project root before using.

**Subdirectories:**

- `automaton/`: A Blockly-based visual programming interface for controlling automaton behavior (72 files, including Google's Blockly library).
- `models/`: 3D model files in JSON, OBJ, and glTF formats for loading into the test environment.
- `jsox/`: JSOX (JavaScript Object eXchange) serialization library for saving/loading state.

## Hardware (FPGA)

**Location:** `hardware/fpga/` (417 files)

FPGA-based hardware implementation of the lnQuat adder for real-time quaternion pose tracking in robotics. Targets the Digilent Cmod-A7 board (Xilinx Artix-7 FPGA).

### Verilog Source Files

| File | Purpose |
|------|---------|
| `clock-module.v` through `clock-module.5.v` | Clock generation modules (5 iterations) |
| `Counter.v` | Basic counter module |
| `new-counter.v`, `new-counter2.v` | Revised counter implementations |
| `SimulationParams.v` | Simulation parameter definitions |

### Vivado Projects

The `vivado/` directory contains three Xilinx Vivado project directories:

- **Cmod-A7/** (`CMod-A7.xpr`): Primary project targeting the Digilent Cmod-A7 board (Artix-7 XC7A35T).
- **Cora-7z/** (`Cora-7z.xpr`): Alternative project targeting the Digilent Cora Z7 board (Zynq-7000).
- **myip/**: Custom AXI IP cores — `AXI_Header`, `InputMerge`, and `OutputSplitter` for interfacing the lnQuat adder with a processor bus.

### Building the FPGA Project

1. Install Xilinx Vivado (2020.2 or later recommended).
2. Open the `.xpr` project file for your target board (e.g., `hardware/fpga/vivado/Cmod-A7/CMod-A7.xpr`).
3. Run synthesis, implementation, and bitstream generation within Vivado.
4. Program the FPGA board via USB using Vivado's hardware manager.

**Reference:** [FPGA lnQuat Adder Paper (ARITH20)](http://www.acsel-lab.com/arithmetic/arith20/papers/ARITH20_Arnold.pdf)

## AI Research

**Location:** `AI/` (70 files)

AI-assisted research documents, simulation demos, and revised papers generated with Claude and other AI tools.

- **`claud-rotations/`** (6 files): Monographs on rotation groups and Lie algebra, AI-generated with human review.
- **`Revised/`** (24 files): Revised versions of research papers in markdown and PDF format.
- **`space-web/`** (12 files): Voronoi diagrams and Three.js space simulations.
- **12 standalone HTML demos**: Interactive simulations including Ehrenfest-Doppler observer (`ehrenfest_doppler_observer.html`), LC circuit phase circles (`lc_threephase_circles.html`), vacuum wavepackets (`lc_vacuum_wavepackets_17.html`), gravitational lensing comparison (`lensing_compare_3.html`), N-body displacement shader (`nbody_displacement_shader.html`), resonant cavity transport (`resonant_cavity_transport.html`), Huygens wavelet simulation (`sonnet-claud-huygens4.html`), STFR 3-body sigma model (`stfr_3body_sigma.html`), STFR lens model (`stfr_lens_v33.html`), N-body simulations (`stfr_nbody.html`, `stfr_nbody_sigma.html`), and Thomson acceleration (`thornson_v10_accel.html`).

These AI HTML demos are self-contained and can be opened directly:

```
http://localhost:8080/AI/stfr_nbody.html
http://localhost:8080/AI/lensing_compare_3.html
```

## Documentation & Papers

### Research Documents (`docs/research/` — 27 files)

| Document | Topic |
|----------|-------|
| [whitepaper.md](docs/research/whitepaper.md) | Core theory: log-quaternion operations and conversions |
| [Curvature.md](docs/research/Curvature.md) | Curvature and differential rotation in demos |
| [Development.md](docs/research/Development.md) | Internal vs. external rotation exploration |
| [Twister.md](docs/research/Twister.md) | Rodrigues Rotation Formula application |
| [Rotations.md](docs/research/Rotations.md) | Rotation formalism overview |
| [SpinProbabilities.md](docs/research/SpinProbabilities.md) | Quantum spin probability models |
| [QuantizedProbability.md](docs/research/QuantizedProbability.md) | Quantum mechanics formulations |
| [FirstPrinciples.md](docs/research/FirstPrinciples.md) | Foundational physics principles |
| [LorentzVsReality.md](docs/research/LorentzVsReality.md) | Lorentz transformation analysis |
| [LightSpeedMath.md](docs/research/LightSpeedMath.md) | One-way speed of light mathematics |
| [LightSpeedNotes.md](docs/research/LightSpeedNotes.md) | Light speed experiment notes |
| [Relativity.FAQ.md](docs/research/Relativity.FAQ.md) | Relativity FAQ |
| [OnRotationMatrices.md](docs/research/OnRotationMatrices.md) | Rotation matrix theory |
| [4D-cross-product.md](docs/research/4D-cross-product.md) | 4D geometry operations |
| [SphereMap.md](docs/research/SphereMap.md) | Sphere mapping techniques |
| [FrenetConversions.md](docs/research/FrenetConversions.md) | Frenet frame conversions |
| [LHV_Theory.md](docs/research/LHV_Theory.md) | Local hidden variable theory |
| [LieProductRule.md](docs/research/LieProductRule.md) | Lie product rule applications |
| [WhatIs.md](docs/research/WhatIs.md) | Foundational explanations |
| [AncientBalanceBeams.md](docs/research/AncientBalanceBeams.md) | Ancient balance beam physics |
| [RelativisticOrbits.md](docs/research/RelativisticOrbits.md) | Relativistic orbital mechanics |
| [lightspeedclocks.md](docs/research/lightspeedclocks.md) | Light speed clock analysis |
| [light-speed-force.md](docs/research/light-speed-force.md) | Force in light speed context |
| [LightSpeedExperiment.md](docs/research/LightSpeedExperiment.md) | Light speed experiment design |
| [LightSpeedExperiment-HW-Development.md](docs/research/LightSpeedExperiment-HW-Development.md) | Hardware development for light speed experiment |
| [LightSpeedSim.md](docs/research/LightSpeedSim.md) | Light speed simulation design |
| [Theoretical Physics Issues.md](docs/research/Theoretical%20Physics%20Issues.md) | Open theoretical physics issues |

### Light Speed Paper v2 (`paper/` — 21 files)

A complete academic paper on one-way speed of light measurements, split into chapters for easier editing:

`LSP2-Abstract.md` → `LSP2-Introduction.md` → `LSP2-Propagation.md` → `LSP2-Experiment.md` → `LSP2-Results.md` → `LSP2-Doppler.md` → `LSP2-Light-Aberration.md` → `LSP2-Length-Contraction.md` → `LSP2-Full-Transformation.md` → Appendices A through J → `LSP2-References-and-Declarations.md`

Images referenced in the paper chapters are located in `attach/media/` (11 image files).

### Other Resources

- **LaTeX Sources** (`docs/*.tex`): `AxisFromLatLong.tex`, `interpolate.tex`, `math.tex`, `polarmap.tex` — mathematical formula typesetting for axis conversion, interpolation, and polar mapping.
- **Development Notes** (`docs/notes/`): `MATH.md`, `axisPermutations.txt`, `lightSpeedScribbles.txt`, `twister-work.txt`.
- **Research Paper Drafts** (`assets/papers/`): 4 Word documents — `LightSpeedPaper.docx`, `LightSpeedPaper2.docx`, `LightSpeedPaper-CoverLetter.docx`, `LightSpeedPaper-TItlePage.docx`.
- **Presentations** (`assets/presentations/`): "A One-way Speed of Light.pptx" — slide deck on the one-way speed of light research.
- **Summer of Math Exposition** (`SoME1/`): Rodrigues rotation research submission with 3 result plot images.

Also see the [Wikipedia article on Quaternion Natural Log](https://en.wikipedia.org/wiki/Rotation_formalisms_in_three_dimensions#Quaternion_Natural_Log).

## API Reference

### lnQuat Constructor and Properties

```javascript
const q = new lnQuat(theta, direction, a, b, e);
```

- `theta` (Number): Rotation angle in radians
- `direction` (Object `{x, y, z}`): Rotation axis (will be normalized internally)
- `a`, `b`, `e` (Number, optional): Direct component values

The internal state is stored as `q.x`, `q.y`, `q.z` — these are the rotation vector components (axis × angle).

### Complete Method Reference

| Method | Signature | Returns | Description |
|--------|-----------|---------|-------------|
| `set` | `(theta, d, a, b, e)` | `this` | Reset rotation from angle + axis |
| `scale` | `(n)` | `this` | Scale rotation magnitude by factor `n` |
| `add` | `(q2, t)` | `this` | Add rotation `q2` (in-place), optional time scale `t` |
| `add2` | `(q2, t)` | `this` | Alternative addition variant |
| `sub2` | `(q)` | `this` | Subtract rotation |
| `addConj` | `(q)` | `this` | Add conjugate of rotation |
| `cross` | `(other, target)` | target | Cross product with another lnQuat |
| `exp` | `(target, t)` | `Quat` | Convert to standard quaternion, optional time `t` |
| `apply` | `(v)` | `{x,y,z}` | Rotate vector `v` by this rotation |
| `applyInv` | `(v)` | `{x,y,z}` | Apply inverse rotation to vector |
| `applyDel` | `(v, del, q2, linear, result2)` | result | Differential rotation application |
| `getBasis` | `()` | `{forward, right, up}` | Get rotation as orthonormal basis vectors |
| `getBasisT` | `(del)` | `{forward, right, up}` | Get basis at fractional rotation `del` |
| `getBasisV` | `(del)` | `{forward, right, up}` | Get basis vectors variant |
| `getRelativeBasis` | `(q2)` | basis | Get basis relative to another rotation |
| `getFrame` | `(t, x, y, z)` | frame | Get full transformation frame |
| `getFrameFunctions` | `(lnQvel)` | functions | Get callable frame functions with velocity |
| `slerp` | `(p, t, target, oct)` | target | Spherical interpolation at parameter `t` |
| `slerpRel` | `(q2, del)` | result | Relative slerp between two rotations |
| `spinDiff` | `(p, target)` | target | Compute spin difference between rotations |
| `spin` | `(th, axis, oct)` | `this` | Apply spin rotation |
| `freeSpin` | `(th, axis, oct)` | `this` | Apply free spin (no constraints) |
| `accel` | `(v, steps, internal)` | `this` | Apply acceleration; `internal`=true for intrinsic |
| `pitch` | `(angle)` | `this` | Rotate around X axis |
| `yaw` | `(angle)` | `this` | Rotate around Y axis |
| `roll` | `(angle)` | `this` | Rotate around Z axis |
| `twist` | `(angle)` | `this` | Apply twist rotation |
| `getPitch` | `()` | Number | Get current pitch angle |
| `getYaw` | `()` | Number | Get current yaw angle |
| `getRoll` | `()` | Number | Get current roll angle |
| `forward` | `()` | `{x,y,z}` | Get forward direction vector |
| `right` | `()` | `{x,y,z}` | Get right direction vector |
| `up` | `()` | `{x,y,z}` | Get up direction vector |
| `principal` | `()` | `this` | Reduce to principal rotation (mod 2π) |
| `torque` | `(angles, turns)` | `this` | Apply torque forces |
| `fromBasis` | `(basis)` | `this` | Set rotation from basis matrix |
| `update` | `()` | `this` | Recalculate internal state |

### Standalone Functions

```javascript
lnQuatAdd(q, q2, s)  // Add q2 to q with scale s, returns new lnQuat
lnQuatSub(q, q2, s)  // Subtract q2 from q with scale s, returns new lnQuat
slerp2(q, p, t, target, external)  // Standalone slerp function
```

## Troubleshooting

**Blank screen / no rendering:**
Ensure you are serving files through an HTTP server, not opening them directly as `file://`. ES6 modules and cross-origin requests are blocked by browsers on the `file://` protocol.

**WebGL errors in console:**
Verify your browser supports WebGL2. Open `chrome://gpu` (Chrome) or `about:support` (Firefox) and check that "WebGL2" shows as enabled. Update your graphics drivers if WebGL2 is listed as unavailable.

**Three.js "module not found" errors:**
The project bundles Three.js r119 in `3d/src/three.js/`. Do not install a different version via npm — the demos depend on the specific bundled version and its local extensions (TrackballControls, MeshLine, DragPanControls).

**freeSpaceTest demos show unstyled popups:**
Run `npm install @d3x0r/popups` in the project root to install the popup CSS dependency.

**FPGA bitstream generation fails:**
Ensure you are using Xilinx Vivado 2020.2 or later. The Cmod-A7 project targets the XC7A35T part — verify the correct part is selected in Vivado's project settings.

**Math scripts fail with "require is not defined":**
Some scripts use ES6 import syntax. Run them with Node.js v14+ and ensure you're in the project root directory.

**GitHub Pages links in HTML files:**
Some HTML demos contain links to `d3x0r.github.io/STFRPhysics/...`. These are external URLs pointing to the live hosted version and do not affect local operation.

## Glossary

- **quat**: Short for "quaternion" — a 4-component number `(w, x, y, z)` representing a 3D rotation
- **lnQuat**: Quaternion in natural log mapping. A 3-component rotation vector `(x, y, z)` where the axis and angle are combined. `lnQuat().exp()` converts back to a standard `Quat()`
- **dlnQuat**: Dual log-quaternion; extends lnQuat with translation coordinates for combined rotation + position
- **SLERP**: Spherical Linear Interpolation — smooth interpolation between two rotation states along the shortest arc on the rotation sphere
- **Rodrigues Rotation Formula**: The formula used to rotate a vector around an axis by an angle; the core mathematical operation inside `apply()` and `accel()`
- **Intrinsic rotation**: Rotation applied relative to the body's own axes (e.g., an airplane's pitch/yaw/roll)
- **Extrinsic rotation**: Rotation applied relative to fixed world axes (e.g., tracking a satellite from the ground)
- **Hopf fibration**: A continuous mapping from the 3-sphere (S³) to the 2-sphere (S²), visualized in several demos
- **Isosurface**: A 3D surface representing points of constant value in a scalar field, extracted using marching tetrahedra/cubes algorithms

## References

- [Visualizing Quaternions (YouTube)](https://www.youtube.com/watch?v=d4EgbgTm0Bg)
- [FPGA lnQuat Adder (ARITH20 Paper)](http://www.acsel-lab.com/arithmetic/arith20/papers/ARITH20_Arnold.pdf)
- [Practical Exponential Coordinates using Implicit Dual Quaternions](http://www.neil.dantam.name/papers/dantam2018practical.pdf)
- [Quaternion Formulas Collection](https://maxime-tournier.github.io/notes/quaternions.html)
- [Wikipedia: Quaternion](https://en.wikipedia.org/wiki/Quaternion)
- [Wikipedia: Angular Velocity](https://en.wikipedia.org/wiki/Angular_velocity)
- [Wikipedia: Rotation Formalisms — Quaternion Natural Log](https://en.wikipedia.org/wiki/Rotation_formalisms_in_three_dimensions#Quaternion_Natural_Log)
- [Quaternions and Exponentials (JHU Slides)](http://www.cs.jhu.edu/~misha/Fall19/29.pdf)
- [WebGL Insights — Data Visualization Techniques](http://webglinsights.blogspot.com/)

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
