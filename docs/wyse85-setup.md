# Setting up the WYSE WY-85

The WY-85 has its own setup system. You need to configure that *before* worrying too much about Linux, especially if the status line says `LOCL`.

## Enter setup

Press **F3 (SETUP)** on the WY-85 keyboard.

In the setup directory, move between the menu items with the arrow keys and use **Space** or **Enter** to select one. There are several levels inside each category. The original manual has the full list of parameters; the names below follow that manual.

## VT100 and serial settings

1. In the **General** setup levels, select **VT100** emulation (the relevant field is called `Mode` in the manual).
2. In the **first General** level, set **Local = OFF**. Otherwise the terminal ignores incoming host data.
3. In the **first Communications** level, set **Echo = OFF**. This selects full-duplex for a normal Linux login.
4. Set the COMM serial parameters to match the host: start with **9600 baud, 8 data bits, no parity, 1 stop bit**.
5. Check communication/flow-control options if characters don't arrive reliably. Your cable and both endpoints must agree.

The WY-85 also supports other emulation modes, but `vt100` is a reasonable starting point for Linux.

## Save the setup

Changing a setting doesn't automatically store it permanently.

1. Press **F3** to return to the setup directory.
2. Select **Save** and press Space or Enter.
3. The display briefly blanks while settings are saved.

You can also leave without saving, but those changes will be lost after power-off. The manual describes **Recall** (last saved settings) and **Default** (factory defaults) as separate actions. I'd avoid resetting everything until you've written down the existing configuration.

## If F3 doesn't work

If the keyboard has stopped responding, don't treat this as a Linux service issue just yet. You should normally be able to access setup without a working serial host connection.

Start with the keyboard plug and cable, then see [troubleshooting](troubleshooting.md). Don't open the CRT enclosure just to investigate a keyboard problem: CRT equipment can retain dangerous high voltage even when unplugged.

Reference: [WY-85 User's Guide](https://bitsavers.trailing-edge.com/pdf/wyse/WY-85/88-060-01B_WY-85_Users_Guide_May85.pdf), Chapter 2.
