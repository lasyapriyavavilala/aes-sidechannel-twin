# AES Side-Channel Digital Twin

An interactive, in-browser visualization of a side-channel attack on AES-128.

A model built from the chip's datapath predicts the power it leaks, and an attacker
turns that leakage back into the secret key — shown live:

- **The chip** — the 128-bit AES state register stepping through its 11 clock cycles,
  with the bits that flip each cycle (the power the chip leaks) lit up.
- **Oscilloscope + leak detector** — the power trace and the signal-to-noise ratio,
  with the leak appearing in the final round.
- **The attacker** — 256 key-byte guesses; the correct one rises out of the noise and
  the key is recovered.

Everything runs in the browser: it encrypts with AES-128, simulates the power traces,
measures the leak, and runs the CPA attack on the fly — no pre-baked data.

**Live page:** https://lasyapriyavavilala.github.io/aes-sidechannel-twin/

Part of the `sca-twin` project: an information-theoretic leakage twin validated against
Verilog RTL and against the real ASCAD and AES_HD trace datasets.

## Is this really a digital twin?

Fair question — "digital twin" gets stretched a lot, so here's the honest version.

A digital twin isn't just a simulation; it's a model of a **specific real system**, kept in
correspondence with the real thing through real measurements, and used to **predict or test**
something about it. This project earns the name through that correspondence loop:

1. **Built from the real structure, not a guess.** The power-leakage model is derived from the
   actual AES datapath (power ∝ the register bits that flip each clock), not an assumed curve.
2. **Validated against real measurements.** Given only the datapath and a noise level, the model
   reproduces the *measured* leakage of real chips — the signal-to-noise ratio it predicts
   (~0.015) matches the real ASCAD / AES_HD trace datasets.
3. **Validated against the real blueprint.** The model matches a Verilog RTL implementation of the
   same datapath, byte-for-byte, per clock cycle.
4. **Used to decide something real:** how leaky a design is and whether a countermeasure (masking)
   works — *before* committing to silicon.

So the chain is: real datapath → model → checked against real traces → checked against RTL → used
for a pre-silicon decision.

**Scope, stated plainly:** this is a **pre-silicon / design-time** twin (standard in chip design),
not one wired to a live physical chip streaming telemetry in real time. And *this web page* is the
live simulation and storytelling front-end — its "twin" credibility comes from the validation work
in the full [sca-twin](https://github.com/lasyapriyavavilala) project, not from the animation alone.
