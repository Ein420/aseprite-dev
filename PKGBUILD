# Maintainer : 00ein00 <Ein420@proton.me>

INSTDIR=opt/Aseprite

pkgname=aseprite-dev
pkgver=1.3.11_beta2
pkgrel=1
pkgdesc='Create animated sprites and pixel art'
arch=('x86_64')
url="https://www.aseprite.org/"
license=('LicenseRef-Aseprite-EULA')
makedepends=( # "Meta" dependencies
  cmake ninja git python
  # Aseprite (including e.g. LAF)
  libxi
  # Skia
  gn harfbuzz-icu
  clang
)
provides=('asprite-dev')
conflicts=('aseprite')
post_install() {
  xdg-icon-resource forceupdate --theme hicolor &>/dev/null
  update-desktop-database -q
}

post_upgrade() {
  post_install
}

pre_remove() {
  rm -rvf "/${INSTDIR}"
}

post_remove() {
  xdg-icon-resource forceupdate --theme hicolor &>/dev/null
  update-desktop-database -q
}

package() {

  # Install the binary and its `.desktop` file
  install -vdm 755 Aseprite "$pkgdir/opt/Aseprite/"
  cp -R Aseprite "$pkgdir/opt/"
  install -vDm 644 "$srcdir"/aseprite-dev.desktop "$pkgdir/usr/share/applications/$pkgname.desktop"
  # Install the icons in the correct directory (which is not the default)
  local _size
  for _size in 16 32 48 64 128 256; do
    # The installed icon's name is taken from the `.desktop` file
    install -vDm 644 "$srcdir"/Aseprite/data/icons/ase$_size.png "$pkgdir/usr/share/icons/hicolor/${_size}x$_size/apps/aseprite.png"
  done
  # Create symbolic link
  echo "Creating symbolic link: /opt/Aseprite/ -> /usr/bin/aseprite-dev"
  mkdir -p "$pkgdir"/usr/bin/
  ln -sf "/opt/Aseprite/aseprite" "$pkgdir/usr/bin/$pkgname"
  # Also install the licenses
  install -vDm 644 -t "$pkgdir/usr/share/licenses/$pkgname" Aseprite/data/{EULA.txt,docs/LICENSES.md}
  # Copy the font's license, but leave it in the font directory as well (probably doesn't hurt)
  install -vm 644 "$srcdir"/Aseprite/data/fonts/LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/font.txt"

}
