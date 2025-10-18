Excellent ✅ — here’s your **fully GitHub-formatted Markdown (`README.md`)** version, written exactly the way it should appear on your repository page — with emojis, headings, LaTeX-friendly math, syntax-highlighted code, and perfectly clean Markdown spacing.

You can directly copy-paste this file as
`Week4/Day1_Basics_IdVds/README.md`
in your GitHub project.

---

````markdown
# 🌟 Welcome to Week 4 – Day 1  
## 🚀 Exploring the Basics of MOSFET Operation: *Drain Current vs Drain-to-Source Voltage (Id–Vds)*  

Welcome to **Week 4, Day 1** of our circuit-design learning series!  
Today we’ll explore one of the most fascinating fundamentals of semiconductor devices — the **relationship between drain current (Id)** and **drain-to-source voltage (Vds)** in MOSFETs.  

Through both **theory** and **SPICE simulation**, we’ll uncover how an NMOS transistor transitions through its three operating regions — **cutoff**, **linear**, and **saturation**.  
By the end of this lab you’ll know how to interpret Id–Vds curves, understand threshold behavior, and perform your own simulations using the **Sky130 PDK**.  

Let’s dive in ⚡  

---

## 🎯 Objective  

To study and simulate the **Id–Vds characteristics** of NMOS and PMOS transistors, analyze their operating regions, and verify theoretical behavior using **SPICE**.

---

## ⚙️ 1️⃣ Circuit Design and SPICE Overview  

All MOS circuits are made using **PMOS** and **NMOS** transistors.  
The simplest CMOS circuit — the **inverter** — contains:

- **PMOS** → pull-up device  
- **NMOS** → pull-down device  

SPICE helps us visualize how these transistors behave for different gate and drain voltages.

---

## 🔋 2️⃣ NMOS and PMOS Characteristics  

| Parameter | NMOS | PMOS |
|------------|-------|-------|
| Channel type | n-type | p-type |
| Carrier type | Electrons | Holes |
| Active when | V<sub>GS</sub> > V<sub>T</sub> | V<sub>GS</sub> < V<sub>T</sub> |
| Role in CMOS inverter | Pull-down | Pull-up |

---

## 💡 3️⃣ Why We Need SPICE  

SPICE (**Simulation Program with Integrated Circuit Emphasis**) enables engineers to:

- Analyze transistor behavior **before fabrication**  
- Plot **Id–Vds** and **Id–Vgs** curves  
- Estimate **delay and switching times**  
- Validate **buffer delay tables** and **inverter performance**

---

## 🔬 4️⃣ NMOS Structure and Threshold Voltage  

An **NMOS** has a p-type substrate with two n⁺ diffusions (source & drain).  
At **V<sub>GS</sub> = 0 V**, both junctions are reverse-biased → no conduction.  
Increasing **V<sub>GS</sub>** attracts electrons under the gate oxide, forming an **inversion channel**.

### ⚙️ Threshold Voltage Expression  

\[
V_T = V_{TO} + \gamma \left(\sqrt{2|\phi_f| + V_{SB}} - \sqrt{2|\phi_f|}\right)
\]

| Symbol | Description |
|:--|:--|
| **VTO** | Threshold voltage at zero body bias |
| **γ (Gamma)** | Body-effect coefficient |
| **φ<sub>f</sub> (Fermi potential)** | \( \frac{kT}{q}\ln\left(\frac{N_A}{n_i}\right) \) |
| **C<sub>ox</sub>** | Gate-oxide capacitance = \( \frac{ε_{ox}}{t_{ox}} \) |

---

## 📈 5️⃣ Induced Charge and Drain Current Derivation  

\[
Q_i(x) = -C_{ox}\big[(V_{GS}-V(x))-V_T\big]
\]

Drift velocity:  
\[
v_n(x) = μ_n E = -μ_n \frac{dV}{dx}
\]

Channel current:  
\[
I_D = -W Q_i(x) v_n(x)
\]

Integrating along the channel:  
\[
I_D = μ_n C_{ox} \frac{W}{L}\Big[(V_{GS}-V_T)V_{DS} - \frac{V_{DS}^2}{2}\Big]
\]

---

### 🔹 Linear (Resistive) Region  

When \( V_{DS} < (V_{GS}-V_T) \)  

