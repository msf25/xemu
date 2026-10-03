# xemu (transfer-fix)

> **Experimental fork.** Speeds up file transfers to and from the emulated Xbox (XBDM / Xbox Neighborhood / `xbcp`).
> Only tested for file transfers, not for games. It may break things.

Two changes on top of upstream `master`:

- **nvnet RX fix** (`hw/xbox/mcpx/nvnet/nvnet.c`): when the guest's RX ring was full, packets were dropped and never
  retried, so host → Xbox TCP stalled on retransmit timeouts. They now stay queued until the guest frees a buffer.
- **Busy-wait poll fix** (`util/qemu-timer.c`): xemu's short-deadline busy-wait ignored I/O events, so finished disk
  reads and network traffic waited up to 1 ms. It now keeps polling while it spins.

Measured with XBDM 5849 over NAT, Windows host: upload ~0.13 MB/s → 25–40 MB/s, download ~3 MB/s → 5–8 MB/s.
Both seem to depend on the host CPU clock.


---

Please visit [https://xemu.app](https://xemu.app) for more information.
