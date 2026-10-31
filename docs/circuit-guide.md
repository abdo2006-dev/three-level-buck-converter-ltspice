# Circuit and experiment guide

Abdulrahman Ahmed Mohammed

## Design and control architecture

The nominal design is a 66 V input, 32 V output, 500 W three-level buck converter. A 2.048 Ω load draws 15.625 A at the target output. The power stage has a 20 µF flying capacitor, 1.36 µH output inductor and 500 µF output capacitor. Flying-capacitor ESR is 3 mΩ; output-capacitor ESR is normally 13 mΩ.

Four switching positions contain eight MOSFET models: two parallel BSC034N10LS5 devices in each 100 V A bank and two parallel BSC027N06LS5 devices in each 60 V B bank. Each parallel device sees its bank's voltage. The manufacturer-model case-temperature boundary is fixed at 25 °C.

The flying capacitor creates intermediate switching levels `VIN − VFly` and `VFly`. They coincide when `VFly = VIN/2`. An imbalance changes both the voltage applied to the inductor and the voltage blocked by the switches.

Loop A uses `eA = V(VIN)/2 − V(FLYP,FLYN)` to balance the flying capacitor. Loop B uses `eB = Vref − V(VOUT)` to regulate the output. RC-state compensators produce corrections CA and CB, followed by `DA = limit(32/VIN_parameter + CA, 0.02, 0.98)` and the corresponding expression for DB. During input application, that fixed parameter is different from instantaneous input-node voltage.

Complementary triangular carriers run at 100 kHz. Behavioral gate sources compare their carrier and duty command using a `tanh` transition scale of 0.001 and a 0.01 command gap. Gate voltages must be measured against each switch's source. The command gap describes the modeled nonoverlap; it does not establish practical driver timing. The fixed-PWM comparison uses PULSE sources instead.

## Signals to inspect

| Expression | Meaning and use |
|---|---|
| `V(VOUT)` | Output terminal voltage; use for regulation, ripple and load-response extrema |
| `V(FLYP,FLYN)` | Flying-capacitor terminal voltage, including ESR contribution; target is half the input |
| `I(L_o)` | Output-inductor current; positive in the saved device reference direction |
| `V(SW,VOUT)` | Voltage across the output inductor; integrate it to explain current change |
| `V(DA)`, `V(DB)` | Dimensionless duty commands represented by behavioral-source node voltages |
| `V(GHSA,FLYP)`, `V(GHSB,SW)` | Source-referenced upper-bank gate commands |
| `V(GLSA)`, `V(GLSB,FLYN)` | Source-referenced lower-bank gate commands |
| `V(VIN,FLYP)`, `V(FLYP,SW)`, `V(SW,FLYN)`, `V(FLYN)` | Drain-to-source voltages for QHSA, QHSB, QLSB and QLSA, respectively |

A saved plot setting selects expressions and axes. Its visible time range can be narrower than the data record, and its axes may clip peaks. Zoom out or autoscale when examining extrema; use the actual samples for numerical measurements.

## Schematic-by-schematic guide

### Fixed-PWM startup and switching states

`Open_Loop_Startup_and_Switch_Stress.asc`

This is the baseline power-stage experiment for examining switching levels, floating gate commands, flying-capacitor imbalance and switch blocking voltage without feedback correction. The schematic retains `.tran 0 2m 0 20n startup`; the report discusses the initial startup and the later 380–440 µs interval. A requested stop time is not evidence of settled operation.

The reported flying-capacitor peak is 67.547 V. Even over 380–440 µs, the output spans 31.575–32.426 V, so the reported response is still unsettled. Open the switching-state close-up to relate the switch node to source-referenced gate commands, and the MOSFET-stress view to inspect each bank's differential voltage. This establishes the baseline startup problem and the quantities used in later diagnosis.

### Startup with 13 mΩ output ESR

`Startup_with_Output_ESR_13_mOhm.asc`

This fixed-PWM startup case requests 450 µs with a 20 ns maximum step and the `startup` option. It provides the finite-ESR side of the output-capacitor damping comparison.

