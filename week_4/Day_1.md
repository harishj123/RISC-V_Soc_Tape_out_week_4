
# 🌟 Welcome to Week 4 – Day 1  
## 🚀 Exploring the Basics of MOSFET Operation: *Drain Current vs Drain-to-Source Voltage (Id–Vds)*  

Welcome to **Week 4, Day 1** of our circuit-design learning series!  
Today we’ll explore one of the most fascinating fundamentals of semiconductor devices — the **relationship between drain current (Id)** and **drain-to-source voltage (Vds)** in MOSFETs.  

Through both **theory** and **SPICE simulation**, we’ll uncover how an NMOS transistor transitions through its three operating regions — **cutoff**, **linear**, and **saturation**.  
By the end of this lab you’ll know how to interpret Id–Vds curves, understand threshold behavior, and perform your own simulations using the **Sky130 PDK**.  

Let’s dive in ⚡  

Perfect — you’re absolutely right 👍

GitHub **doesn’t render LaTeX-style inline equations (`[ ... ]` or `\(...\)`)** like in docs or PDFs.
Instead, it supports code formatting and HTML-style math using backticks and Unicode subscripts/superscripts.

Below is your **fully GitHub-rendered and readable version** — all formulas rewritten to display perfectly on GitHub Markdown (no LaTeX required).
You can copy-paste this as your GitHub file directly (`README.md` or report file).

---

```markdown
# 🌟 **WEEK 4 – DAY 1**

## **Topic:** Basics of Drain Current vs Drain-to-Source Voltage (Id–Vds) Characteristics

---

### 🎯 **Objective**

To understand the drain current characteristics of NMOS and PMOS transistors, their regions of operation, and simulate them using **SPICE**.

---

## ⚙️ **1. Circuit Design and SPICE Simulation**

All MOS circuits are built using **PMOS** and **NMOS** transistors.

An **inverter** is the simplest CMOS circuit consisting of:
- One **PMOS** (pull-up)
- One **NMOS** (pull-down)

The operation of these MOSFETs depends on:
- **Drain current (Id)**
- **Gate-to-Source voltage (Vgs)**
- **Drain-to-Source voltage (Vds)**

---

## ⚡ **2. NMOS and PMOS Characteristics**

| Parameter | NMOS | PMOS |
|------------|-------|------|
| Channel type | n-type | p-type |
| Carrier type | Electrons | Holes |
| Active when | Vgs > Vt | Vgs < Vt |
| Symbol | ![NMOS Symbol](https://i.imgur.com/XpjI3Jm.png) | ![PMOS Symbol](https://i.imgur.com/5sS7iXo.png) |

- **NMOS** conducts when **Vgs > Vt**  
- **PMOS** conducts when **Vgs < Vt**

---

## 🧩 **3. Why We Need SPICE**

**SPICE (Simulation Program with Integrated Circuit Emphasis)** is used to:

- Model transistor behavior accurately.
- Observe current–voltage characteristics (Id–Vds, Id–Vgs).
- Estimate **delay, rise time, and fall time** using buffer delay tables.
- Predict circuit behavior before fabrication.

---

## 🔬 **4. NMOS Structure and Threshold Voltage**

An **NMOS** has:
- Source (S), Drain (D), Gate (G), and Body (B)
- p-type substrate with n⁺ source and drain regions

When **Vgs = 0**, both junctions are **reverse-biased** (no conduction).  
As **Vgs** increases, electrons are attracted to the gate region, forming an **inversion channel**.

---

### 🧮 **Threshold Voltage (Vt)**

When **Vgs = Vt**, a thin electron channel forms under the gate — conduction starts.  
When **Vgs > Vt**, the channel strengthens, increasing **Id**.

**Cases:**
1. **VSB = 0** → no body effect  
2. **VSB > 0** → threshold voltage increases due to body effect

**Expression:**
```

Vt = Vto + γ [ √(2|φf| + VSB) - √(2|φf|) ]

