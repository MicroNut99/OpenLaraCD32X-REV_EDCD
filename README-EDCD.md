# OpenLara CD32X REV - EDCD: started from the Mega EverDrive PRO

**Runs on the console** (Genesis + Sega CD + 32X, Mega EverDrive PRO, 25 Oct 2026).

This is the OpenLara CD32X REV tree with the **cartridge boot**: the Mega EverDrive PRO stays the
cartridge (the console's "mode 1") and is the disc's 16 MB RAM cart.  The CD32X BootCamp's
`edboot/EDCD.32X` launcher starts the disc; the game is copied into the EverDrive and runs from it.

## Build

One command, from the folder that holds `build.sh`, with the same toolchain settings as a normal build
(`ROOTDIR=` / `SEGA_TOOLCHAIN=`) and the CD32X BootCamp checked out next to it:

    CART=ED EDCD=1 bash build.sh

In this repository `build.sh` is in `OpenLaraCD32X/`, so from the repository's root:

    CART=ED EDCD=1 BOOTCAMP=../CD32X-BootCamp bash OpenLaraCD32X/build.sh

(`BOOTCAMP=<folder>` is where the CD32X BootCamp is; without it, `../CD32X-BootCamp` next to the folder
that holds `build.sh`.)

The full line as used here (WSL, the toolchain tree on `S:`, the log kept next to the build):

    cd /mnt/s/KoboPort/sega-toolchain-12.1/sega/OpenLaraCD32X-REV
    CART=ED EDCD=1 BOOTCAMP=/mnt/s/KoboPort/sega-toolchain-12.1/sega/CD32X-BootCamp bash /mnt/s/KoboPort/sega-toolchain-12.1/sega/OpenLaraCD32X-REV/build.sh 2>&1 | tee /mnt/s/KoboPort/sega-toolchain-12.1/sega/OpenLaraCD32X-REV/build_edcd_log.txt

It makes:

| File | What to do with it |
|---|---|
| `subcpu/CDROMPlayer_cartED_music.cue` + `.bin` | burn it (data + CD audio) |
| `subcpu/EDCD.32X` | copy it to the EverDrive's SD card |

Then: the disc in the Sega CD, the EverDrive in the 32X, start `EDCD.32X` from the EverDrive menu
(not "Run Mega-CD").  The disc also boots the normal way with a RAM cart.

Other switches: `EDCD_X=1` CTRL0 with the EverDrive's X bit; `EDCD_W0=<bank>` the game in that EverDrive
bank; `M1WORDS=1` word copies into the cart.  All off by default; none was needed on the console.

The console finding behind it (25 Oct 2026, CD32X BootCamp `edw0`): with the EverDrive's write-enable bit
on, some of the 68000's writes to *other* addresses corrupt the cart, and the SH2's reads of the cart come
back as zeros meanwhile.  So the launcher keeps that bit off and the listener (`subcpu/hw_md.s`, `MODE1=1`)
turns it on only inside a copy into the cart.  The details are in the CD32X BootCamp's
`docs/12_CARTRIDGE_BOOT.md`.

The longer story - what it is, what it isn't, how it works, what the console taught - is in
[`README-EVERDRIVE.md`](README-EVERDRIVE.md).
