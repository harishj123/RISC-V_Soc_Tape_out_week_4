
---

# 🧠 Day 2 – Week 4: MOSFET Characteristics and CMOS Voltage Transfer Analysis

---

## 🌟 Welcome to Day 2!

Welcome to **Day 2 of Week 4**, where we dive deeper into the fascinating world of **MOSFET behavior** and **digital switching analysis**.

Today’s exploration bridges the **analog characteristics** of MOS transistors with the **digital logic behavior** of CMOS inverters. You’ll uncover how channel dimensions influence transistor current flow, and how these physical properties ultimately shape the **voltage transfer characteristics (VTC)** of a CMOS circuit — the heart of every digital logic gate.

---

## ⚙️ Overview

This session focuses on understanding:

* The difference between **long-channel** and **short-channel** MOSFETs.  
* The impact of **scaling and velocity saturation** on current flow.  
* How **CMOS inverters** convert analog input transitions into crisp digital outputs.

By the end of this lab, you’ll clearly visualize how the **microscopic physics of transistors** connect to **macroscopic circuit-level logic behavior**.

---

## 🧩 1. Channel Variations in MOSFETs

MOSFETs operate differently depending on their **channel length (L)**. The channel acts as a highway for charge carriers — and when it’s shortened, traffic rules change dramatically.

