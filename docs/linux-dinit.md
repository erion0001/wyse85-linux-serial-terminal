# Linux login with dinit

Dinit also runs ordinary foreground processes, so the idea is the same: start `agetty`, keep it supervised and restart it if needed.

Paths and available services vary a little between dinit distributions. The example below is for a system where service descriptions live in `/etc/dinit.d/`.

## 1. Create the service file

```sh
sudo nano /etc/dinit.d/wyse85
```

Add:

```ini
type = process
command = /sbin/agetty --local-line --8bits ttyUSB0 9600 vt100
restart = true
```

Use the path reported by `command -v agetty` if `/sbin/agetty` is wrong. The arguments mean: device `ttyUSB0`, 9600 baud and terminal type `vt100`.

## 2. Check and start

If `dinitcheck` is installed, validate the service description:

```sh
sudo dinitcheck wyse85
```

Then start and check it:

```sh
sudo dinitctl start wyse85
sudo dinitctl status wyse85
```

To have Dinit load it at boot, use:

```sh
sudo dinitctl enable wyse85
```

Whether `enable` works directly depends on the distribution's boot-service layout; some setups need an explicit `--from` target or dependency instead. Check the distribution's Dinit documentation rather than forcing a boot dependency blindly.

## 3. Stop or disable

```sh
sudo dinitctl stop wyse85
sudo dinitctl disable wyse85
```

## Notes

- The serial device must exist when `agetty` starts. USB hotplug behaviour can need extra service dependencies or device handling.
- The `restart` setting supervises agetty; it won't fix a missing USB device or crossed cable.
- This example has **not been tested** on my WY-85 yet.

References: [Dinit service file reference](https://davmac.org/projects/dinit/man-pages-html/dinit-service.5.html), [`dinitctl` reference](https://davmac.org/projects/dinit/man-pages-html/dinitctl.8.html), [`agetty` manual](https://man7.org/linux/man-pages/man8/agetty.8.html).
