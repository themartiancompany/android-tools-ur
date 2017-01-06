# Maintainer: Anatol Pomozov
# Contributor: 謝致邦 <Yeking@Red54.com>
# Contributor: Alucryd <alucryd at gmail dot com>

pkgname=android-tools
pkgver=7.1.1_r13
pkgrel=1
pkgdesc='Android platform tools'
arch=(i686 x86_64)
url='http://tools.android.com/'
license=(Apache MIT)
depends=(openssl pcre)
optdepends=('python: for mkbootimg script')
makedepends=(git clang gtest)
source=(git+https://android.googlesource.com/platform/system/core#tag=android-$pkgver
        git+https://android.googlesource.com/platform/system/extras#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/libselinux#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/f2fs-tools#tag=android-$pkgver
        build.sh # regenerate this file with generate_build.rb tool
        fix_build.patch
        bash_completion.fastboot
        bash_completion.adb) # Bash completion file was taken from https://github.com/mbrubeck/android-completion
sha1sums=('SKIP'
          'SKIP'
          'SKIP'
          'SKIP'
          '84dd43a0102e35fcdf0a855c617e1d151f49fb2a'
          '33538c9161c199f1e608d3b8f519adb1cd9d46d5'
          '7004dbd0c193668827174880de6f8434de8ceaee'
          '2e69152091bb9642be058e49ec6cb720a2fd91dc')

prepare() {
  patch -p1 < fix_build.patch
}

build() {
  PKGVER=$pkgver sh ./build.sh
}

package(){
  install -m755 -d "$pkgdir"/usr/bin
  install -m755 -t "$pkgdir"/usr/bin fastboot adb core/mkbootimg/mkbootimg
  install -Dm 644 bash_completion.fastboot "$pkgdir"/usr/share/bash-completion/completions/fastboot
  #adb completion is provided by bash-completion file now
  #install -Dm 644 bash_completion.adb "$pkgdir"/usr/share/bash-completion/completions/adb
}
