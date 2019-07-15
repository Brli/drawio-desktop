# Maintainer: kitsunyan <`echo a2l0c3VueWFuQGFpcm1haWwuY2MK | base64 -d`>

pkgname=drawio-desktop
pkgver=10.9.6
pkgrel=1
pkgdesc='Diagram drawing application built on web technology'
arch=('x86_64')
url='https://github.com/jgraph/drawio'
license=('Apache')
depends=(electron)
makedepends=(yarn)
source=("drawio-desktop-$pkgver.zip::https://github.com/jgraph/drawio/releases/download/v$pkgver/draw.war")
sha256sums=('1c0bf26f52c3c5f1a66f9be005d83946be47fea43737888d43819b7ff5202498')

prepare() {
  rm -rfv "META-INF" "WEB-INF" "drawio-desktop-$pkgver.zip"
}

build() {
  yarn install --cache ../npm-cache --only=production
  yarn autoclean -I
  yarn autoclean -F
  rm -rfv yarn.lock
  rm -f 'package-lock.json'
  rm -rfv node_modules/{electron,.bin}
  find $srcdir -maxdepth 10 -name 'package.json' -exec sed 's,/tmp/makepkg/drawio-desktop/src,/usr/lib/draw.io,g' -i {} +
}

package() {
  install -dm755 "$pkgdir/usr/lib"
  cp -rp . "$pkgdir/usr/lib/draw.io"

  # create run script
  install -Dm755 /dev/stdin "$pkgdir/usr/bin/draw.io" <<END
#!/bin/sh
exec electron /usr/lib/draw.io "$@" > /dev/null 2> /dev/null
END

  # create desktop file
  install -Dm644 /dev/stdin "$pkgdir/usr/share/applications/draw.io.desktop" <<END
[Desktop Entry]
Name=draw.io
Comment=draw.io desktop
Exec=/usr/bin/draw.io %U
Terminal=false
Type=Application
Icon=draw.io
Categories=Graphics;
END

  # create icons
  find 'images' -regex '.*/drawlogo[0-9]+\.png' |
  grep -o '[0-9]\+' |
  while read size; do
    install -Dm644 "images/drawlogo$size.png" \
    "$pkgdir/usr/share/icons/hicolor/${size}x${size}/apps/draw.io.png"
  done
}
