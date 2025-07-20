# Maintainer: Ryan Hughes <1630358+ryanrhughes@users.noreply.github.com>
pkgname=omarchy-theme-example
pkgver=1.0.0
pkgrel=1
pkgdesc="Example theme package for Omarchy"
arch=('any')
url="https://github.com/ryanrhughes/omarchy-theme-example"
license=('MIT')
depends=()
makedepends=()
source=()
sha256sums=()
install=omarchy-theme-example.install

package() {
  # Install theme files to system directory
  install -dm755 "$pkgdir/usr/share/omarchy/themes/example"

  # Copy theme files from local directory
  cp -r "$startdir/theme_files"/* "$pkgdir/usr/share/omarchy/themes/example/"

  # Set proper permissions
  find "$pkgdir/usr/share/omarchy/themes/example" -type f -exec chmod 644 {} \;
  find "$pkgdir/usr/share/omarchy/themes/example" -type d -exec chmod 755 {} \;
}
