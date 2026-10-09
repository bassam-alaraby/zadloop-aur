# ZadLoop for Arch Linux

Unofficial experimental packaging of [ZadLoop](https://zadloop.com/) for Arch
Linux and Arch-based distributions such as Omarchy.

> **Experimental release — version 0.5.0**
>
> This community package is not affiliated with or supported by ZadLoop. The
> upstream Linux downloads currently target Debian/Ubuntu and Fedora; Arch
> Linux is not listed as an officially supported distribution. Use it for
> evaluation and report packaging issues through this repository.

## Package status

- Architecture: `x86_64`
- Packaging source: the official ZadLoop Debian package, downloaded during the
  build and verified against a pinned SHA-256 checksum.
- Application format: Electron desktop application.
- Installed size: approximately 1.14 GiB.
- AUR status: not published yet.

The Debian package itself is not included in this repository. The build recipe
retrieves it from ZadLoop's official download endpoint. The Arch package
declares the system libraries needed by the bundled application and provides a
`zadloop` command and desktop entry.

## Build and install

On an Arch Linux or Omarchy `x86_64` system, install the build tools and clone
this repository:

```sh
sudo pacman -S --needed base-devel git
git clone https://github.com/bassam-alaraby/zadloop-aur.git
cd zadloop-aur
makepkg -si --cleanbuild --clean
```

Run `makepkg` as your regular user, without `sudo`. It downloads the official
Debian package, verifies its checksum, builds the Arch package, and installs it
after a successful build. Allow additional free disk space for the downloaded
archive, build files, and installed application.

Launch ZadLoop from the application menu or run:

```sh
zadloop
```

For an initial evaluation, use a disposable project or folder and review the
selected workspace access mode before asking the application to make changes.
The in-app updater has not been verified on Arch; use package updates and do
not approve an in-app update until Arch behavior is confirmed.

## Remove

```sh
sudo pacman -Rns zadloop
```

This removes ZadLoop and dependencies that were installed automatically for it
and are no longer required by another package. It does not normally remove
personal application data in your home directory.

## Package maintenance

Verify that the source download matches the checksum in `PKGBUILD`:

```sh
makepkg --verifysource
```

Build the package without installing it:

```sh
makepkg --syncdeps --cleanbuild
```

If `namcap` is installed, check the recipe and the resulting package archive:

```sh
namcap PKGBUILD zadloop-*.pkg.tar.zst
```

After changing `PKGBUILD`, regenerate `.SRCINFO` before committing:

```sh
makepkg --printsrcinfo > .SRCINFO
```

The 0.5.0 package has been built and received a basic launch and file-creation
smoke test. Broader application testing and verification of the in-app update
flow remain outstanding.

## License

The package recipe is licensed under MIT. ZadLoop is a separate upstream
application and is distributed under its own license and terms.
