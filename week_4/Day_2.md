
---

# 🧠 **Day 2 – Week 4: MOSFET Characteristics and CMOS Voltage Transfer Analysis**

---

## 🌟 **Welcome to Day 2!**

Welcome to **Day 2 of Week 4**, where we dive deeper into the fascinating world of **MOSFET behavior** and **digital switching analysis**.

Today’s exploration bridges the **analog characteristics** of MOS transistors with the **digital logic behavior** of CMOS inverters. You’ll uncover how channel dimensions influence transistor current flow, and how these physical properties ultimately shape the **voltage transfer characteristics (VTC)** of a CMOS circuit — the heart of every digital logic gate.

---

## ⚙️ **Overview**

This session focuses on understanding:

* The difference between **long-channel** and **short-channel** MOSFETs.
* The impact of **scaling and velocity saturation** on current flow.
* How **CMOS inverters** convert analog input transitions into crisp digital outputs.

By the end of this lab, you’ll clearly visualize how the **microscopic physics of transistors** connect to **macroscopic circuit-level logic behavior**.

---

## 🧩 **1. Channel Variations in MOSFETs**

MOSFETs operate differently depending on their **channel length (L)**. The channel acts as a highway for charge carriers — and when it’s shortened, traffic rules change dramatically.

* **Long Channel MOSFET** – Classic operation with linear carrier velocity.
* **Short Channel MOSFET** – Advanced operation where carrier velocity saturates due to high electric fields.

---

## **2. Long Channel MOSFET**

### 🧠 Concept:

In long-channel MOSFETs, the **carrier velocity (v)** increases **linearly** with the electric field (E):

[
v = μ_n E
]

Thus, the **drain current (ID)** depends **quadratically** on the gate voltage ((V_{GS} - V_{TH})).

---

### **2.1 Drain Current Model (Saturation Region)**

[
I_D = \frac{1}{2} μ_n C_{ox} \frac{W}{L} (V_{GS} - V_{TH})^2
]

This relationship shows **quadratic control**, meaning a small increase in gate voltage causes a significant rise in drain current — ideal for analog amplification.

---

### **2.2 Regions of Operation**

| Region              | Condition                  | Current Equation                                                             | Behavior          |
| :------------------ | :------------------------- | :--------------------------------------------------------------------------- | :---------------- |
| **Cut-off**         | (V_{GS} < V_{TH})          | (I_D = 0)                                                                    | No conduction     |
| **Linear / Triode** | (V_{DS} < V_{GS} - V_{TH}) | (I_D = μ_n C_{ox} \frac{W}{L}[(V_{GS} - V_{TH})V_{DS} - \frac{V_{DS}^2}{2}]) | Ohmic region      |
| **Saturation**      | (V_{DS} ≥ V_{GS} - V_{TH}) | (I_D = \frac{1}{2} μ_n C_{ox} \frac{W}{L}(V_{GS} - V_{TH})^2)                | Current saturates |

---

## **3. Short Channel MOSFET**

As devices shrink (L < 0.1 µm), the **electric field** inside the channel becomes intense, forcing the carrier velocity to **saturate at (v_{sat})**.
This transforms the current dependence from **quadratic** to **linear** with gate voltage.

[
v =
\begin{cases}
μ_n E, & E < E_{crit} \
v_{sat}, & E ≥ E_{crit}
\end{cases}
]

---

### **3.1 Drain Current (Velocity-Saturated Region)**

[
I_D = W C_{ox} (V_{GS} - V_{TH}) v_{sat}
]

Here, (I_D) grows linearly with ((V_{GS} - V_{TH})), indicating **velocity saturation** dominance.

---

### **3.2 Regions of Operation**

| Region                             | Condition                  | Behavior       |
| :--------------------------------- | :------------------------- | :------------- |
| **Cut-off**                        | (V_{GS} < V_{TH})          | Device OFF     |
| **Linear**                         | (V_{DS} < V_{GS} - V_{TH}) | Ohmic region   |
| **Saturation (Pre-velocity sat.)** | (V_{DS} = V_{GS} - V_{TH}) | Quadratic rise |
| **Velocity-Saturated**             | (V_{DS} > V_{DS,sat})      | Linear rise    |

---

### **3.3 Comparison Table**

| Property          | Long Channel       | Short Channel           |
| :---------------- | :----------------- | :---------------------- |
| Velocity Relation | (v ∝ E)            | (v = v_{sat})           |
| (I_D) vs (V_{GS}) | Quadratic          | Linear                  |
| No. of Regions    | 3                  | 4                       |
| Dependence on L   | (I_D ∝ 1/L)        | Nearly independent of L |
| Control           | Voltage-controlled | Field-controlled        |
| Current Trend     | Gradual rise       | Early saturation        |

