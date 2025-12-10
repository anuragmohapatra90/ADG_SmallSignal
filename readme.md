# ADG\_SmallSignalADG

## Advanced Damping and Generator Small-Signal Analysis

This repository contains Python scripts for analyzing and designing damping controllers to improve the small-signal stability of power systems. It covers both conventional methods (Power System Stabilizers - PSS) and advanced techniques using modern Inverter-Based Resources (IBR) as Power Oscillation Dampers (POD) or for high-frequency LCL filter damping.

The core approach in all files is linearized state-space modeling ($\Delta \dot{x} = A\Delta x + B\Delta u$) to perform stability analysis (eigenvalues, root locus) and time-domain simulations.

## Key Features & Topics Covered

  * **Small-Signal Stability Analysis:** Calculating eigenvalues, damping ratios, and participation factors for system modes.
  * **Conventional PSS Design:** Implementing a conventional PSS to damp electromechanical oscillations (swing modes).
  * **Inverter-as-a-POD:** Utilizing a Grid-Forming Inverter's fast current control loop to provide supplementary damping torque to a synchronous generator.
  * **Inverter High-Frequency Damping:** Analyzing LCL filter resonance instability in a Grid-Following Inverter (GFL) and comparing passive damping (physical resistors) versus active damping (virtual resistors) as a stabilization technique.

## Project Files Breakdown

| File Name | Description | Key Focus |
| :--- | :--- | :--- |
| `pss_in_conventional_power_systems.py` | Implementation and tuning of a conventional PSS on a 9-state synchronous generator model. | **PSS Design** for swing mode damping. Includes visualization of the augmented system's state matrix and root locus analysis across varying PSS gain ($K_{pss}$). |
| `pod_with_an_inverter.py` | Models a hybrid system (Synchronous Generator + Inverter) where the inverter is used as a fast **Power Oscillation Damper (POD)**. | **Inverter-as-a-POD** for low-frequency swing mode damping. Demonstrates how the inverter current injection counteracts generator power oscillations and includes a power breakdown analysis. |
| `grid_following_inverter_as_a_linear_control_model.py` | Models a Grid-Following Inverter (GFL) with an LCL filter and its full control hierarchy (PLL, Power Loop, Current Loop). | **Active Damping** (Virtual Resistor) for LCL resonance. Compares the instability of a high-efficiency (low resistance) LCL filter against stabilization using passive damping (high resistance) versus active damping (software-based). |

## Technical Background

### State-Space Modeling

All models rely on linearizing the system dynamics around an operating point ($x_0, u_0$).

$$\Delta \dot{x} = A \Delta x + B \Delta u$$
$$\Delta y = C \Delta x + D \Delta u$$

  * **Stability:** Determined by the real parts of the eigenvalues of the **$A$** matrix. A negative real part indicates a stable mode (damped oscillation).
  * **Controller Augmentation:** The controller (PSS or POD) is modeled in state-space and appended to the plant matrix, creating a new augmented closed-loop matrix ($A_{cl}$):

$$A_{cl} = \begin{bmatrix} A_p & B_{p,ctrl} C_{c} \\ B_{c} C_{p, \omega} & A_{c} \end{bmatrix}$$

### Damping Concepts

| Controller | Target Oscillation | Actuator | Stabilization Mechanism |
| :--- | :--- | :--- | :--- |
| **PSS** | Low-frequency (0.5 - 2 Hz) Electromechanical Oscillation | Generator Exciter ($V_{r}$) | Modulates field voltage to provide damping torque. |
| **POD (Inverter)** | Low-frequency (0.5 - 2 Hz) Electromechanical Oscillation | Inverter Current ($I_{d,ref}$) | Modulates active current injection to counteract generator power changes. |
| **Active Damping** | High-frequency (e.g., LCL Resonance) | Inverter Voltage ($V_{inv}^*$) | Uses capacitor current feedback to create a **virtual resistor** to damp resonance without physical losses. |

## Requirements

The scripts require standard scientific Python libraries.

```bash
pip install numpy pandas matplotlib scipy
```

## Usage

Each file is designed to be run independently to generate visualizations and reports on the system's performance and stability.

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/anuragmohapatra90/ADG_SmallSignal.git
    cd ADG_SmallSignal
    ```
-----
