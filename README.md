# RISC-V-SoC-Tapeout-Program-Week4

---

# 🌟 Day 4: Transient Analysis and Delay Measurement (CMOS Inverter)

This section explains how to perform **transient analysis** using **Ngspice** on a CMOS inverter built with **Sky130 technology**, and how to calculate **rise and fall delays** from the waveform.

---

## ⚙️ SPICE Netlist

```
*Model Description
.param temp=27

*Including sky130 library files
.lib "sky130_fd_pr/models/sky130.lib.spice" tt

*Netlist Description

XM1 out in vdd vdd sky130_fd_pr__pfet_01v8 w=0.84 l=0.15
XM2 out in 0 0 sky130_fd_pr__nfet_01v8 w=0.36 l=0.15

Cload out 0 50fF

Vdd vdd 0 1.8V
Vin in 0 PULSE(0V 1.8V 0 0.1ns 0.1ns 2ns 4ns)

*Simulation Commands
.tran 1n 10n

.control
run
plot Vin V(out)
.endc

.end
```

---

🧩 Description

This transient simulation is used to observe the dynamic switching behavior of the CMOS inverter.

Parameter	Description

Vdd	1.8 V supply voltage
Vin	Input pulse toggling between 0 V and 1.8 V
Period	4 ns (2 ns high + 2 ns low)
Rise/Fall time	0.1 ns
Cload	50 fF load capacitor at output
Corner	tt (typical process corner)



---

🧠 Theory

A CMOS inverter works as follows:

When Vin = 0V → PMOS ON, NMOS OFF → Vout = Vdd

When Vin = Vdd → PMOS OFF, NMOS ON → Vout = 0V


During switching, there is a time delay between input change and output response.
This delay depends on transistor size, load capacitance, and process speed.


---

⏱️ Delay Definitions

Symbol	Description	Event

tPHL	Propagation delay (High → Low)	Output falls after input rises
tPLH	Propagation delay (Low → High)	Output rises after input falls


These delays are measured at Vdd/2 (50% of the supply voltage).


---

📈 Delay Measurement Procedure

1. Run the transient simulation (run)


2. Plot Vin and Vout waveforms.


3. Find the time points where both Vin and Vout cross Vdd/2 (≈ 0.9V).


4. Note down:

t_in_rise → Time when Vin rises past 0.9V

t_out_fall → Time when Vout falls past 0.9V

t_in_fall → Time when Vin falls past 0.9V

t_out_rise → Time when Vout rises past 0.9V



5. Compute delays using:

tPHL = t_out_fall - t_in_rise
tPLH = t_out_rise - t_in_fall




---

🧮 Example Calculation

Measurement	Time (ns)

t_in_rise	2.00
t_out_fall	2.12
t_in_fall	4.00
t_out_rise	4.14


Calculated Delays:

tPHL = 2.12 - 2.00 = 0.12 ns
tPLH = 4.14 - 4.00 = 0.14 ns

> ⚡ Observation:
PMOS is slower (μp < μn), so tPLH > tPHL.
To balance rise/fall delays, designers increase PMOS width.




---

🧾 Notes

Increase transistor width → reduces delay

Reduce load capacitance → reduces delay

Use faster process corner (ff) → reduces delay

Typical range: Delay ≈ 0.1–0.2 ns for sky130 inverter



---

📊 Summary Table

Parameter	Description	Typical Value

Vdd	Supply Voltage	1.8 V
Vm	Switching Threshold (Vout = Vin)	~0.9 V
tPHL	High → Low Delay	~0.12 ns
tPLH	Low → High Delay	~0.14 ns
Cload	Load Capacitance	50 fF
Process Corner	Typical (tt)	—



---

🧠 Understanding the Waveform

Vin (Input) → Square wave toggling between 0 and 1.8 V

Vout (Output) → Inverted waveform, delayed slightly

Delay is measured between 50% points of input and output transitions



---

🧰 Improving Delay Performance

Increase PMOS width (W/L) to balance rise and fall times

Minimize output load capacitance

Use higher supply voltage (within limits)

Use fast process corner (ff)



---

✅ Conclusion

Transient analysis helps evaluate the dynamic performance of CMOS logic circuits.
By analyzing rise and fall delays, designers can optimize inverter design for speed, balance, and power efficiency in Sky130 technology.


---
