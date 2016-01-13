# Maintainer: Anatol Pomozov
# Contributor: 謝致邦 <Yeking@Red54.com>
# Contributor: Alucryd <alucryd at gmail dot com>

pkgname=android-tools
pkgver=6.0.1_r10
pkgrel=1
pkgdesc='Android platform tools'
arch=(i686 x86_64)
url='http://tools.android.com/'
license=(Apache MIT)
depends=(openssl pcre)
makedepends=(git)
source=(git+https://android.googlesource.com/platform/system/core#tag=android-$pkgver
        git+https://android.googlesource.com/platform/system/extras#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/libselinux#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/f2fs-tools#tag=android-$pkgver
        build.sh
        fix_build.patch
        bash_completion) # Bash completion file was taken from https://github.com/mbrubeck/android-completion
sha1sums=('SKIP'
          'SKIP'
          'SKIP'
          'SKIP'
          '2bf75e141d4dbbc04ad23c19b3d5e67300de3edd'
          '40a978209b6d1bbf99e04b8bd5fca6429b97f1b1'
          'e1bd94fd4dd260af3c068496071d67738d431aec')

prepare() {
  patch -p1 < ../fix_build.patch
}

build() {
  PKGVER=$pkgver bash build.sh
}

package(){
  install -m755 -d "$pkgdir"/usr/bin
  install -m755 -t "$pkgdir"/usr/bin mkbootimg fastboot adb
  install -Dm 644 bash_completion "$pkgdir"/usr/share/bash-completion/completions/$pkgname
}
