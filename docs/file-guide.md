# File and plot reference

Abdulrahman Ahmed Mohammed

## Symbols and model libraries

| File | Role and interpretation |
|---|---|
| `BSC034N10LS5.asy` | Symbol for the 100 V MOSFET used in the two A banks. Pin order is drain, gate, source, junction temperature, case temperature. It refers to the 100 V manufacturer library. |
| `BSC027N06LS5.asy` | Corresponding five-terminal symbol for the 60 V MOSFET used in the B banks; points to the 60 V manufacturer library. |
| `Compensator.asy` | Reusable three-terminal symbol connecting error input, compensator output and ground to CTRL_A or CTRL_B. The actual equations are in the controller library. |
| `Control_Compensators.lib` | RC-state realizations of CTRL_A and CTRL_B. Includes zero initial controller-state capacitors and a 0.01 rad/s leakage pole. There is no explicit duty-clamp integrator reset or back-calculation anti-windup. |
| `OptiMOS5_100V_LTSpice.lib` | Infineon BSC034N10LS5 model dependency, version 041121. Retains its model notices. |
| `OptiMOS_OptiMOS5_60V_LTSpice.lib` | Infineon BSC027N06LS5 model dependency, version 100122. Retains its model notices. |

These files define simulation behavior and connectivity; they contain no independent experimental result. A symbol alone is insufficient without its referenced subcircuit library. The case-temperature boundary is fixed at 25 °C; this does not establish a practical thermal design.

## Every plot configuration

Each of the 13 schematics has a same-topic default `.plt`. Its purpose follows the experiment guide above. The eight additional settings are alternate views of existing runs. All 21 settings are listed below; trace expressions are taken directly from the files.

| Setting | Matching waveform topic | Traces | What to inspect |
|---|---|---|
| `Closed_Loop_Prebiased_Recovery.plt` | `Closed_Loop_Prebiased_Recovery` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Fixed_PWM_Prebiased_Recovery.plt` | `Fixed_PWM_Prebiased_Recovery` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Load_Change_Response_48V_Input.plt` | `Load_Change_Response_48V_Input` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Load_Change_Response_66V_Input.plt` | `Load_Change_Response_66V_Input` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Load_Change_Response_70V_Input.plt` | `Load_Change_Response_70V_Input` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Open_Loop_Startup_and_Switch_Stress.plt` | `Open_Loop_Startup_and_Switch_Stress` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Output_Reference_Ramp_Startup.plt` | `Output_Reference_Ramp_Startup` | `V(FLYP,FLYN)`, `V(vout)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Closed_Loop_Comparison.plt` | `Closed_Loop_Prebiased_Recovery` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Controlled recovery trace only. Open the separate fixed-PWM RAW and its own setting to compare implementations; this file does not contain both datasets. |
| `Open_Loop_MOSFET_Voltage_Stress.plt` | `Open_Loop_Startup_and_Switch_Stress` | `V(vin,flyp)`, `V(flyp,sw)`, `V(sw,flyn)`, `V(flyn)` | Four bank VDS traces across startup; compare actual extrema with 100 V / 60 V ratings. |
| `Open_Loop_Switching_States.plt` | `Open_Loop_Startup_and_Switch_Stress` | `V(sw)`, `V(flyp,flyn)`, `V(ghsa,flyp)`, `V(ghsb,sw)` | Switching-node levels and upper source-referenced gate timing; saved close-up is 400–420 µs. |
| `Reference_Ramp_Duty_and_Current.plt` | `Output_Reference_Ramp_Startup` | `I(L_o)`, `V(db)` | Output-duty floor alongside the continuing inductor-current excursion over 0–100 µs. |
| `Startup_Controller_Duty_Commands.plt` | `Output_Reference_Ramp_Startup` | `V(DA)`, `V(DB)` | 0–40 µs comparison of DA and DB; shows that DA remains active after DB reaches its floor. |
| `Startup_Inductor_Voltage_and_Current.plt` | `Output_Reference_Ramp_Startup` | `I(L_o)`, `V(SW,VOUT)`, `V(FLYP,FLYN)`, `V(vout)` | 15–35 µs close-up linking positive net inductor voltage, current, capacitor voltage and output. |
| `Startup_MOSFET_Voltage_Stress.plt` | `Output_Reference_Ramp_Startup` | `V(VIN,FLYP)`, `V(FLYP,SW)`, `V(SW,FLYN)`, `V(FLYN)` | Full 0–100 µs bank VDS view showing 60 V rating exceedances. |
| `Startup_Source_Referenced_Gates.plt` | `Output_Reference_Ramp_Startup` | `V(GHSA,FLYP)`, `V(GLSA)`, `V(GHSB,SW)`, `V(GLSB,FLYN)` | 15–35 µs gate-command combinations for state classification, measured relative to each source. |
| `Startup_with_Output_ESR_13_mOhm.plt` | `Startup_with_Output_ESR_13_mOhm` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Startup_with_Zero_Output_ESR.plt` | `Startup_with_Zero_Output_ESR` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Voltage_Regulation_48V_Input.plt` | `Voltage_Regulation_48V_Input` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Voltage_Regulation_66V_Input.plt` | `Voltage_Regulation_66V_Input` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Voltage_Regulation_70V_Input.plt` | `Voltage_Regulation_70V_Input` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |
| `Zero_Energy_Cold_Start.plt` | `Zero_Energy_Cold_Start` | `V(vout)`, `V(flyp,flyn)`, `I(L_o)` | Default view for the same-topic experiment. Use the measurement windows in its guide; autoscale for full-record peaks. |

## Reports, figures and documentation

| File | Contents and use |
|---|---|
| `README.md` | Project overview, architecture, detailed result tables, design decisions, navigation and reproduction steps. |
| `report/P54_Research_Report.pdf` | Complete academic report with RAW-based figures, equations, measurements, qualifications and references. |
| `docs/circuit-guide.md` | Purpose, setup, findings and limits for every schematic. |
| `docs/engineering-analysis.md` | Explanation of energy paths, feedback interaction and unresolved startup behavior. |
| `analysis/measurement-method.md` | Measurement windows, integration definitions and worked startup calculation. |
| `THIRD_PARTY_NOTICES.md` | Source-paper artwork and manufacturer-model attribution and reuse scope. |
| `.gitignore` | Keeps large generated RAW, log and netlist files and local metadata out of Git history. |
| `figures/power-stage.png` | Actual LTspice power stage, showing eight-device bank implementation. |
| `figures/gate-drive.png` | Actual behavioral gate-drive implementation. |
| `figures/closed-loop-recovery.png` | Plot made from saved feedback-recovery RAW samples. |
| `figures/fixed-pwm-recovery.png` | Corresponding fixed-PWM recovery data plot. |
| `figures/load-response-66v.png` | Nominal-input load increase and release. |
| `figures/startup-duty-commands.png` | DA/DB behavior during reference-ramp startup. |
| `figures/startup-inductor-voltage-current.png` | Startup inductor voltage/current evidence for the volt-second explanation. |

The GitHub figures and report provide recorded numerical evidence without requiring a new simulation. The large binary RAW datasets and original logs are in the separate professor submission package. Plot settings need those datasets or a newly generated compatible record; PNG figures cannot substitute for RAW data in measurement work. No smoothing, filtering, waveform-value modification or spike removal was applied to the RAW-based figures.
