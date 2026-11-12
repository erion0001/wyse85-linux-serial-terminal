# LINE, ECHO and half-duplex

This confused me at first because the WY-85 can show typed characters by itself **or** rely on Linux to echo them back. If both ends echo, everything looks doubled.

## LINE — full-duplex

Set **Echo = OFF** in the first Communications setup level.

The WY-85 sends keystrokes to the computer. The host sends back whatever should appear on the display. The terminal's status line shows **LINE**.

This is normally what you want with `agetty` and a Linux shell.

## ECHO / HDX — half-duplex

Set **Echo = ON**.

The terminal displays printable keystrokes locally while transmitting them to the computer. The status line shows **ECHO**. If the Linux host also echoes input, each character can appear twice.

Half-duplex isn't automatically a fault: it's simply a different way of handling keyboard input. It's usually not the right setting for an interactive Linux login, though.

## LOCL is different

`LOCL` isn't another kind of online echo. It means the terminal is **offline/local**: the keyboard is handled locally and host input is ignored. Go into F3 setup and set **Local = OFF**.

## Quick check

| Status | What it suggests | First thing to check |
| --- | --- | --- |
| `LINE` | Online, full-duplex | Correct for a typical Linux login |
| `ECHO` | Online, half-duplex | Set Echo OFF if typing appears twice |
| `LOCL` | Local mode | Set Local OFF in General setup |
| `BLCK` | Block transmission | Switch back to conversational mode |

The WY-85 manual describes these modes in Chapter 3. My hardware testing is still in progress.
