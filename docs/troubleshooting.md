# Troubleshooting

This is a running list of the things I've checked (or still need to check). The WY-85 on my desk powers on, but the keyboard/serial issue isn't fully resolved.

## The screen says `LOCL`

`LOCL` means the terminal is in Local mode. Incoming data from the Linux host is ignored.

If the keyboard works:

1. Press **F3** for setup.
2. Enter the first **General** setup level.
3. Set **Local = OFF**.
4. Save the change from the setup directory.

Power-cycling alone isn't a guaranteed fix because saved configuration can survive shutdown.

If F3 doesn't respond, investigate the keyboard/connection first. The setup key is handled by the terminal, not Linux.

## Characters are printed twice

If the status line says `ECHO`, the terminal is in half-duplex mode. With a normal Linux login, the host may echo the same character again.

Set **Echo = OFF** in the first Communications setup level. The status line should say `LINE` in the normal full-duplex mode.

## The screen is blank / no login prompt

Check in this order:

1. Is the WY-85 on, and is its brightness adjusted?
2. Does the status line show `LOCL`? Set Local OFF.
3. Is the cable plugged into **COMM**, not the printer port?
4. Is `/dev/ttyUSB0` actually the right Linux port?
5. Is the getty running? Check the relevant init-system guide.
6. Do both ends use the same baud rate, data bits, parity and stop bits?
7. Is the cable straight-through or null-modem as required? Verify TX, RX, ground and flow-control requirements, not just connector gender.
8. Try pressing Return on the terminal after the service starts.

On Linux, check whether some other process owns the serial device:

```sh
sudo fuser -v /dev/ttyUSB0
```

Don't run `screen`, `minicom` or another serial monitor alongside the getty on the same device.

## Keyboard input doesn't work

This is the part I'm still looking into with my WY-85.

- Inspect the keyboard connector and check for bent or dirty contacts.
- Check the cable for visible damage.
- With everything powered off and disconnected, check cable continuity using a multimeter if you know the connector pinout.
- Separate keyboard faults from serial faults: F3 should be usable independently of Linux.
- Don't assume there is a safe keyboard reset or firmware replacement without checking the hardware documentation.

I haven't confirmed whether my problem is the keyboard, its cable, a configuration state or the terminal's electronics. I won't label it fixed until I can reproduce the result.

## Safety

The WY-85 is CRT equipment. Don't open the monitor enclosure for these software/serial tests. High voltage can remain inside even after disconnection from power. Cable continuity measurements should only be done on disconnected, unpowered cables.

Reference: [WY-85 User's Guide](https://bitsavers.trailing-edge.com/pdf/wyse/WY-85/88-060-01B_WY-85_Users_Guide_May85.pdf), Chapters 2–4.