```

Where:
- Vto → Threshold voltage at zero body bias  
- γ (gamma) → Body effect coefficient  
```

γ = √(2 * q * ε_si * NA) / Cox

```
- φf (Fermi potential) → (kT/q) * ln(NA / ni)  
- Cox → Gate oxide capacitance per unit area = ε_ox / t_ox

---

## 🔋 **5. Induced Charge and Drain Current**

For NMOS:
```

Qi(x) = -Cox * [ (Vgs - V(x)) - Vt ]

```

Drift velocity:
```

vn(x) = μn * E = -μn * (dV/dx)

```

Drain current:
```

Id = -Qi(x) * W * vn(x)

```

Substitute and integrate over the channel:
```

Id = μn * Cox * (W / L) * [ (Vgs - Vt) * Vds - (Vds² / 2) ]

```

---

### 🟢 **Linear (Resistive) Region**
Condition:
```

Vds < (Vgs - Vt)

```

Equation:
```

Id ≈ Kn * (Vgs - Vt) * Vds

```
Where:
```

Kn = μn * Cox * (W / L)

```

---

### 🔴 **Saturation Region**
Condition:
```

Vds ≥ (Vgs - Vt)

```

Equation:
```

Id = (1/2) * Kn * (Vgs - Vt)²

````

At this point, **channel pinch-off** occurs near the drain.

---

## ⚗️ **6. Example Simulation Setup**

| Parameter | Value |
|------------|--------|
| Threshold Voltage (Vt) | 0.5 V |
| Vgs Sweep | 0.5 V → 2.5 V |
| Vds Sweep | 0 V → 1.05 V |

**Observation:**
- Vgs < 0.5 V → transistor off (Id ≈ 0)  
- Vgs = 1 V → small Id (linear region)  
- Vgs = 2 V or 2.5 V → high Id (saturation region)

---

## 💻 **7. SPICE Simulation Steps**

### **Model Parameters**
Include:
- `VTO` → Threshold voltage  
- `GAMMA` → Body effect constant  
- `LAMBDA` → Channel-length modulation  
- `KP` or `KN_DASH` → Transconductance parameter  

---

### **SPICE Commands**
```bash
cd
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
cd sky130CircuitDesignWorkshop/
cd design/sky130_fd_pr/cells/nfet_01v8/
less sky130_fd_pr__nfet_01v8__tt.pm3.spice
less sky130_fd_pr__nfet_01v8__tt.corner.spice
cd ../../
vim day1_nfet_idvds_L2_W5.spice
````

---

### **SPICE Netlist Example**

```spice
* NMOS ID-VDS Characteristics
M1 drain gate source source sky130_fd_pr__nfet_01v8 W=5u L=2u
VGS gate source 0
VDS drain source 0
.dc VDS 0 1.05 0.05 VGS 0.5 2.5 0.5
.plot DC I(M1)
.end
```

---

## 📈 **8. Id–Vds Graph Interpretation**

* **X-axis:** Vds
* **Y-axis:** Id
* Each curve represents a different Vgs.
* Low Vds → Linear region
* High Vds → Saturation region

**Knee Point:**
Occurs where `Vds = Vgs - Vt`.

---

## 🧠 **9. Regions of Operation**

| Region                 | Condition        | Description                           |
| ---------------------- | ---------------- | ------------------------------------- |
| **Cut-off**            | Vgs < Vt         | No channel formed, Id ≈ 0             |
| **Linear (Resistive)** | Vds < (Vgs − Vt) | Channel formed, Id increases linearly |
| **Saturation**         | Vds ≥ (Vgs − Vt) | Channel pinches off, Id saturates     |

---

## ✅ **10. Conclusion**

* The **Id–Vds** curves show **three regions** of MOSFET operation: Cutoff, Linear, and Saturation.
* As **Vgs** increases, more electrons form the channel → **Id increases**.
* **SPICE simulation** validates theoretical behavior.
* Understanding these characteristics is essential for **CMOS logic** and **analog design**.

---
