# ZadLoop for Arch Linux (zadloop-bin)

Unofficial, experimental packaging of [ZadLoop](https://zadloop.com/) for Arch
Linux and Arch-based distributions such as Omarchy.

> **Experimental release — version 0.5.1**
>
> This community package is not affiliated with or supported by ZadLoop. The
> upstream Linux downloads currently target Debian/Ubuntu and Fedora; Arch
> Linux is not listed as an officially supported distribution. Use it for
> evaluation and report packaging issues through this repository.

## Package status

- Package name: `zadloop-bin` (provides and conflicts with `zadloop`)
- Architecture: `x86_64`
- Source: the official ZadLoop Debian package, downloaded during the build
  from a version-specific URL on `releases.zadloop.com` and verified against a
  pinned SHA-256 checksum.
- Application format: Electron desktop application (installed to `/opt/ZadLoop`).
- Installed size: approximately 1.1 GiB.
- AUR status: not published yet.

The Debian package is not included in this repository. The package provides a
`zadloop` command and a desktop entry.

## Build and install

```sh
sudo pacman -S --needed base-devel git
git clone https://github.com/bassam-alaraby/zadloop-bin.git
cd zadloop-bin
makepkg -si
```

Run `makepkg` as your regular user, without `sudo`. Make sure you have enough
free disk space for the downloaded archive, the build files and the installed
application.

Launch ZadLoop from the application menu or run `zadloop`.

For an initial evaluation, use a disposable project or folder and review the
selected workspace access mode before asking the application to make changes.

## Updates

The in-app updater is disabled in this package: the updater configuration
(`app-update.yml`) is removed during packaging, so updates are delivered
through the package manager instead. When a new ZadLoop version is released,
this package must be updated (`pkgver`, checksum, `.SRCINFO`) before you can
upgrade.

## Remove

```sh
sudo pacman -Rns zadloop-bin
```

This removes ZadLoop and dependencies that were installed automatically for it.
It does not normally remove personal application data in your home directory.

## Package maintenance

Update to a new upstream version:

```sh
# edit pkgver in PKGBUILD, then:
updpkgsums
makepkg --printsrcinfo > .SRCINFO
makepkg -sf
```

Check the recipe and the resulting package (`namcap` reports many warnings
about the bundled Python/Node runtime; these come from the upstream package):

```sh
namcap PKGBUILD zadloop-bin-*.pkg.tar.zst
```

Testing so far: the package builds and installs. Broader application testing
is still in progress.

## License

The package recipe (`PKGBUILD`, `zadloop.install`, this README) is licensed
under MIT. ZadLoop itself is proprietary software distributed under its own
terms; this repository does not redistribute any ZadLoop binaries.