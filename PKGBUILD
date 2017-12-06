# Maintainer: Anatol Pomozov
# Contributor: 謝致邦 <Yeking@Red54.com>
# Contributor: Alucryd <alucryd at gmail dot com>

pkgname=android-tools
pkgver=8.1.0_r1
pkgrel=1
pkgdesc='Android platform tools'
arch=(x86_64)
url='http://tools.android.com/'
license=(Apache MIT)
depends=(pcre2 libusb)
optdepends=('python: for mkbootimg script')
makedepends=(git clang gtest ruby cmake ninja go-pie)
_boringssl_commit=14308731e5446a73ac2258688a9688b524483cb6 # keep it in sync with android https://android.googlesource.com/platform/external/boringssl/+/$pkgver/BORINGSSL_REVISION
source=(git+https://android.googlesource.com/platform/system/core#tag=android-$pkgver
        git+https://android.googlesource.com/platform/system/extras#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/selinux#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/f2fs-tools#tag=android-$pkgver
        git+https://boringssl.googlesource.com/boringssl#commit=$_boringssl_commit
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
          'fa0e4579f2f26a0e0efe53549e9a91ef3a334eff'
          '640db48e8e915f8c7f525dd044f16a3b502d4689'
          'ec473160d7445f97bccabd1c32ac0ae2f77900c1'
          '7004dbd0c193668827174880de6f8434de8ceaee')

prepare() {
  PKGVER=$pkgver ./generate_build.rb > build.ninja

  cd $srcdir/core
  patch -p1 < ../fix_build_core.patch

  cd $srcdir/selinux
  patch -p1 < ../fix_build_selinux.patch

  mkdir -p $srcdir/boringssl/build && cd $srcdir/boringssl/build && cmake -GNinja ..; ninja
}

build() {
  ninja
}

package(){
  install -m755 -d "$pkgdir"/usr/bin
  install -m755 -t "$pkgdir"/usr/bin fastboot adb core/mkbootimg/mkbootimg
  install -Dm 644 bash_completion.fastboot "$pkgdir"/usr/share/bash-completion/completions/fastboot
}
