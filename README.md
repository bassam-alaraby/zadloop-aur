# ZadLoop Arch / AUR draft

This is a staging draft, not ready to publish. It repackages the official
Debian package because the vendor currently offers `.deb` and `.rpm` Linux
downloads but no source archive or Arch package.

## What is confirmed

- Official download page lists ZadLoop 0.5.0 for x64.
- Linux targets listed by the vendor: Ubuntu 24.04, Debian 13, Fedora 40+.
- The Debian link resolves through the official page to
  `https://zadloop.com/download/linux/deb`; the RPM link is
  `https://zadloop.com/download/linux/rpm`.
- Vendor says ZadLoop updates itself and requests the package manager to
  install updates. This behavior must be tested on Arch; it may conflict with
  pacman ownership and should be disabled or addressed before publishing.

## Not confirmed; required before AUR

The official package download endpoint could not be fetched in this run: the
shell request returned HTTP 403 and the in-app browser blocked direct download.
Therefore the package contents, exact package format internals, implementation
technology (Electron/Tauri/native), runtime dependencies, desktop/icon paths,
update mechanism, package licensing details, final asset URL, and SHA-256 are
not verified. `depends=('glibc')` and `sha256sums=('SKIP')` in the PKGBUILD are
explicit placeholders. Do not submit this draft to the AUR in this state.

Once the `.deb` is available locally, inspect it without installing:

```sh
ar t ZadLoop-0.5.0-x64.deb
mkdir -p deb-inspect
bsdtar -xf ZadLoop-0.5.0-x64.deb -C deb-inspect
bsdtar -xOf deb-inspect/control.tar.* ./control
find deb-inspect -type f -print
find deb-inspect -type f -exec file {} +
```

Use `readelf -d` on ELF executables and shared libraries to identify shared
library needs. Check Debian control `Depends`, maintainer scripts, desktop
entry, icons, bundled runtime, and updater behavior. Map only actual runtime
requirements to Arch packages, pin a version-specific upstream asset URL if
available, calculate its SHA-256, and replace both placeholders in `PKGBUILD`.
Do not execute binaries or maintainer scripts just to inspect the package.

## Local build and test steps

After completing the metadata and checksum, from this directory:

1. On Arch or Omarchy, install the required tools:

   ```sh
   sudo pacman -S --needed base-devel namcap
   ```

2. Run `makepkg --verifysource` to check the source URL and checksum.
3. Run `makepkg --syncdeps --cleanbuild` to build the package.
4. Run `namcap PKGBUILD zadloop-*.pkg.tar.zst` and resolve findings.
5. In a disposable Arch VM, install with
   `sudo pacman -U ./zadloop-*.pkg.tar.zst`; launch ZadLoop, check its desktop
   entry and icons, test core flows and restart behavior, and inspect whether
   its updater invokes `apt`, `dnf`, `dpkg`, or `rpm`.
6. Remove it with `sudo pacman -Rns zadloop` and confirm package-owned files
   were removed cleanly.
7. Regenerate `.SRCINFO` using `makepkg --printsrcinfo > .SRCINFO` after all
   metadata is final.

## Publish after local verification

After the maintainer decides to publish, configure an AUR account and SSH key.
Then, from a separate clean directory (these commands publish externally):

```sh
mkdir zadloop-aur && cd zadloop-aur
git init -b master
git remote add origin ssh://aur@aur.archlinux.org/zadloop.git
cp /absolute/path/to/final/PKGBUILD .
makepkg --printsrcinfo > .SRCINFO
git diff --check
git diff -- PKGBUILD .SRCINFO
git add PKGBUILD .SRCINFO
git commit -m 'Initial import'
git push -u origin master
```

Replace the example path with this directory's path. The final `git push`
publishes the package; it has not been run.
