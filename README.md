# P.H.A.N.T.O.M.
### Photonic Harassment and Neutralisation of Tactical Observation Mechanisms

A wearable, glasses-mounted system that autonomously detects unauthorised IR-based imaging devices and deploys active photonic countermeasures to degrade their imaging quality. Built on ATmega328P with no external connectivity required.

**Version:** 3.0 | **Submitted to:** Prof. Yogesh Babu S | **VIT Vellore — 23BCT0151**

---

## System Overview

Three tightly coupled subsystems:

| Layer | Role |
|---|---|
| Sensing Layer | Dual-channel passive photodiode detection with peak-hold pulse stretching |
| Attacking Layer | Pulsed 850nm IR + cool-white LEDs in three-phase macro-cycle with PRNG timing |
| Power Distribution Network | Split-rail architecture isolating logic from high-current muscle rail |

---

## Hardware

| Component | Spec |
|---|---|
| MCU | ATmega328P |
| IR Emitters | 3× OSRAM SFH4715AS (850nm, 1000mW/sr) |
| IR Sensors | 7× 940nm PIN Photodiodes (6 Hunter + 1 Ambient) |
| Op-Amp | LM358 (unity-gain ADC buffer — v3 new) |
| Darlington Driver | ULN2003A (probe LED driver — v3 new) |
| GPIO Expander | PCF8574 I2C (bank select — v3 new) |
| MOSFET | IRLZ44N (IR bank + visible array gate drive) |
| Battery | 2S 18650 Li-ion 4000mAh (~36hr battery life) |

---

## v3.0 New ICs

- **LM358** — Unity-gain voltage follower on all Hunter channels, eliminates ADC loading drift
- **ULN2003A** — High-current Darlington driver for IR probe LEDs, removes GPIO stress
- **PCF8574** — I2C GPIO expander for IR bank gate control, frees ATmega328P GPIO

---

## Detection Strategy

- Dual-channel ratio detection: Hunter > 1.5× Filtered Ambient
- Peak-hold pulse stretcher expands µs IR pulses to 1ms detection window
- Confidence counter (3 consecutive samples) rejects noise
- Spatial consistency check across 6 Hunter channels
- ADC prescaler 16 → ~76,800 sps at 8-bit resolution

---

## Emission Strategy

- Three-phase macro-cycle: Burst → Lull → Spike
- Skewed PRNG jitter (min of two uniform draws)
- Probabilistic spike injection (p=0.20, 5A, ≤150µs)
- Directional weighted bank selection from Hunter spatial data
- Dual-spectrum interleaving: 850nm IR + 5000K cool-white visible

---

## Validation Status

| Subsystem | Status |
|---|---|
| Hunter Sensing (HPF + Peak-Hold) | ✅ PASS |
| Ambient Channel (LPF + dual-path) | ✅ PASS |
| LM358 ADC Buffer | ✅ PASS — v3 new |
| ULN2003A Probe LED Driver | ✅ PASS — v3 new |
| PCF8574 I2C Bank Select | ✅ PASS — v3 new |
| MOSFET Gate Switching (3-Phase) | ✅ PASS |
| SFH4715AS IR Emission | ✅ PASS |
| Visible LED Array | ✅ PASS |
| PDN Star Ground + Decoupling | ✅ PASS |
| TIA Front-End | ⏳ PENDING — Faculty guidance required |
| Battery Life Endurance | ⏳ PENDING |
| Full Integrated Firmware | 🔄 IN PROGRESS |

---

*Devang Chaturvedi — 23BCT0151 — VIT Vellore*
