# Measurement definitions

Abdulrahman Ahmed Mohammed

For a window `[ta, tb]`, means use the time integral divided by window duration. Window endpoints are linearly interpolated; integration is trapezoidal and time weighted. Ripple is maximum minus minimum in that window. Startup extrema are taken from their complete available records.

- Nominal feedback and fixed-PWM comparison: late window 1.45–1.50 ms; full record 0–1.50 ms.
- Constant-input cases: 2.95–3.00 ms.
- Load response: analysis window 2.8–4.8 ms; increase at 3 ms and release at 4 ms.
- Cold-start record: 0–58.273 µs, incomplete.
- Reference-ramp record: 0–100 µs; detailed current-rise interval 18.1013–25.9107 µs.

## Worked inductor calculation

```text
L = 1.36 µH
Interval ≈ 7.8094 µs
Mean V_L = 4.4475 V
Δi ≈ 4.4475 × 7.8094 / 1.36 ≈ 25.538 A
```

The full-sample voltage integration gives 25.5383 A; measured current increase is 25.5384 A. The rounded worked calculation is illustrative, not a substitute for sample integration.

## Capacitor-current estimate

The terminal model is `Vterminal = uC + RESR × iC`, with `iC = C × duC/dt`. Using 20 µF, 3 mΩ, zero initial ideal-capacitor voltage and piecewise-linear terminal voltage gives approximately 1.151 mC from 15 µs to the terminal-voltage peak. This is a model-derived estimate, not directly recorded branch current.
