# Linux login with systemd

On a systemd system (for example Debian or Arch Linux), use the provided `serial-getty@.service` template. It will run `agetty` on a serial port and provide a regular login prompt.

These steps assume the adapter appears as `/dev/ttyUSB0`. **Check yours first.** Also, don't run a second getty or a serial terminal program on the same port at the same time.

## 1. Find the port

```sh
ls -l /dev/serial/by-id/
ls -l /dev/ttyUSB*
```

You can also inspect recent kernel messages with `sudo dmesg` or `journalctl -k`.

If you use a native PC serial port, the device might be `/dev/ttyS0` instead.

## 2. Override the service settings

```sh
sudo systemctl edit serial-getty@ttyUSB0.service
```

Add:

```ini
[Service]
ExecStart=
ExecStart=-/sbin/agetty --local-line --8bits 9600 %I vt100
```

The empty `ExecStart=` removes the template command. The next line sets a fixed **9600 baud**, 8-bit input and `TERM=vt100`. `--local-line` avoids requiring modem carrier detect for a direct cable connection.

Check `command -v agetty` and use that path if `/sbin/agetty` isn't available on your distro.

## 3. Start it

```sh
sudo systemctl daemon-reload
sudo systemctl enable --now serial-getty@ttyUSB0.service
systemctl status serial-getty@ttyUSB0.service
```

Logs:

```sh
journalctl -u serial-getty@ttyUSB0.service -b
```

If it's running but the screen stays blank, hit Return on the WY-85 and check the cable, COMM port, serial parameters and Local mode. A running service does **not** prove the serial connection is good.

## 4. Stop or remove it

```sh
sudo systemctl disable --now serial-getty@ttyUSB0.service
```

To remove the override later, use `sudo systemctl revert serial-getty@ttyUSB0.service` if you want to return to the distro defaults.

## Notes

- USB device names can change after reconnecting the adapter. Don't blindly reuse `/dev/ttyUSB0` if you have several USB serial devices.
- The WY-85 and Linux must agree on baud rate, parity, stop bits and flow control.
- Agetty creates a login prompt; it does not replace or fix faulty RS-232 wiring.

Sources: [systemd serial-getty template](https://github.com/systemd/systemd/blob/main/units/serial-getty%40.service.in), [`agetty` manual](https://man7.org/linux/man-pages/man8/agetty.8.html).