Over 0–450 µs, maximum inductor current is 391.734 A, minimum current is −133.529 A and maximum output voltage is 43.662 V. Finite ESR reduces the excursions relative to the zero-ESR case, but the remaining current is excessive. The simulation log contains tolerance-relaxation warnings; retain that qualification when interpreting its waveform.

### Startup with zero output ESR

`Startup_with_Zero_Output_ESR.asc`

This companion case sets output-capacitor ESR to zero and uses the same 450 µs observation request. Compare it directly with the 13 mΩ case to examine the influence of ESR on damping and terminal voltage.

Maximum inductor current is 432.963 A, minimum is −218.867 A and maximum output voltage is 46.679 V. Relative to 13 mΩ, the positive current peak increases by 41.229 A (10.5%), reverse-current magnitude by 85.338 A (63.9%) and output peak by 3.017 V. Zero ESR therefore worsens these recorded excursions. This log also contains tolerance-relaxation warnings.

### Feedback recovery from stored energy

`Closed_Loop_Prebiased_Recovery.asc`

This is the nominal 66 V feedback experiment and the controlled side of the fixed-PWM comparison. Both capacitors begin at 30 V, inductor current begins at 14.6484375 A, and controller states begin at zero. The transient uses `uic`, requests 1.5 ms and limits the timestep to 30 ns.

The initial response reaches 75.876 A inductor current, 35.520 V flying-terminal voltage and 32.926 V output. In the exact 1.45–1.50 ms window, output mean is 32.00053 V and output ripple is 54.823 mV peak to peak. Current mean is 15.62448 A with 4.4012 A ripple. Flying-terminal mean is 32.84464 V, 0.15536 V below its 33 V target, with 3.9122 V ripple.

This case demonstrates close late output regulation during prebiased recovery. The initial current excursion and the remaining flying-voltage ripple are part of the result. Use the full record to inspect recovery and the late window to reproduce the reported regulation measurements.

### Fixed PWM with matching storage initial conditions

`Fixed_PWM_Prebiased_Recovery.asc`

This is the open-loop counterpart to the nominal feedback recovery: 66 V input, 2.048 Ω load, 30 V on each capacitor, 14.6484375 A initial inductor current, `uic`, 1.5 ms stop time and 30 ns maximum step. Its pulse-generation implementation uses fixed PULSE sources; the feedback case uses triangular-carrier behavioral sources.

Over 1.45–1.50 ms, output mean is 32.18633 V, error is +0.18633 V and ripple is 82.82 mV peak to peak. Flying-terminal mean is 31.99919 V. Current mean is 15.71619 A and ripple is 6.4483 A. The full-record output peak is 33.39848 V and current peak is 47.9637 A.

Feedback reduces the late output error to +0.00053 V and lowers late output ripple, but its early current peak is higher. Because pulse generation also differs, this comparison evaluates the two saved implementations; it does not isolate feedback as the only changed variable.

### Initialized regulation at 48 V input

`Voltage_Regulation_48V_Input.asc`

This separate constant-input test runs for 3 ms with `uic` and a 30 ns maximum step. Inspect the final 2.95–3.00 ms window. The output starts at 32 V, the flying capacitor at 24 V and inductor current at 15.625 A; these are initialized operating-point tests.

The output mean is 32.00000 V with 0.263 V peak-to-peak ripple. Flying-terminal mean is 23.999 V with 2.6448 V ripple. Current mean is 15.62510 A with 20.4704 A ripple. The ideal conversion ratio is 0.6667. This case is above the one-half conversion boundary and has the largest current ripple of the three input cases.

Compare these three files to study input dependence of steady initialized operation. They are separate constant-input runs, so their results do not measure the transient response to a changing input supply.

### Initialized regulation at 66 V input

`Voltage_Regulation_66V_Input.asc`

This separate constant-input test runs for 3 ms with `uic` and a 30 ns maximum step. Inspect the final 2.95–3.00 ms window. The output starts at 32 V, the flying capacitor at 33 V and inductor current at 15.625 A; these are initialized operating-point tests.

The output mean is 32.00007 V with 0.052 V peak-to-peak ripple. Flying-terminal mean is 32.979 V with 3.9042 V ripple. Current mean is 15.62505 A with 4.1245 A ripple. The ideal conversion ratio is 0.4848. This nominal case has the smallest current ripple of the three; its balanced intermediate level is close to the 32 V output.

