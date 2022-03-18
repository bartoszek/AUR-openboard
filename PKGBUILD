# Maintainer: Frank Siegert <frank.siegert@googlemail.com>
# Contributor: bartus <arch-user-repoᘓbartus.33mail.com>
pkgname=openboard
pkgver=1.6.1
_src_folder="OpenBoard-${pkgver}"
pkgrel=4
pkgdesc="Interactive whiteboard software for schools and universities"
arch=('x86_64' 'i686')
url="http://openboard.ch/index.en.html"
license=('GPL3')
depends=('qt5-base' 'qt5-multimedia' 'qt5-svg' 'qt5-script' 'qt5-webkit' 'qt5-tools' 'qt5-xmlpatterns' 'libpaper' 'bzip2' 'openssl' 'libfdk-aac' 'sdl' 'ffmpeg4.4')
depends+=(quazip)  #drop internal quazip and use system one.
depends+=(poppler) #replace internal xpdf with poppler and drop freetype/xpdf from deps
makedepends=('patch')
source=("https://github.com/OpenBoard-org/OpenBoard/archive/v${pkgver}.tar.gz"
        openboard.desktop)
source+=(qchar.patch)
source+=(quazip.patch)
source+=(c++17.patch)
source+=(drop_ThirdParty_repo.patch)
source+=(ffmpeg4.4.patch)
sha256sums=('cf5bfb570b9ac4e61e1670c5a433f1dcaf0de1e8dbcbd544f058711690afba79'
            '64289f9d91cb25fa79fb988f19d43a542d67380fcf27668d0da1ee4ba1e705eb'
            'b40fdab85f5921d0404c07db64628a2428a87d39193d2797bbef2e69b1d51549'
            '177559f359b0a6d7e407fc9c44dedfab2d492b5cc9334e1d763b16fdf2efc683'
            '7b3ca090cee096f47b27f29f2a1a956b4afcaa08338a084fcb20cd7f01d71e26'
            'a6a9bc1f9c9bee0345b735fcf422245ae7946f96f6c34520dd63530a98978c14'
            '071c63d43a3fdfed9048e8e1c8821861d2bcaa621c06bf51cd35d579f825396c')

prepare() {
  cd "$srcdir"/$_src_folder
  msg2 "drop_ThirdParty_repo"
  patch -f -p1 < "$srcdir"/drop_ThirdParty_repo.patch || true
  msg2 "qchar"
  patch -p1 < "$srcdir"/qchar.patch
  msg2 "quazip"
  patch -f -p1 < "$srcdir"/quazip.patch
  msg2 "cpp17"
  patch -f -p1 < "$srcdir"/c++17.patch
  msg2 "ffmpeg4.4"
  patch -f -p1 < "$srcdir"/ffmpeg4.4.patch
  msg2 "gcc11"
  sed 's/_serialize/serialize/g' -i src/pdf-merger/Object.{h,cpp}
}

build() {
  cd "$srcdir"/$_src_folder
# convert translations to binary form
  lrelease OpenBoard.pro
  qmake OpenBoard.pro -spec linux-g++
  make
}

package() {
  cd "$srcdir"/$_src_folder

  install -Dm755 build/linux/release/product/OpenBoard -t "$pkgdir"/opt/openboard/
  cp -rp "$srcdir"/$_src_folder/resources/{customizations,etc,i18n,library} -t "$pkgdir"/opt/openboard/
  install -Dm644 "$srcdir"/$_src_folder/resources/images/OpenBoard.png -t "$pkgdir"/usr/share/icons/hicolor/64x64/apps/
  install -Dm644 "$srcdir"/openboard.desktop -t "$pkgdir"/usr/share/applications/
  install -dm755 "$pkgdir"/usr/bin/
  ln -s /opt/openboard/OpenBoard "$pkgdir"/usr/bin/openboard
}
