# Maintainer: Anatol Pomozov
# Contributor: 謝致邦 <Yeking@Red54.com>
# Contributor: Alucryd <alucryd at gmail dot com>

pkgname=android-tools
pkgver=8.0.0_r4
pkgrel=2
pkgdesc='Android platform tools'
arch=(i686 x86_64)
url='http://tools.android.com/'
license=(Apache MIT)
depends=(pcre2 libusb)
optdepends=('python: for mkbootimg script')
makedepends=(git clang gtest ruby cmake ninja go)
source=(git+https://android.googlesource.com/platform/system/core#tag=android-$pkgver
        git+https://android.googlesource.com/platform/system/extras#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/selinux#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/f2fs-tools#tag=android-$pkgver
        git+https://boringssl.googlesource.com/boringssl
        generate_build.rb
        fix_build_core.patch
        fix_build_selinux.patch
        bash_completion.fastboot)
        # Bash completion file was taken from https://github.com/mbrubeck/android-completion
sha1sums=('SKIP'
          'SKIP'
          'SKIP'
          'SKIP'
          'SKIP'
          '12b6bc1cbf850958850c3e4a5bc19d8b32f845b9'
          '45e41bab3633bb0be96b238aae3164a5c90721f1'
          'ec473160d7445f97bccabd1c32ac0ae2f77900c1'
          '7004dbd0c193668827174880de6f8434de8ceaee')

prepare() {
  PKGVER=$pkgver ./generate_build.rb > build.ninja

  cd $srcdir/core
  patch -p1 < ../fix_build_core.patch

  cd $srcdir/selinux
  patch -p1 < ../fix_build_selinux.patch

  mkdir $srcdir/boringssl/build && cd $srcdir/boringssl/build && cmake -GNinja ..; ninja
}

build() {
  ninja
}

package(){
  install -m755 -d "$pkgdir"/usr/bin
  install -m755 -t "$pkgdir"/usr/bin fastboot adb core/mkbootimg/mkbootimg
  install -Dm 644 bash_completion.fastboot "$pkgdir"/usr/share/bash-completion/completions/fastboot
}
