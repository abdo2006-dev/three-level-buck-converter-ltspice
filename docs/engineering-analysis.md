# Engineering explanation

## Flying-capacitor balance

The capacitor creates two intermediate switching levels: `VIN − VFly` and `VFly`. At `VFly = VIN/2`, the levels coincide. This reduces the inductor voltage excursion in balanced operation. Startup imbalance changes both inductor voltage and device stress, so the ideal half-input stress assumption does not apply to the observed startup.

## Two controllers, one power stage

Loop A senses `VIN/2 − VFly`; loop B senses `Vref − VOUT`. Their compensators generate bounded duty demands. Separate errors do not separate the energy paths: both duty commands still select states of the same switching network. There is no explicit clamp-dependent integrator reset or back-calculation in the implemented controller library.

## Prebiased recovery

The nominal recovery starts with 30 V on both capacitors and 14.6484 A in the inductor, using `uic`. Feedback gives close late output regulation, while the initial current reaches 75.876 A. These results demonstrate recovery from stored energy, not successful zero-energy startup.

## Load response

At 32 V, the load change increases current demand by 7.8125 A. The initial deficit comes from the 500 µF output capacitor, giving an ideal slope of −0.015625 V/µs. The 13 mΩ ESR adds a voltage change of about 0.102 V. The signs reverse on load release. These estimates explain the response direction; the switching simulation determines the actual extrema.

## Startup mechanism

Between the output duty floor and current peak, the observed current increases by 25.5384 A. The differential inductor-voltage integral predicts 25.5383 A. Loop A remains near 0.495 while loop B is clamped at 0.02, leaving Mode 1 active for about 42.9% of that interval. Positive net volt-seconds explain why reducing the output-loop command alone did not stop the current rise.

## Scope

The cold-start run is incomplete; the reference-ramp record covers only the first tenth of the ramp. Direct flying-capacitor current was not saved, so charge/current estimates use an ESR-aware capacitor model. Behavioral gate commands and fixed case temperature do not qualify practical gate-drive timing, cooling or hardware safe operation. The report retains solver warnings and both 60 V bank rating exceedances.
