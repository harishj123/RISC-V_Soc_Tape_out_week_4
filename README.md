# 📘 Week 4 – CMOS Circuit Design: MOSFET Characteristics & CMOS Inverter Analysis

This repository contains simulation files, theoretical notes, and visualizations from **Week 4, Day 1 and Day 2** of our CMOS Circuit Design Learning Series. The focus is on **MOSFET Id–Vds behavior**, **device scaling effects**, and **CMOS inverter voltage transfer characteristics (VTC)** using **Sky130 PDK** and **NGSpice**.

---

## 📅 Day 1 – Understanding MOSFET Operation (Id–Vds)

### 📌 Topics Covered:
- MOSFET structure and regions of operation (Cutoff, Linear, Saturation)
- Derivation of Id–Vds equations
- Threshold voltage & body effect
- SPICE simulation using Sky130 PDK
- NMOS transistor drain current characteristics

### 🔧 Files:
- `day1_nfet_idvds_L2_W5.spice` – SPICE netlist for Id–Vds sweep
- `images/` – Contains curves and diagrams

### 💡 Key Concepts:
- NMOS conducts when Vgs > Vt
- Id varies with Vds and Vgs
- Cutoff: Vgs < Vt → Id ≈ 0
- Linear: Vds < (Vgs − Vt)
- Saturation: Vds ≥ (Vgs − Vt)

---

![Day 1](https://github.com/harishj123/RISC-V_Soc_Tape_out_week_4/blob/main/week_4/Day_1.md)



## 📅 Day 2 – MOSFET Scaling and CMOS Inverter VTC

### 📌 Topics Covered:
- Long vs short channel MOSFETs
- Velocity saturation effect in short-channel devices
- I_D vs V_GS and I_D vs V_DS characteristics
- CMOS inverter VTC (Voltage Transfer Curve)
- Noise margins and switching behavior

### 🔧 Files:
- `day2_nfet_idvds_L015_W039.spice` – Short channel Id–Vds simulation
- `day2_nfet_idvgs_L015_W039.spice` – Id–Vgs sweep
- `images_Day2/` – Simulation results and VTC plots

### 💡 Key Concepts:
- Long-channel: Id ∝ (Vgs − Vth)²
- Short-channel: Id ∝ (Vgs − Vth) due to velocity saturation
- CMOS inverter VTC shows transition from logic high to low
- Noise margins determine circuit robustness

---

![Day 2](https://github.com/harishj123/RISC-V_Soc_Tape_out_week_4/blob/main/week_4/Day_2.md)


## 🛠️ Tools & Requirements

- [Sky130 PDK](https://github.com/google/skywater-pdk)
- [Ngspice](http://ngspice.sourceforge.net/)
- Git, Vim (for netlist editing)

---

## 🧪 How to Run Simulations

```bash
# Clone the repo
git clone https://github.com/yourname/week4_cmos_mosfet_analysis.git
cd week4_cmos_mosfet_analysis

# Run Day 1 simulation
ngspice day1_nfet_idvds_L2_W5.spice

# Run Day 2 simulations
ngspice day2_nfet_idvds_L015_W039.spice
ngspice day2_nfet_idvgs_L015_W039.spice
