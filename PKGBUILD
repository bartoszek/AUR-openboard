# Maintainer: Frank Siegert <frank.siegert@googlemail.com>
pkgname=openboard
pkgver=1.5.3
pkgrel=2
pkgdesc="Interactive whiteboard software for schools and universities"
arch=('x86_64' 'i686')
url="http://openboard.ch/index.en.html"
options=(ccache)
license=('GPL3')
depends=('qt5-base' 'qt5-multimedia' 'qt5-svg' 'qt5-script' 'qt5-webkit' 'qt5-tools' 'qt5-xmlpatterns' 'libpaper' 'bzip2' 'openssl' 'libfdk-aac' 'sdl' 'ffmpeg')
depends+=(quazip)  #drop internal quazip and use system one.
source=("https://github.com/OpenBoard-org/OpenBoard/archive/v$pkgver.tar.gz"
        "https://github.com/OpenBoard-org/OpenBoard-ThirdParty/archive/master.zip"
        qchar.patch
        qwebkit.patch
        https://github.com/OpenBoard-org/OpenBoard/pull/218.diff
        https://github.com/OpenBoard-org/OpenBoard/pull/223.diff
        openboard.desktop)
source+=(quazip.diff quazip_libs.pri.diff)
md5sums=('fe3644033dccfd16c80b683210e4ac57'
         'fa1ff089f0bcc15d2a510bb90cdd3002'
         'bf2c524f3897cfcfb4315bcd92d4206e'
         '60f64db6bf627015f4747879c4b30fd3'
         'f484614cc48181287607afb5a45ef644'
         '04c421c140e983d41975943ede5fe61a'
         '21d1749400802f8fc0669feaf77de683'
         '30a7928f696f958d5e8f06e02c49639f'
         '2930ce863ffafdf21fdcf1455b2503c7')

prepare() {
  rm -rf $srcdir/OpenBoard-ThirdParty
  mv "$srcdir/OpenBoard-ThirdParty-master" "$srcdir/OpenBoard-ThirdParty"
  cd $srcdir/OpenBoard-ThirdParty
  patch -p0 < $srcdir/quazip_libs.pri.diff

  cd $srcdir/OpenBoard-$pkgver
  patch -p1 < $srcdir/qchar.patch
  patch -p1 < $srcdir/qwebkit.patch
  patch -p1 < $srcdir/218.diff
  patch -p1 < $srcdir/223.diff
  patch -p1 < $srcdir/quazip.diff
}

build() {
  cd "$srcdir/OpenBoard-ThirdParty"
  
  cd freetype
  qmake freetype.pro -spec linux-g++
  make
  cd ..

  cd xpdf/xpdf-3.04
  ./configure --with-freetype2-library="../../freetype/lib/linux" --with-freetype2-includes="../../freetype/freetype-2.6.1/include"
  cd ..
  qmake xpdf.pro -spec linux-g++
  make
  cd ..

  cd "$srcdir/OpenBoard-$pkgver"
  qmake OpenBoard.pro -spec linux-g++
  make
}

package() {
  cd "$srcdir/OpenBoard-$pkgver"

  mkdir -p $pkgdir/opt/openboard

  for i in customizations etc i18n library; do
    cp -rp $srcdir/OpenBoard-$pkgver/resources/$i $pkgdir/opt/openboard;
  done

  cp -rp $srcdir/OpenBoard-$pkgver/resources/images/OpenBoard.png $pkgdir/opt/openboard/
  cp -rp build/linux/release/product/OpenBoard $pkgdir/opt/openboard/

  mkdir -p $pkgdir/usr/share/applications
  cp $srcdir/openboard.desktop $pkgdir/usr/share/applications 
  mkdir -p $pkgdir/usr/bin
  ln -s /opt/openboard/OpenBoard $pkgdir/usr/bin/openboard
}
