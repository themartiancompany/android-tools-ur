# Maintainer: Anatol Pomozov
# Contributor: 謝致邦 <Yeking@Red54.com>
# Contributor: Alucryd <alucryd at gmail dot com>

pkgname=android-tools
pkgver=9.0.0_r45
pkgrel=1
pkgdesc='Android platform tools'
arch=(x86_64)
url='http://tools.android.com/'
license=(Apache MIT)
depends=(pcre2 libusb)
optdepends=('python: for mkbootimg script')
makedepends=(git clang gtest ruby cmake ninja go-pie)
provides=(fastboot adb)
conflicts=(fastboot adb)
_boringssl_commit=`curl https://android.googlesource.com/platform/external/boringssl/+/refs/tags/android-$pkgver/BORINGSSL_REVISION?format=TEXT | base64 -d`
source=(git+https://android.googlesource.com/platform/system/core#tag=android-$pkgver
        git+https://android.googlesource.com/platform/system/extras#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/selinux#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/f2fs-tools#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/e2fsprogs#tag=android-$pkgver
        git+https://android.googlesource.com/platform/external/avb#tag=android-$pkgver
        git+https://boringssl.googlesource.com/boringssl#commit=$_boringssl_commit
        generate_build.rb
        fix_build_core.patch
        fix_build_selinux.patch
        fix_build_e2fsprogs.patch
        bash_completion.fastboot)
        # Bash completion file was taken from https://github.com/mbrubeck/android-completion
sha1sums=('SKIP'
          'SKIP'
          'SKIP'
          'SKIP'
          'SKIP'
          'SKIP'
          'SKIP'
          '238507086a99134820cc9900545cbff06772dc30'
          '62446582a96b3a39e5d91e3e2ef8b8b38a5a735e'
          'ec473160d7445f97bccabd1c32ac0ae2f77900c1'
          '5df8c7e00a4066733d59050e8e1fcd4cc2b22104'
          '7004dbd0c193668827174880de6f8434de8ceaee')

prepare() {
  # for the Android version calculation see FS#63200
  android_sdk_ver=`curl https://android.googlesource.com/platform/development/+/refs/tags/android-$pkgver/sdk/plat_tools_source.prop_template?format=TEXT | base64 -d | sed -n 's/^Pkg.Revision=\(.*\)$/\1/p'`
  PKGVER="$android_sdk_ver-$pkgver" LDFLAGS='-Wl,-z,relro,-z,now' ./generate_build.rb > build.ninja

  cd $srcdir/core
  patch -p1 < ../fix_build_core.patch

  cd $srcdir/selinux
  patch -p1 < ../fix_build_selinux.patch

  cd $srcdir/e2fsprogs
  patch -p1 < ../fix_build_e2fsprogs.patch

  cd $srcdir/avb
  sed -i 's|/usr/bin/env python$|/usr/bin/env python2|g' avbtool

  mkdir -p $srcdir/boringssl/build && cd $srcdir/boringssl/build && cmake -GNinja ..; ninja
}

build() {
  ninja
}

package(){
  install -m755 -d "$pkgdir"/usr/bin
  install -m755 -t "$pkgdir"/usr/bin fastboot adb mke2fs.android e2fsdroid ext2simg core/mkbootimg/mkbootimg avb/avbtool
  install -Dm 644 bash_completion.fastboot "$pkgdir"/usr/share/bash-completion/completions/fastboot
}
