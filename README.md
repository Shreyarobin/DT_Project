# 🦾 RoboSim Pro — 6-DOF Industrial Robotic Arm Simulator

A browser-based, real-time 3D simulator for a 6-degree-of-freedom industrial robotic arm. Built entirely in a single HTML file using Three.js, it features full manual joint control, inverse kinematics, autonomous pick-and-place routines, stochastic failure modelling, and Monte Carlo performance analysis — all rendered in a dark industrial HUD aesthetic.

---

## ✨ Features

### 🤖 Arm Control
- **6-DOF joint control** — Base (±180°), Shoulder (±120°), Elbow (±150°), Wrist Pitch (±120°), Wrist Roll (±180°), Wrist Yaw (±90°)
- **Gripper control** — Continuously variable open/close with visual fill indicator
- **Speed modes** — SLOW (0.3×), NORMAL (1×), FAST (2.5×), MAX (5×)
- **Pose presets** — HOME, REACH, FOLD, LEFT, RIGHT, WAVE
- **Keyboard shortcuts** for all joints and base translation (WASD, arrow keys, IJKL, G/P, R, N)

### 🧠 Inverse Kinematics
- Real-time IK solver for end-effector positioning
- Click-to-drive: click any point on the floor to navigate the base
- Auto-pick shortcut (`N`) — navigate to nearest object and grasp it

### 🏭 Autonomous Simulation
- **Gather loop** — autonomous pick-and-place cycle; arm drives to scattered objects, picks them up, and deposits them in a designated collection zone
- **Normal mode** — low stochastic noise, high reliability
- **Stress mode** — elevated failure probabilities, joint fatigue, simulated sensor noise
- Graceful abort with safe object release mid-cycle

### 📊 Stochastic & Analytics
- **Live metrics dashboard** — pick attempts, success rate, collision count, fatigue level
- **Grasp quality score** — real-time quality bar updated per pick
- **Proximity sensor** — CLEAR / WARN / CRITICAL proximity HUD indicator with colour-coded alerts
- **Energy accumulator** — tracks cumulative joint movement as a proxy for power usage
- **Sparkline chart** — rolling success-rate history rendered on a canvas sparkline
- **Event log panel** — timestamped discrete-event stream (GRASP_SUCCESS, GRASP_FAILURE, COLLISION, PLACE_OBJECT, TASK_COMPLETE, etc.)
- **Monte Carlo analysis** — runs 100 simulated trials for both Normal and Stress scenarios; outputs mean success rate, standard deviation, 95% confidence interval, and a histogram comparison chart

### 🌐 3D Scene
- Three.js (r128) WebGL renderer with PCF soft shadows and sRGB output
- Industrial environment: gridded floor, safety lines, three-wall enclosure, ceiling light strips
- Detailed robot mesh: segmented arm links, cylindrical joints, parallel-jaw gripper with articulated fingers
- Orbit camera (drag to rotate, scroll to zoom), fog, and ambient/directional/point lighting

---

## 🚀 Getting Started

No build step or server required.

```bash
# Clone or download the repo
git clone https://github.com/your-username/robosim-pro.git
cd robosim-pro

# Open directly in a browser
open robotic_arm_v9_1.html
```

Or simply double-click `robotic_arm_v9_1.html` in your file explorer. A modern browser with WebGL support is all that is needed.

> **Tip:** For best performance use Chrome or Firefox on a desktop with GPU acceleration enabled.

---

## 🎮 Controls

### Keyboard

| Key(s) | Action |
|--------|--------|
| `←` / `→` | Rotate base |
| `↑` / `↓` | Shoulder pitch |
| `W` / `S` | Elbow pitch |
| `A` / `D` | Wrist roll |
| `Q` / `E` | Wrist yaw |
| `Z` / `X` | Wrist pitch |
| `G` | Pick (close gripper + grasp) |
| `P` | Put down (release object) |
| `R` | Reset to home position |
| `I` / `K` | Drive base forward / back |
| `J` / `L` | Drive base left / right |
| `N` | Navigate to nearest object + auto-pick |

### Mouse / Viewport

| Action | Result |
|--------|--------|
| Click + drag | Orbit camera |
| Scroll wheel | Zoom |
| Click floor | Drive base to that point |

### Panel Controls

| Control | Description |
|---------|-------------|
| Joint sliders | Direct per-joint angle input |
| Speed selector | Set animation playback speed |
| Preset buttons | Snap to named poses |
| AUTO SIM | Start autonomous gather loop |
| STOP SIM | Abort simulation gracefully |
| MONTE CARLO | Run 100-trial statistical analysis |
| NORMAL / STRESS | Switch scenario noise profile |

---

## 🗂️ Project Structure

```
robotic_arm_v9_1.html   # Complete self-contained application
README.md
```

Everything — HTML, CSS, JavaScript, and 3D scene — lives in a single file. External dependencies are loaded from CDN:

| Library | Version | Purpose |
|---------|---------|---------|
| [Three.js](https://threejs.org/) | r128 | 3D rendering |
| [Orbitron](https://fonts.google.com/specimen/Orbitron) | Google Fonts | HUD display font |
| [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) | Google Fonts | Panel monospace font |

---

## 🔬 Technical Details

- **IK Solver** — Analytic two-segment inverse kinematics; falls back gracefully when the target is unreachable
- **Event System** — Priority-queue discrete event scheduler drives stochastic failures, fatigue increments, and sensor noise injection
- **Collision Detection** — Bounding-sphere checks between gripper and scene objects; blocked motion triggers `BLOCKED` HUD state and logs a collision event
- **Monte Carlo Engine** — Pure JS statistical simulation; runs N trials sampling from per-scenario probability distributions and computes μ, σ, 95% CI, and histogram bins
- **Animation** — Promise-based interpolation (`pAnimateTo`) enables clean async/await sequencing of multi-step arm movements

---

## 🛠️ Customisation

| What to change | Where to look |
|----------------|--------------|
| Number of scattered objects | `const OBJ_COUNT` near scene initialisation |
| Collection zone position / radius | `COLLECTION_CENTER`, `COLLECTION_RADIUS` constants |
| Stress-mode failure probability | `STRESS_FAIL_PROB` constant |
| Joint angle limits | `min`/`max` attributes on slider elements & IK clamp values |
| Colour theme | CSS `:root` variables (`--accent`, `--accent2`, `--accent3`, etc.) |

---

## 🌐 Browser Compatibility

| Browser | Support |
|---------|---------|
| Chrome 90+ | ✅ Full |
| Firefox 88+ | ✅ Full |
| Edge 90+ | ✅ Full |
| Safari 15+ | ✅ Full |
| Mobile browsers | ⚠️ Partial (no keyboard shortcuts) |

---

## 📄 License

MIT — free to use, modify, and distribute.

---

## 🙏 Acknowledgements

- [Three.js](https://threejs.org/) — the 3D backbone
- Industrial HUD aesthetic inspired by real SCARA and 6-axis robot teach pendants
