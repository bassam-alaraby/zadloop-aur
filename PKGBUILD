pkgname=zadloop
pkgver=0.5.0
pkgrel=1
pkgdesc='ZadLoop desktop coding agent'
arch=('x86_64')
url='https://zadloop.com/'
license=('MIT')
depends=(
  alsa-lib
  at-spi2-core
  bubblewrap
  dbus
  expat
  gcc-libs
  glib2
  'glibc>=2.39'
  gtk3
  libcups
  libnotify
  libsecret
  libxss
  libxtst
  mesa
  nss
  systemd-libs
  util-linux
  util-linux-libs
  xdg-utils
)
optdepends=('libappindicator: system tray integration')
makedepends=('binutils' 'libarchive')
options=('!strip')
install='zadloop.install'
source=("zadloop-${pkgver}-x64.deb::https://zadloop.com/download/linux/deb")
noextract=("zadloop-${pkgver}-x64.deb")
sha256sums=('b16b8fa217d568f28dffdc55a8c0be6577fc938e0b5cde88af1d9e870a00df53')

package() {
  local deb="$srcdir/zadloop-${pkgver}-x64.deb"
  local payload

  payload=$(ar t "$deb" | awk '/^data\.tar(\..*)?$/ { print; exit }')
  [[ -n "$payload" ]] || {
    error 'No Debian data.tar payload found'
    return 1
  }

  install -d "$pkgdir"
  ar p "$deb" "$payload" | bsdtar -xpf - --no-same-owner -C "$pkgdir"

  install -d "$pkgdir/usr/bin"
  ln -s /opt/ZadLoop/zadloop "$pkgdir/usr/bin/zadloop"
}
