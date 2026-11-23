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
