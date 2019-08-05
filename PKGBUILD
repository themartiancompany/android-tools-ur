# Maintainer: Anatol Pomozov
# Contributor: 謝致邦 <Yeking@Red54.com>
# Contributor: Alucryd <alucryd at gmail dot com>

pkgname=android-tools
pkgver=29.0.2
pkgrel=1
tag=platform-tools-$pkgver
pkgdesc='Android platform tools'
arch=(x86_64)
url='http://tools.android.com/'
license=(Apache MIT)
depends=(pcre2 libusb)
optdepends=('python: for mkbootimg script')
makedepends=(git clang gtest ruby cmake ninja go-pie)
provides=(fastboot adb)
conflicts=(fastboot adb)
_boringssl_commit=`curl https://android.googlesource.com/platform/external/boringssl/+/refs/tags/$tag/BORINGSSL_REVISION?format=TEXT | base64 -d`
source=(git+https://android.googlesource.com/platform/system/core#tag=$tag
        git+https://android.googlesource.com/platform/system/extras#tag=$tag
        git+https://android.googlesource.com/platform/system/tools/mkbootimg#tag=$tag
        git+https://android.googlesource.com/platform/external/selinux#tag=$tag
        git+https://android.googlesource.com/platform/external/f2fs-tools#tag=$tag
        git+https://android.googlesource.com/platform/external/e2fsprogs#tag=$tag
        git+https://android.googlesource.com/platform/external/avb#tag=$tag
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
          'SKIP'
          'ff8613a331b9026f2f413768f88ccd10e26149bf'
          '99a1618bd93af8ef3ff2cca893e950a0346021fe'
          'b2ccf6dac3577d230f910e668ae70af6051fee46'
          'bcebdf1e706a3c3da175234840c6ee4e13652012'
          '7004dbd0c193668827174880de6f8434de8ceaee')

prepare() {
  PLATFORM_TOOLS_VERSION="$pkgver" LDFLAGS='-Wl,-z,relro,-z,now' ./generate_build.rb > build.ninja

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
  install -m755 -t "$pkgdir"/usr/bin fastboot adb mke2fs.android e2fsdroid ext2simg avb/avbtool
  install -Dm 755 mkbootimg/mkbootimg.py "$pkgdir"/usr/bin/mkbootimg
  install -Dm 755 mkbootimg/unpack_bootimg.py "$pkgdir"/usr/bin/unpack_bootimg
  install -Dm 644 bash_completion.fastboot "$pkgdir"/usr/share/bash-completion/completions/fastboot
}
