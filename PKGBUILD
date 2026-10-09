# Draft for review only. Do not publish until dependency metadata and checksum
# have been verified against the official 0.5.0 Debian package.
pkgname=zadloop
pkgver=0.5.0
pkgrel=1
pkgdesc='Coding agent that starts in Arabic'
arch=('x86_64')
url='https://zadloop.com/'
license=('custom')
# TODO: Populate runtime dependencies from the Debian control metadata and
# inspect ELF/N-API dependencies. glibc is only the known baseline.
depends=('glibc')
makedepends=('binutils' 'libarchive')
options=('!strip')
source=("zadloop-${pkgver}-x64.deb::https://zadloop.com/download/linux/deb")
# TODO: Replace SKIP with the verified SHA-256 of the pinned upstream asset.
# The official page currently exposes a redirecting download endpoint, not a
# versioned asset URL or published checksum.
sha256sums=('SKIP')

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
}