\[
I_D = K_n (V_{GS}-V_T) V_{DS}
\]

where \( K_n = μ_n C_{ox} \frac{W}{L} \)

### 🔸 Saturation Region  

When \( V_{DS} ≥ (V_{GS}-V_T) \)  

\[
I_D = \frac{1}{2} K_n (V_{GS}-V_T)^2
\]

---

## 🧮 6️⃣ Example Simulation Setup  

| Parameter | Value |
|------------|--------|
| Threshold Voltage (Vt) | 0.5 V |
| V<sub>GS</sub> Sweep | 0.5 V → 2.5 V |
| V<sub>DS</sub> Sweep | 0 V → 1.05 V |

**Expected Results 📊**  
- For V<sub>GS</sub> &lt; 0.5 V → transistor OFF  
- As V<sub>GS</sub> increases → Id rises linearly  
- Beyond V<sub>GS</sub> ≈ 2 V → early saturation  

---

## 🧰 7️⃣ SPICE Simulation Steps  

```bash
cd
git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop.git
cd sky130CircuitDesignWorkshop/design/sky130_fd_pr/cells/nfet_01v8/
less sky130_fd_pr__nfet_01v8__tt.pm3.spice
less sky130_fd_pr__nfet_01v8__tt.corner.spice
cd ../../
vim day1_nfet_idvds_L2_W5.spice
````

---

### 🧾 SPICE Netlist Example

```spice
* NMOS ID–VDS Characteristics
M1 drain gate source source sky130_fd_pr__nfet_01v8 W=5u L=2u
VGS gate source 0
VDS drain source 0
.dc VDS 0 1.05 0.05 VGS 0.5 2.5 0.5
.plot DC I(M1)
.end
```

---

## 📊 8️⃣ Id–Vds Graph Interpretation

* **X-axis :** V<sub>DS</sub>
* **Y-axis :** I<sub>D</sub>
* Each curve → fixed V<sub>GS</sub>
* **Knee point :** (V_{DS}=V_{GS}-V_T)
* Beyond knee → **saturation region**

---

## ⚡ 9️⃣ Regions of Operation

1. **Cut-off** → V<sub>GS</sub> < V<sub>T</sub>, no conduction
2. **Linear / Resistive** → V<sub>DS</sub> < (V<sub>GS</sub> − V<sub>T</sub>)
3. **Saturation** → V<sub>DS</sub> ≥ (V<sub>GS</sub> − V<sub>T</sub>) → current saturates

---

## 🧾 🔟 SPICE Model Parameters

| Parameter        | Meaning                    |
| ---------------- | -------------------------- |
| **VTO**          | Threshold voltage          |
| **γ (Gamma)**    | Body-effect constant       |
| **λ (Lambda)**   | Channel-length modulation  |
| **KP / KN_DASH** | Transconductance parameter |

> **SPICE Model Parameters + SPICE Netlist = SPICE Simulation Setup**

---

## 🧭 1️⃣1️⃣ Resistive Operation of MOSFET

In the **linear region**, the MOSFET behaves as a **voltage-controlled resistor**.
Current varies approximately linearly with V<sub>DS</sub>.
This region is widely used in **analog applications** such as variable-resistor circuits and amplifiers.

---

## 🧩 1️⃣2️⃣ Conclusion

* The **Id–Vds** plots clearly show the transition between **cutoff**, **linear**, and **saturation** regions.
* As **V<sub>GS</sub>** rises, the inversion channel strengthens → increased Id.
* **SPICE simulations** verify theoretical behavior and provide insight into device performance.
* This knowledge forms the foundation for **CMOS logic** and **analog design**.

---

## 📁 Recommended Repository Structure

```
Week4/
 └── Day1_Basics_IdVds/
      ├── README.md                 ← this file
      ├── day1_nfet_idvds_L2_W5.spice
      └── simulation_results.png    ← Id–Vds plot
```

---

🌱 *End of Day 1 — Understanding NMOS Id–Vds Characteristics through SPICE Simulation*

```

---

Would you like me to include a ready-made **Id–Vds plot (simulation_results.png)** to match this README for your GitHub folder? I can generate a clean graph showing curves for multiple V<sub>GS</sub> values (0.5 V–2.5 V).
```