* **Long Channel MOSFET** – Classic operation with linear carrier velocity.  
* **Short Channel MOSFET** – Advanced operation where carrier velocity saturates due to high electric fields.

  ![image alt](https://github.com/harishj123/RISC-V_Soc_Tape_out_week_4/blob/main/week_4/images_Day2/Quadratic%20and%20linear%20graph.png?raw=true)

---

## 2. Long Channel MOSFET

### 🧠 Concept:

In long-channel MOSFETs, the carrier velocity `v` increases linearly with the electric field `E`:

![image alt](https://github.com/harishj123/RISC-V_Soc_Tape_out_week_4/blob/main/week_4/images_Day2/Drain%20current%20model.png?raw=true)


v = μ_n * E

Thus, the drain current `I_D` depends quadratically on the gate overdrive `(V_GS - V_TH)`.

---

### 2.1 Drain Current Model (Saturation Region)


I_D = (1/2) * μ_n * C_ox * (W / L) * (V_GS - V_TH)^2


This relationship shows quadratic control — a small increase in gate voltage produces a large increase in drain current (useful in analog amplification).

---

### 2.2 Regions of Operation

| Region              | Condition                  | Current Equation                                                                 | Behavior          |
| :------------------ | :------------------------- | :------------------------------------------------------------------------------- | :---------------- |
| **Cut-off**         | `V_GS < V_TH`              | `I_D = 0`                                                                        | No conduction     |
| **Linear / Triode** | `V_DS < V_GS - V_TH`       | `I_D = μ_n * C_ox * (W / L) * [ (V_GS - V_TH)*V_DS - (V_DS^2)/2 ]`               | Ohmic region      |
| **Saturation**      | `V_DS ≥ V_GS - V_TH`       | `I_D = (1/2) * μ_n * C_ox * (W / L) * (V_GS - V_TH)^2`                           | Current saturates |

---

## 3. Short Channel MOSFET

When device length `L` becomes very small (e.g., `L < 0.1 µm`), the electric field inside the channel can become high enough to **saturate carrier velocity** at `v_sat`. This changes the `I_D` dependence from quadratic to approximately linear in `(V_GS - V_TH)`.


v = { μ_n * E          , for E < E_crit
v_sat            , for E >= E_crit }


---

### 3.1 Drain Current (Velocity-Saturated Region)

![image alt](https://github.com/harishj123/RISC-V_Soc_Tape_out_week_4/blob/main/week_4/images_Day2/velocity%20saturation%20effect.png?raw=true)


I_D = W * C_ox * (V_GS - V_TH) * v_sat


Here, `I_D` grows approximately linearly with `(V_GS - V_TH)` — velocity saturation dominates.

---

### 3.2 Regions of Operation

![image alt](https://github.com/harishj123/RISC-V_Soc_Tape_out_week_4/blob/main/week_4/images_Day2/Region%20of%20operation.png?raw=true)


| Region                             | Condition                        | Behavior       |
| :--------------------------------- | :------------------------------- | :------------- |
| **Cut-off**                        | `V_GS < V_TH`                    | Device OFF     |
| **Linear**                         | `V_DS < V_GS - V_TH`             | Ohmic region   |
| **Saturation (Pre-vsat)**          | `V_DS = V_GS - V_TH`             | Quadratic rise |
| **Velocity-Saturated**             | `V_DS > V_DS,sat`                | Linear rise    |

---

### 3.3 Peak Current Variation

The drain current is expressed as:
Id = (Kn / 2) * (Vgs - Vth)^2 * (1 + λVds)

Here, the current increases with Vgs and slightly with Vds due to channel length modulation.
Higher Vgs values produce stronger inversion layers, resulting in greater peak current.

Observation

As Vgs increases, the Id–Vds curve rises upward.

Peak current occurs in the saturation region where Id becomes nearly constant.

Small slope in saturation is due to λ (channel length modulation).

![image alt](https://github.com/harishj123/RISC-V_Soc_Tape_out_week_4/blob/main/week_4/images_Day2/peak%20current%20variation.png?raw=true)


### 3.3 Comparison Table

| Property          | Long Channel                  | Short Channel                     |
| :---------------- | :---------------------------: | :-------------------------------- |
| Velocity Relation | `v ∝ E`                       | `v = v_sat`                       |
| `I_D` vs `V_GS`   | Quadratic                     | Linear (after vsat)               |
| No. of Regions    | 3                             | 4                                  |
| Dependence on L   | `I_D ∝ 1 / L`                 | Nearly independent of `L`         |
| Control           | Voltage-controlled            | Field-controlled                  |
| Current Trend     | Gradual quadratic rise        | Early saturation → linear trend   |

---

## 🧪 Lab 1: ID–VDS Characteristics

### Objective

To observe drain current `I_D` variation with drain-to-source voltage `V_DS` for an NMOS transistor.

### Procedure

```bash
vim day2_nfet_idvds_L015_w039.spice
ngspice day2_nfet_idvds_L015_w039.spice
plot -vdd#branch
````

![image alt](https://github.com/harishj123/RISC-V_Soc_Tape_out_week_4/blob/main/week_4/images_Day2/ngspice.png?raw=true)


### Observation

* The `I_D` vs `V_DS` curve exhibits **cut-off**, **linear (triode)**, and **saturation** regions.
* For short-channel devices, **velocity saturation** limits current increase at higher `V_DS`, causing earlier flattening of the curve.

---

## 🧪 Lab 2: ID–VGS Characteristics

### Objective

To analyze drain current `I_D` variation with gate-to-source voltage `V_GS`.

### Procedure

```bash
vim day2_nfet_idvgs_L015_w039.spice
ngspice day2_nfet_idvgs_L015_w039.spice
plot -vdd#branch
```
![image alt](https://github.com/harishj123/RISC-V_Soc_Tape_out_week_4/blob/main/week_4/images_Day2/ngspice%20id%20vs%20vgs.png?raw=true)

### Observation

* `I_D` increases with `V_GS`, then **saturates** as carrier velocity approaches `v_sat`.
* Threshold voltage `V_TH` (the point where conduction starts) is around `0.7 V` in the simulated device.
* The curve shows a transition from nonlinear (quadratic) to linear behavior — illustrating short-channel effects.

---

## 🧠 4. CMOS Voltage Transfer Characteristics (VTC)

### Definition

The Voltage Transfer Characteristic (VTC) of a CMOS inverter shows `V_out` as a function of `V_in`. It indicates switching behavior, noise margins and gain.

---

### 4.1 Logic Operation Summary

| Input `V_in` | NMOS | PMOS | Output `V_out` | Logic |
| :----------- | :--: | :--: | :------------: | :---: |
| `0 V`        |  OFF |  ON  |     `V_DD`     |  HIGH |
| `V_DD`       |  ON  |  OFF |      `0 V`     |  LOW  |

---

### 4.2 Switching and Noise Margins

* **Switching point (`V_m`)**: input voltage where `V_in = V_out`.
* Noise margins: `NMH = V_OH - V_IH` and `NML = V_IL - V_OL`.
* A steeper VTC slope in the transition region yields higher gain and better noise immunity.

---

### 4.3 Derivation Steps for VTC (conceptual)

1. PMOS gate-source relation: `V_SG,P = V_DD - V_in`
2. NMOS drain-source: `V_DS,N = V_out`
3. PMOS drain-source: `V_SD,P = V_DD - V_out`
4. For each `V_in`, compute `I_D`(NMOS) vs `V_out` and `I_D`(PMOS) vs `V_out`.
5. The intersection of the two `I_D` curves gives the operating `V_out` for that `V_in`.
6. Repeat across the `V_in` range and join intersection points → VTC curve.

---

### 4.4 VTC Key Features

| Feature                 | Description                             |              |                            |
| :---------------------- | :-------------------------------------- | ------------ | -------------------------- |
| Logic HIGH Output       | `V_out ≈ V_DD` when `V_in` is LOW       |              |                            |
| Logic LOW Output        | `V_out ≈ 0 V` when `V_in` is HIGH       |              |                            |
| Transition Region       | Region where both NMOS and PMOS conduct |              |                            |
| Switching Voltage `V_m` | Where `V_out = V_in`                    |              |                            |
| Noise Margins           | Define reliability against input noise  |              |                            |
| Gain                    | `                                       | dV_out/dV_in | ` in the transition region |

---

### 4.5 Graphical Behavior (summary)

* `V_in` low → PMOS ON → `V_out ≈ V_DD`
* `V_in` rising → NMOS turns on → `V_out` falls
* `V_in = V_m` → midpoint transition (both conduct)
* `V_in` high → NMOS ON, PMOS OFF → `V_out ≈ 0 V`

---

## 🔍 5. Key Design Insights

* Threshold voltage (`V_TH`) shifts change `V_m` and noise margins.
* Proper transistor sizing (`W_p / W_n`) yields symmetric VTC and balanced rise/fall strengths.
* A steeper VTC slope implies faster switching and better noise immunity.
* VTC analysis underpins logic family performance and static/dynamic behavior.

---

## ✅ Conclusion

In this session, we examined how **device physics** drives **circuit behavior**:

* **Long-channel MOSFETs** show `I_D ∝ (V_GS - V_TH)^2` (quadratic).
* **Short-channel MOSFETs** shift toward `I_D ∝ (V_GS - V_TH)` (linear) due to **velocity saturation**.
* These device-level effects directly affect **CMOS inverter VTC**, switching point, gain, and noise margins.

NGSpice simulations help visualize `I_D` vs `V_DS` and `I_D` vs `V_GS` and confirm theoretical expectations — a necessary step in designing high-speed, low-power CMOS circuits.

---
