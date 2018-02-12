# Maintainer: kitsunyan <kitsunyan@inbox.ru>

pkgname=drawio-desktop
pkgver=8.1.2
pkgrel=1
pkgdesc='Diagram drawing application built on web technology'
arch=('x86_64')
url='https://github.com/jgraph/drawio'
license=('Apache')
depends=(electron gconf libnotify)
conflicts=(drawio-desktop-bin)
makedepends=(npm)
source=("drawio-desktop-$pkgver.zip::https://github.com/jgraph/drawio/releases/download/v$pkgver/draw.war")
noextract=("drawio-desktop-$pkgver.zip")
sha256sums=('37c069a32a3a0987d89eca8ae31ad8ee21fa8d502030ec0887edacd0565745c7')

prepare() {
  rm -rf "$srcdir/drawio-$pkgver"
  mkdir "$srcdir/drawio-$pkgver"
  cd "$srcdir/drawio-$pkgver"

  bsdtar -xf "../drawio-desktop-$pkgver.zip" -C .
  rm -rf "META-INF" "WEB-INF"
}

build() {
  cd "$srcdir/drawio-$pkgver"

  npm install --cache ../npm-cache --only=production
  rm -f 'package-lock.json'
}

package() {
  cd "$srcdir/drawio-$pkgver"

  mkdir -p "$pkgdir/usr/lib"
  cp -rp . "$pkgdir/usr/lib/draw.io"

  # create run script
  mkdir -p "$pkgdir/usr/bin"
  printf '%s\n' \
  '#!/bin/sh' \
  'exec electron /usr/lib/draw.io "$@" > /dev/null 2> /dev/null' \
  > "$pkgdir/usr/bin/draw.io"
  chmod a+x "$pkgdir/usr/bin/draw.io"

  # create desktop file
  mkdir -p "$pkgdir/usr/share/applications"
  printf '%s\n' \
  '[Desktop Entry]' \
  'Name=draw.io' \
  'Comment=draw.io desktop' \
  'Exec=/usr/bin/draw.io %U' \
  'Terminal=false' \
  'Type=Application' \
  'Icon=draw.io' \
  'Categories=Graphics;' \
  > "$pkgdir/usr/share/applications/draw.io.desktop"

  # create icons
  find 'images' -regex '.*/drawlogo[0-9]+\.png' |
  grep -o '[0-9]\+' |
  while read size; do
    install -Dm644 "images/drawlogo$size.png" \
    "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/draw.io.png"
  done
}
