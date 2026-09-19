# PHANTOM — Detection Circuit Simulations (LTspice)

LTspice `.asc` schematics validating the dual-channel passive peak-hold pulse stretcher with ratio-based ambient-referenced IR detection circuit described in the provisional patent filing.

- **ambient_referenced_detector_draft1.asc** — Base RC front-end: a pulsed current source (modeling the photodiode's IR-triggered response) split into a fast "hunter" channel and a slow-averaged "ambient" reference channel, isolating true IR events from ambient light drift.
- **peak_hold_pulse_stretcher_draft2.asc** — Full circuit: adds a comparator stage (`V=if(V(Vh) > 0.1, 5, 0)`) driving an NMOS peak-hold switch with diode clamping, stretching a microsecond-scale detection pulse into a sustained output suitable for MCU polling. Includes an OSRAM SFH4715AS diode SPICE model (`Is=1e-18 Rs=0.5 N=1.5 Cjo=50p`).

Open in [LTspice](https://www.analog.com/en/resources/design-tools-and-calculators/ltspice-simulator.html) and run `.tran 0 20ms 0 1us` to reproduce the transient response.
