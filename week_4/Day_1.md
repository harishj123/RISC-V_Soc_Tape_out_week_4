
# 🌟 Welcome to Week 4 – Day 1  
## 🚀 Exploring the Basics of MOSFET Operation: *Drain Current vs Drain-to-Source Voltage (Id–Vds)*  

Welcome to **Week 4, Day 1** of our circuit-design learning series!  
Today we’ll explore one of the most fascinating fundamentals of semiconductor devices — the **relationship between drain current (Id)** and **drain-to-source voltage (Vds)** in MOSFETs.  

Through both **theory** and **SPICE simulation**, we’ll uncover how an NMOS transistor transitions through its three operating regions — **cutoff**, **linear**, and **saturation**.  
By the end of this lab you’ll know how to interpret Id–Vds curves, understand threshold behavior, and perform your own simulations using the **Sky130 PDK**.  

Let’s dive in ⚡  

## **WEEK 4 – DAY 1**

### **Topic:** Basics of Drain Current vs Drain-to-Source Voltage (Id–Vds) Characteristics

### **Objective:**

To understand the drain current characteristics of NMOS and PMOS transistors, their regions of operation, and simulate them using **SPICE**.

---

## **1. Circuit Design and SPICE Simulation**

All MOS circuits are built using **PMOS** and **NMOS** transistors.
An **inverter** is the simplest CMOS circuit consisting of:

* One **PMOS** (pull-up)
* One **NMOS** (pull-down)

The operation of these MOSFETs depends on the **Drain current (Id)**, **Gate-to-Source voltage (Vgs)**, and **Drain-to-Source voltage (Vds)**.

---

## **2. NMOS and PMOS Characteristics**

| Parameter    | NMOS                                            | PMOS                                            |
| ------------ | ----------------------------------------------- | ----------------------------------------------- |
| Channel type | n-type                                          | p-type                                          |
| Carrier type | Electrons                                       | Holes                                           |
| Active when  | Vgs > Vt                                        | Vgs < Vt                                        |
| Symbol       | ![NMOS Symbol](https://i.imgur.com/XpjI3Jm.png) | ![PMOS Symbol](https://i.imgur.com/5sS7iXo.png) |

* **NMOS** conducts when **Vgs > Vt**
* **PMOS** conducts when **Vgs < Vt**

---

## **3. Why We Need SPICE**

**SPICE (Simulation Program with Integrated Circuit Emphasis)** is used to:

* Model transistor behavior accurately.
* Observe current–voltage characteristics (Id–Vds, Id–Vgs).
* Estimate **delay, rise time, and fall time** using buffer delay tables.
* Predict circuit behavior before fabrication.

Example:
For an inverter, SPICE helps plot how output voltage changes with varying input, showing switching delay and threshold.

---

## **4. NMOS Structure and Threshold Voltage**

An **NMOS** has:

* Source (S), Drain (D), Gate (G), and Body (B).
* The substrate is **p-type**, with **n+** source and drain diffusions.

At **Vgs = 0**, both junctions are **reverse-biased**, and resistance between S and D is very high.

When **Vgs increases**, electrons are attracted under the gate, forming an **inversion channel**.

---

### **Threshold Voltage (Vt)**

When **Vgs = Vt**, a thin channel of electrons forms at the surface → conduction starts.

When **Vgs > Vt**, the channel becomes stronger → higher drain current.

Two cases:

1. **VSB = 0** → no body bias (normal case).
2. **VSB > 0** → body effect increases Vt.

---

### **Threshold Voltage Expression**

[
V_t = V_{to} + \gamma \left(\sqrt{2|\phi_f| + V_{SB}} - \sqrt{2|\phi_f|}\right)
]

Where:

* **Vto** = Threshold voltage at zero body bias
* **γ (gamma)** = Body effect coefficient
  [
  \gamma = \frac{\sqrt{2q \varepsilon_{si} N_A}}{C_{ox}}
  ]
* **φf (Fermi potential)** = (\frac{kT}{q} \ln\left(\frac{N_A}{n_i}\right))
* **Cox** = Gate oxide capacitance per unit area = (\frac{\varepsilon_{ox}}{t_{ox}})

---

## **5. Induced Charge and Drain Current**

For NMOS:
[
Q_i = -C_{ox}[(V_{gs} - V(x)) - V_t]
]

At any point x along the channel:

* (Q_i(x)) is the inversion charge density.
* (E = -\frac{dV}{dx})
* (v_n(x) = \mu_n E = -\mu_n \frac{dV}{dx})

Drift current:
[
I_d = -Q_i(x) W v_n(x)
]

Substituting and integrating:
[
I_d = \mu_n C_{ox} \frac{W}{L} [(V_{gs} - V_t)V_{ds} - \frac{V_{ds}^2}{2}]
]

---

### **Linear (Resistive) Region**

When (V_{ds} < (V_{gs} - V_t)),

[
I_d \approx K_n (V_{gs} - V_t) V_{ds}
]
where (K_n = \mu_n C_{ox} \frac{W}{L})

---

### **Saturation Region**

When (V_{ds} \geq (V_{gs} - V_t)),
[
I_d = \frac{1}{2} K_n (V_{gs} - V_t)^2
]

At saturation, **channel pinch-off** occurs near the drain.

---

## **6. Example Simulation Setup**

### **Given:**

* (V_t = 0.5V)
* Sweep (V_{gs}) = 0.5V, 1V, 1.5V, 2V, 2.5V
* Sweep (V_{ds}) from 0 → 1.05V

You’ll observe:

* When (V_{gs} < 0.5V): transistor off → Id ≈ 0
* When (V_{gs} = 1V): small Id → linear region
* When (V_{gs} = 2V, 2.5V): high Id → saturation region starts earlier

---

## **7. SPICE Simulation Steps**

### **SPICE Model Parameters**

Include:

* `VTO` → Threshold voltage
* `GAMMA` → Body effect constant
* `LAMBDA` → Channel-length modulation
* `KP` or `KN_DASH` → Transconductance parameter

### **SPICE Setup**

```bash
cd
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
cd sky130CircuitDesignWorkshop/
cd design/sky130_fd_pr/cells/nfet_01v8/
less sky130_fd_pr__nfet_01v8__tt.pm3.spice
less sky130_fd_pr__nfet_01v8__tt.corner.spice
cd ../../
vim day1_nfet_idvds_L2_W5.spice
```

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

## **8. Id vs Vds Graph**

* **X-axis**: Vds
* **Y-axis**: Id
* For each Vgs, you’ll get a separate curve.
* At low Vds → linear (resistive) region.
* At high Vds → current saturates.

The **knee point** on each curve corresponds to (V_{ds} = V_{gs} - V_t).

---

## **9. Conclusion**

* The **Id–Vds** characteristics show three regions of NMOS operation:

  1. **Cutoff:** (V_{gs} < V_t), no current.
  2. **Linear (Resistive):** (V_{ds} < (V_{gs} - V_t)), current increases linearly.
  3. **Saturation:** (V_{ds} ≥ (V_{gs} - V_t)), current saturates.

* **SPICE** helps visualize these regions, validate theory, and understand transistor behavior before hardware implementation.

---

Would you like me to make this **as a formatted lab report (with equations and SPICE output placeholders)** — e.g. in a PDF-ready format for submission?
