# Circuit guide

All schematics include the author name. Files are grouped by the engineering question, not an internal experiment sequence.

| Schematic | Purpose | Initial conditions / observation |
|---|---|---|
| `Open_Loop_Startup_and_Switch_Stress.asc` | Baseline fixed-PWM startup and switch stress | Original open-loop conditions; 0–450 µs |
| `Startup_with_Output_ESR_13_mOhm.asc` | Startup with output-capacitor ESR | 13 mΩ ESR; 0–450 µs |
| `Startup_with_Zero_Output_ESR.asc` | ESR comparison | Zero output ESR; 0–450 µs |
| `Closed_Loop_Prebiased_Recovery.asc` | Nominal feedback recovery; controlled side of fixed versus feedback comparison | Capacitors 30 V, inductor 14.6484 A; `uic`; 1.5 ms |
| `Fixed_PWM_Prebiased_Recovery.asc` | Fixed-PWM side of the comparison | Same storage initial conditions; PULSE drives; 1.5 ms |
| `Voltage_Regulation_48V_Input.asc` | Constant-input regulation | Separate initialized 3 ms test |
| `Voltage_Regulation_66V_Input.asc` | Constant-input regulation | Separate initialized 3 ms test |
| `Voltage_Regulation_70V_Input.asc` | Constant-input regulation | Separate initialized 3 ms test |
| `Load_Change_Response_48V_Input.asc` | Load response | 8.192 Ω to 2.73067 Ω at 3 ms, returning at 4 ms |
| `Load_Change_Response_66V_Input.asc` | Load response | Same load schedule |
| `Load_Change_Response_70V_Input.asc` | Load response | Same load schedule |
| `Zero_Energy_Cold_Start.asc` | Cold startup | Zero specified storage energy; `startup`; requests 400 µs, recorded run ends at 58.273 µs |
| `Output_Reference_Ramp_Startup.asc` | Reference-ramp startup and diagnostic analysis | Zero specified storage energy; 1 ms reference ramp; recorded through 100 µs |

The reference-ramp diagnostics are views of one simulation, not additional independent experiments. The nominal feedback waveform is reused for the controlled comparison. Manufacturer model files and symbols are included alongside the schematics.
