# OpenLara CD32X REV - EDCD: started from the Mega EverDrive PRO

This is the OpenLara CD32X REV tree with the **cartridge boot**: the Mega EverDrive PRO stays the
cartridge (the console's "mode 1") and is the disc's 16 MB RAM cart.  The CD32X BootCamp's
`edboot/EDCD.32X` launcher starts the disc; the game is copied into the EverDrive and runs from it.

    CART=ED EDCD=1 bash OpenLaraCD32X/build.sh      # the disc (burn the _music.cue) + subcpu/EDCD.32X

Needs the CD32X BootCamp next to this folder (`BOOTCAMP=<folder>` otherwise).  Copy `EDCD.32X` to the
EverDrive's SD card and start it from the EverDrive menu with the disc in the Sega CD (not "Run Mega-CD").

The console finding behind it (25 Oct 2026, CD32X BootCamp `edw0`): with the EverDrive's write-enable bit
on, some of the 68000's writes to *other* addresses corrupt the cart, and the SH2's reads of the cart come
back as zeros meanwhile.  So the launcher keeps that bit off and the listener (`subcpu/hw_md.s`, `MODE1=1`)
turns it on only inside a copy into the cart.  The details are in the CD32X BootCamp's
`docs/12_CARTRIDGE_BOOT.md`.
