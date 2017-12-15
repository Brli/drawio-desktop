# Maintainer: kitsunyan <kitsunyan@inbox.ru>

pkgname=drawio-desktop
pkgver=7.8.5
pkgrel=2
pkgdesc='Diagram drawing application built on web technology'
arch=('x86_64')
url='https://github.com/jgraph/drawio'
license=('Apache')
depends=(electron gconf libnotify)
conflicts=(drawio-desktop-bin)
source=("https://github.com/jgraph/drawio/archive/v$pkgver.tar.gz")
sha256sums=('cf95074d91dcc39a718b1adf755ed0d8865744416f3adadda2e3c0b1675f4b3b')

prepare() {
  cd "$srcdir/drawio-$pkgver"

  # disable logger and updater
  sed -e "/require('electron-log')/d" \
  -e '/^autoUpdater.* = .*$/d' \
  -e "s/require('electron-updater').autoUpdater/{on: () => {}, checkForUpdates: () => {}}/" \
  -i 'war/electron.js'
}

package() {
  cd "$srcdir/drawio-$pkgver"

  mkdir -p "$pkgdir/usr/share/webapps"
  cp -rp 'war' "$pkgdir/usr/share/webapps/draw.io"
  rm -rf "$pkgdir/usr/share/webapps/draw.io/META-INF"
  rm -rf "$pkgdir/usr/share/webapps/draw.io/WEB-INF"

  # create run script
  mkdir -p "$pkgdir/usr/bin"
  printf '%s\n' \
  '#!/bin/sh' \
  'exec electron /usr/share/webapps/draw.io "$@"' \
  > "$pkgdir/usr/bin/draw.io"
  chmod a+x "$pkgdir/usr/bin/draw.io"

  # create desktop file
  mkdir -p "$pkgdir/usr/share/applications"
  printf '%s\n' \
  '[Desktop Entry]' \
  'Name=draw.io' \
  'Comment=draw.io desktop' \
  'Exec=/bin/bash -c '"'"'exec /usr/bin/draw.io "$@" > /dev/null 2> /dev/null'"'"' -- bash %U' \
  'Terminal=false' \
  'Type=Application' \
  'Icon=draw.io' \
  'Categories=Graphics;' \
  > "$pkgdir/usr/share/applications/draw.io.desktop"

  # create icons
  find 'war/images' -regex '.*/drawlogo[0-9]+\.png' |
  grep -o '[0-9]\+' |
  while read size; do
    install -Dm644 "war/images/drawlogo$size.png" \
    "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/draw.io.png"
  done

  install -Dm644 'LICENSE' "$pkgdir/usr/share/licenses/drawio-desktop/LICENSE"
}
