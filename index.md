# F1-Gantry
A production-grade, native **Swift / SwiftUI** application simulating the official FIA Formula 1 starting gantry system. It features high-fidelity visual rendering, a zero-dependency in-memory audio synthesizer, a non-blocking monotonic state machine, and sub-millisecond reaction telemetry with jump-start penalty validation.

<br>

![F1-Gantry](https://raw.githubusercontent.com/barshasantak/f1-gantry/main/f1-gantry.png)

<br>
 
## 🏎️ Project Overview

In Formula 1, standing starts are critical tactical moments where driver reaction times (typically between 180ms and 250ms) can dictate the outcome of a Grand Prix. 

**F1-Gantry** brings the engineering rigor of Formula 1 starting systems to Apple platforms:
- **Dual-Enclosure Gantry:** 5 columns of synchronized red lights across 2 rows, flanked by an integrated green light enclosure (Column 6).
- **Automated Procedural Countdown:** Sequential activation of red light pairs at exact 1.0-second intervals.
- **FIA Variable Abort/Hold Window:** A randomized hold between 1.0 and 4.0 seconds simulating the automated race-start system.
- **Lights Out Execution:** Instantaneous extinguishment of all red lamps, activation of green clearance lamps for 3.0 seconds, and emission of the 1800 Hz "Go" acoustic burst.
- **Reaction Time Telemetry:** High-precision measurement logging driver response delta upon clicking **"START RACE CAR"**, coupled with false-start jump detection under FIA sporting criteria.


## 📜 FIA Regulatory Background & Rules

The timing sequences, light layout, and penalties implemented in this simulator are derived directly from the **FIA Formula 1 Sporting Regulations (Articles 44 & 48)**:

| Parameter | Official FIA Specification | Simulator Implementation |
| :--- | :--- | :--- |
| **Lamp Layout** | Two horizontal rows of 5 red lights positioned over the grid. Green lights indicate track clearance or race start. | 2 Rows × 5 Columns Red + 2 Rows × 1 Column Green. Synchronized vertically per column. |
| **Countdown Interval** | Red lights illuminate sequentially at **1.0-second intervals** (Column 1 through Column 5). | Non-blocking 1.0-second intervals via `Task.sleep` using monotonic clock tracking. |
| **Random Hold Window** | Once all 5 red lights are illuminated, the preset system initiates a random delay of **0.2s to 3.0s** (extended to 1.0s–4.0s here for human testing). | `Double.random(in: 1.0...4.0)` |
| **The "Start" (Lights Out)** | The race start is signalled by **all red lights being extinguished simultaneously**. | Reds immediately extinguish, greens illuminate, and `ContinuousClock.now` records the start mark. |
| **Green Light Duration** | Green lights remain illuminated for track clearance after the start. | Maintained for exactly **3.0 seconds**, after which the system resets to the initial idle state. |
| **Jump Start (False Start)** | **Art. 48.1.a:** A car moves before the red lights are extinguished. Penalized with a **5-second time penalty** or drive-through. | If **"START RACE CAR"** is pressed before `currentState == .lightsOutGo`, a **FALSE START** is flagged, the run aborts, and a 5.0s penalty is logged. |


## Help and Support

### Report Issues
You can report any issues here: [https://forms.gle/XDUkjJ2TJzEruakX9](https://forms.gle/XDUkjJ2TJzEruakX9){:target="_blank"}

Please provide clear, detailed information and the correct repository for the issue so it can be properly triaged and addressed. 

  
<br>
  
<hr>

<small>*© 2026 Santak Das, Tara Design Studio. All rights reserved.*</small>

<br>