Compare these three files to study input dependence of steady initialized operation. They are separate constant-input runs, so their results do not measure the transient response to a changing input supply.

### Initialized regulation at 70 V input

`Voltage_Regulation_70V_Input.asc`

This separate constant-input test runs for 3 ms with `uic` and a 30 ns maximum step. Inspect the final 2.95–3.00 ms window. The output starts at 32 V, the flying capacitor at 35 V and inductor current at 15.625 A; these are initialized operating-point tests.

The output mean is 32.00005 V with 0.139 V peak-to-peak ripple. Flying-terminal mean is 34.985 V with 3.7040 V ripple. Current mean is 15.62504 A with 10.6639 A ripple. The ideal conversion ratio is 0.4571. This case is below the one-half conversion boundary; its current ripple exceeds the nominal 66 V result.

Compare these three files to study input dependence of steady initialized operation. They are separate constant-input runs, so their results do not measure the transient response to a changing input supply.

### Load increase and release at 48 V input

`Load_Change_Response_48V_Input.asc`

This 5 ms initialized feedback run uses `uic` and a 30 ns maximum step. The load changes from 8.192 Ω to 2.7306667 Ω at 3 ms and returns at 4 ms. At 32 V, these are 3.90625 A and 11.71875 A, or 25% and 75% of the nominal 500 W load. Review 2.8–4.8 ms to see the operating point before and after both edges.

The minimum output during 3–4 ms is 31.67668 V. The maximum after release during 4–4.8 ms is 32.31843 V. The largest deviation in those windows is approximately 1.01% of 32 V, including switching ripple. Mean inductor current is 11.71444 A over 3–4 ms and 3.90867 A over 4–4.8 ms.

At load increase, the output capacitor initially supplies the roughly 7.8125 A deficit. Its ideal initial voltage slope is −0.015625 V/µs, and 13 mΩ ESR contributes a voltage step of order 0.102 V. On release, excess inductor current charges the output capacitor and the voltage excursion reverses. Compare the three input cases to see how the same load schedule produces different voltage extrema.

### Load increase and release at 66 V input

`Load_Change_Response_66V_Input.asc`

This 5 ms initialized feedback run uses `uic` and a 30 ns maximum step. The load changes from 8.192 Ω to 2.7306667 Ω at 3 ms and returns at 4 ms. At 32 V, these are 3.90625 A and 11.71875 A, or 25% and 75% of the nominal 500 W load. Review 2.8–4.8 ms to see the operating point before and after both edges.

The minimum output during 3–4 ms is 31.82610 V. The maximum after release during 4–4.8 ms is 32.18495 V. The largest deviation in those windows is approximately 0.58% of 32 V, including switching ripple. Mean inductor current is 11.71615 A over 3–4 ms and 3.90840 A over 4–4.8 ms.

At load increase, the output capacitor initially supplies the roughly 7.8125 A deficit. Its ideal initial voltage slope is −0.015625 V/µs, and 13 mΩ ESR contributes a voltage step of order 0.102 V. On release, excess inductor current charges the output capacitor and the voltage excursion reverses. Compare the three input cases to see how the same load schedule produces different voltage extrema.

### Load increase and release at 70 V input

`Load_Change_Response_70V_Input.asc`

This 5 ms initialized feedback run uses `uic` and a 30 ns maximum step. The load changes from 8.192 Ω to 2.7306667 Ω at 3 ms and returns at 4 ms. At 32 V, these are 3.90625 A and 11.71875 A, or 25% and 75% of the nominal 500 W load. Review 2.8–4.8 ms to see the operating point before and after both edges.

The minimum output during 3–4 ms is 31.78458 V. The maximum after release during 4–4.8 ms is 32.23187 V. The largest deviation in those windows is approximately 0.72% of 32 V, including switching ripple. Mean inductor current is 11.71773 A over 3–4 ms and 3.90713 A over 4–4.8 ms.

