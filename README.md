# ZadLoop for Arch Linux / Omarchy

This AUR package repackages the official x86_64 Debian payload. The package
contents identify the application as Electron, with the app installed under
`/opt/ZadLoop`, a desktop entry and icon, and a bundled Chromium/Electron
runtime.

## Package facts verified from ZadLoop 0.5.0

- Debian control metadata: package `zadloop`, version `0.5.0`, architecture
  `amd64`, MIT license.
- Declared Debian dependencies: GTK 3, libnotify, NSS, XScreenSaver, XTest,
  xdg-utils, AT-SPI, UUID, libsecret, bubblewrap, and glibc 2.39 or newer.
- The main ELF also needs ALSA, CUPS, GBM/Mesa, D-Bus, and systemd's udev
  library. `PKGBUILD` maps these to Arch package names.
- The Debian post-install script creates `/usr/bin/zadloop`, adjusts the
  Electron `chrome-sandbox` mode according to user-namespace availability,
  refreshes desktop/MIME databases, and conditionally installs an AppArmor
  profile. The Arch package ships the executable symlink and handles the
  sandbox mode through `zadloop.install`; it does not run the Debian scripts.
- SHA-256 for the provided official `ZadLoop-0.5.0-x64.deb`:
  `b16b8fa217d568f28dffdc55a8c0be6577fc938e0b5cde88af1d9e870a00df53`.
- `.gitignore` excludes `.deb`, `.rpm`, `.AppImage`, and makepkg build output.
  The downloaded `.deb` is present locally but is not tracked by Git.

The vendor's download page lists Linux 0.5.0 for x64, supports Ubuntu 24.04,
Debian 13, and Fedora 40+, and says the app updates itself through the system
package manager. The embedded updater points at
`https://releases.zadloop.com/desktop/v1/stable/linux/x64/`. Its behavior on
Arch has not been run or verified; test it in a disposable Arch/Omarchy VM
before publishing, especially to confirm it does not try to install a DEB or
RPM outside pacman.

## Build and verify locally

On an Arch Linux or Omarchy system with `base-devel` installed:

```sh
makepkg --verifysource
makepkg --syncdeps --cleanbuild
```

The SHA-256 is pinned in `PKGBUILD`. The source URL is ZadLoop's official
download endpoint; if it starts serving a different release, the checksum
will fail until the package version and hash are updated.

If `namcap` is installed, review the recipe and built archive:

```sh
namcap PKGBUILD zadloop-*.pkg.tar.zst
```

Then install and test the resulting package in a disposable Arch/Omarchy VM,
including the app launcher, core UI, sandbox behavior, and updater. Do not
install it on a daily-use system until that check is complete. Remove the test
package with `sudo pacman -Rns zadloop`.

Regenerate package metadata after changing `PKGBUILD`:

```sh
makepkg --printsrcinfo > .SRCINFO
```

## AUR publishing

The project working tree is connected to its GitHub remote. AUR is a separate
Git repository. To publish, first confirm the Arch VM checks, then follow the
AUR submission process for the package name `zadloop`. Keep the `.deb` out of
the repository; AUR users download it from ZadLoop's official endpoint via
`PKGBUILD`. No AUR push has been made.
