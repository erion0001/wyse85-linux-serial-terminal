# Linux login with runit (Void Linux)

Void uses runit rather than systemd. For a serial login, you can make a small service that runs `agetty` in the foreground.

I'll use `/dev/ttyUSB0` and **9600 8N1 / VT100** here. Check your own device name before using these commands.

## 1. Create a service

```sh
sudo mkdir -p /etc/sv/wyse85
sudo nano /etc/sv/wyse85/run
```

Contents of `run`:

```sh
#!/bin/sh
exec agetty --local-line --8bits ttyUSB0 9600 vt100
```

Note the argument order: `agetty [options] port baud term`.

Make the script executable:

```sh
sudo chmod +x /etc/sv/wyse85/run
```

## 2. Test it once

Void's handbook suggests a test run before enabling a new service permanently:

```sh
sudo touch /etc/sv/wyse85/down
sudo ln -s /etc/sv/wyse85 /var/service/wyse85
sudo sv once wyse85
sudo sv status wyse85
```

If it works, remove `down` and start it normally:

```sh
sudo rm /etc/sv/wyse85/down
sudo sv up wyse85
```

The `/var/service/` link is how Void enables the service for the current runsvdir and future boots.

## 3. Manage it

```sh
sudo sv status wyse85
sudo sv restart wyse85
sudo sv down wyse85
```

To disable the service altogether:

```sh
sudo sv down wyse85
sudo rm /var/service/wyse85
```

## A few things to watch for

- If the USB adapter isn't plugged in when runit starts the service, `agetty` may fail and be retried. Check the device first.
- Only one getty or serial testing program should access the port at once.
- If the WY-85 says `LOCL`, fix that in its F3 setup; runit can't fix the terminal's local mode.

References: [Void runit services](https://docs.voidlinux.org/config/services/index.html), [`agetty` manual](https://man7.org/linux/man-pages/man8/agetty.8.html).