At load increase, the output capacitor initially supplies the roughly 7.8125 A deficit. Its ideal initial voltage slope is −0.015625 V/µs, and 13 mΩ ESR contributes a voltage step of order 0.102 V. On release, excess inductor current charges the output capacitor and the voltage excursion reverses. Compare the three input cases to see how the same load schedule produces different voltage extrema.

### Cold start with zero specified storage energy

`Zero_Energy_Cold_Start.asc`

This case sets both capacitor initial voltages and inductor initial current to zero. It retains the two feedback loops, uses `startup` input application, and requests 400 µs with a 20 ns maximum step.

The saved run ends at 58.273 µs and reaches 580.63 A at about 47.729 µs. During the current rise, the output-loop duty is near its 0.98 upper clamp. The available record demonstrates excessive current and an incomplete simulation. It cannot establish settling, a completed startup trajectory, or a validated maximum over the requested 400 µs. The plot setting may extend beyond the actual data endpoint.

### Output-reference ramp and current-rise diagnosis

`Output_Reference_Ramp_Startup.asc`

This case begins with zero specified storage energy and replaces the output reference with `32 × min(t/1 ms, 1)`. It retains the compensator coefficients, fixed input-parameter duty bias and `startup` supply application. The simulation records 100 µs with a 20 ns maximum step; at that endpoint the reference is only 3.2 V.

The record reaches 198.21 A at about 25.91 µs and −76.43 A at about 72.90 µs. Output peak is 10.69 V and final output is 7.71 V. Flying-terminal voltage peaks at 67.23 V and ends near 40.03 V. The log records Gmin stepping after direct Newton iteration failed. These observations show excessive energy transfer and poor reference tracking during the observed portion of the ramp.

At 18.1013 µs, DB reaches its 0.02 floor while current is 172.674 A. Current keeps rising to 198.212 A at 25.9107 µs. Integrating `V(SW,VOUT)/1.36 µH` over that interval predicts +25.5383 A, agreeing with +25.5384 A from the current samples. Mean inductor voltage is +4.4475 V. DA stays near 0.495, leaving energy-transfer states active despite the DB floor.

Over the full saved 100 µs, QHSB reaches 67.704 V and QLSB 67.109 V, exceeding their 60 V ratings. QHSA and QLSA reach 43.064 V and 43.582 V against 100 V ratings. Reference ramping alone is insufficient in this configuration. The shorter run and different startup conditions prevent treating its lower peak than the cold-start trial as a controlled demonstration of successful startup improvement.

## Measurement and interpretation

Means are time-weighted integrals divided by window duration, using trapezoidal integration and interpolated window endpoints. An arithmetic mean of variable-timestep samples would weight densely stored intervals too heavily. Ripple is maximum minus minimum within the stated window. Full-record startup peaks and late regulation metrics describe different parts of a run.

The nominal recovery and fixed-PWM comparison use 1.45–1.50 ms. Constant-input tests use 2.95–3.00 ms. Load minima use 3–4 ms and release maxima use 4–4.8 ms. Cold-start findings end at the actual 58.273 µs endpoint; reference-ramp findings cover 0–100 µs, with the detailed inductor calculation over 18.1013–25.9107 µs.

The startup gate-state classification treats source-referenced gate voltage above 5 V as ON and assigns each stored interval using its left sample. Between the DB floor and current peak, Mode 1 occupies about 42.9%, Mode 2 52.0%, Mode 3 1.3%, and unclassified/dead-time commands 3.8%; Mode 4 is absent. QLSB is commanded ON for about 96.2% of that interval. This describes commands, not every channel/diode conduction path.

Direct flying-capacitor branch current was not saved. The charge estimate uses `Vterminal = uC + RESR × iC` and `iC = C × duC/dt`, with 20 µF, 3 mΩ, zero initial ideal-capacitor voltage and piecewise-linear terminal voltage. It gives about 1.151 mC between 15 µs and the terminal-voltage peak at 26.9002 µs. Positive current is from FLYP to FLYN. This estimate is model-derived.

The results establish prebiased regulation and diagnose unsuccessful startup. Hardware operation, measured efficiency, cooling performance and a successful coordinated startup controller remain unverified. Manufacturer notices and source attribution are retained.