---

## 🧪 **Lab 1: ID–VDS Characteristics**

### **Objective**

To observe **drain current (ID)** variation with **drain-to-source voltage (VDS)** in an NMOS device.

### **Procedure**

```bash
vim day2_nfet_idvds_L015_w039.spice
ngspice day2_nfet_idvds_L015_w039.spice
plot -vdd#branch
```

### **Observation**

* The curve exhibits **cut-off**, **linear**, and **saturation** regions.
* For short-channel devices, **velocity saturation** limits the current rise at higher (V_{DS}), causing earlier flattening of the curve.

---

## 🧪 **Lab 2: ID–VGS Characteristics**

### **Objective**

To analyze **drain current** variation with **gate-to-source voltage (VGS)**.

### **Procedure**

```bash
vim day2_nfet_idvgs_L015_w039.spice
ngspice day2_nfet_idvgs_L015_w039.spice
plot -vdd#branch
```

### **Observation**

* (I_D) increases with (V_{GS}), then **saturates** as carrier velocity reaches (v_{sat}).
* The **threshold voltage** ((V_{TH})) is around **0.7 V**, where conduction begins.
* The curve clearly shows a **nonlinear to linear transition**, illustrating the short-channel effect.

---

## 🧠 **4. CMOS Voltage Transfer Characteristics (VTC)**

### **Definition**

The **VTC** of a CMOS inverter defines the relationship between **input voltage (Vin)** and **output voltage (Vout)**.
It determines how efficiently the inverter transitions between logic ‘1’ and ‘0’.

---

### **4.1 Logic Operation Summary**

| Input (Vin) | NMOS | PMOS | Output (Vout) | Logic |
| :---------- | :--- | :--- | :------------ | :---- |
| 0 V         | OFF  | ON   | (V_{DD})      | HIGH  |
| (V_{DD})    | ON   | OFF  | 0 V           | LOW   |

---

### **4.2 Switching and Noise Margins**

* **Switching point (Vm):** Input voltage where (V_{in} = V_{out}).
* Defines **noise margins (NMH & NML)** for reliable logic operation.
* A **steeper slope** in the transition region ensures **sharper switching** and **higher noise immunity**.

---

### **4.3 Derivation Steps for VTC**

1. **PMOS relation:** (V_{SGP} = V_{DD} - V_{in})
2. **NMOS relation:** (V_{DSN} = V_{out})
3. **PMOS drain-source:** (V_{SDP} = V_{DD} - V_{out})
4. **Plot NMOS & PMOS current curves** and find intersection points.
5. **Join intersections** across all (V_{in}) → **VTC curve**.

---

### **4.4 VTC Key Features**

| Feature                | Description                             |
| :--------------------- | :-------------------------------------- |
| Logic HIGH Output      | (V_{out} = V_{DD}) when (V_{in}) is LOW |
| Logic LOW Output       | (V_{out} = 0V) when (V_{in}) is HIGH    |
| Transition Region      | Middle zone, both MOSFETs conduct       |
| Switching Voltage (Vm) | Where (V_{out} = V_{in})                |
| Noise Margins          | Define reliability against noise        |
| Gain                   | Maximum slope in transition region      |

---

### **4.5 Graphical Behavior**

* (V_{in}) low → PMOS ON → (V_{out} ≈ V_{DD})
* Increasing (V_{in}) → NMOS begins conducting → (V_{out}) drops
* (V_{in} = V_m) → Transition midpoint → both devices conduct
* (V_{in}) high → NMOS ON, PMOS OFF → (V_{out} ≈ 0V)

---

## 🔍 **5. Key Design Insights**

* **Threshold voltage** shifts impact switching voltage and noise margins.
* **Proper transistor sizing (Wp/Wn)** ensures a balanced and symmetric VTC.
* A **steeper VTC slope** implies **faster switching** and **better noise immunity**.
* VTC forms the **foundation for logic family performance** analysis in CMOS technology.

---

## ✅ **Conclusion**

In this session, we explored how **device physics meets digital design**.

* **Long-channel MOSFETs** exhibit **ideal quadratic behavior**, while **short-channel MOSFETs** transition to **linear current dependence** due to **velocity saturation**.
* These microscopic effects directly influence **CMOS inverter performance**, defining the **voltage transfer curve (VTC)**, **switching threshold**, and **noise tolerance**.

Through **NGSpice simulations**, we visualized how the **drain current** and **transfer characteristics** evolve with voltage — a critical foundation for designing **high-speed, low-power CMOS circuits**.

---
