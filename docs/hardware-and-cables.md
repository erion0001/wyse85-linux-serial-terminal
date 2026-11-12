# Hardware and cables

The most annoying part of this setup might be finding the correct cable. You can have a working terminal, a working serial adapter and still get absolutely nothing on the screen because the plugs fit but the signals don't.

## Shopping list

- WYSE WY-85 and its keyboard
- Linux machine
- USB-to-**RS-232** adapter if the Linux machine doesn't have a native serial port
- Cable or adapter suitable for the WY-85's **COMM** port (DB25)
- Null-modem wiring / adapter **if required by the two endpoints**
- DB9-to-DB25 adapter, if needed
- DB9 female-to-female gender changer, if needed for the physical connectors

### The connector workaround

A lot of the null-modem cables you find online use DB9 connectors and might have the wrong gender for your adapter. Using a female-to-female adapter can make the search easier: you can buy a more common cable and adapt its physical connector to your setup.

**But:** a gender changer only changes which plugs fit. It does *not* swap TX/RX and does *not* make a straight-through cable into a null-modem cable.

Likewise, a USB-to-RS-232 adapter handles the electrical conversion. A null-modem cable doesn't do that conversion; it just changes the RS-232 signal routing.

## Before buying anything

1. Check whether your computer side is DB9 male or female.
2. Check the WY-85 COMM connector and the corresponding pin assignments in **Appendix B** of the user guide.
3. Work out whether your combination needs straight-through wiring or a null-modem crossover. Don't assume solely from connector gender.
4. Check signal ground and whether hardware handshaking lines are required. A three-wire connection won't work with every flow-control setup.
5. If you have a multimeter, check **unpowered cable continuity** against the expected pinout.

Use the terminal's **COMM** port, not its printer or TEST port. Also, avoid confusing real RS-232 with TTL-level serial interfaces used by microcontrollers — those aren't electrically interchangeable.

## A simple connection sketch

```text
Linux PC -> USB-to-RS-232 adapter -> cable/adapters -> WY-85 COMM port
               (if needed)          (verified pinout)
```

This is intentionally not a universal wiring diagram. Different adapters and cables are wired differently, and I'll add a tested pinout once I've verified my own cable.

See the [troubleshooting guide](troubleshooting.md) if nothing is received.
