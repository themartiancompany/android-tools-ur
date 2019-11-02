# Maintainer: Anatol Pomozov
# Contributor: 謝致邦 <Yeking@Red54.com>
# Contributor: Alucryd <alucryd at gmail dot com>

pkgname=android-tools
pkgver=29.0.5
pkgrel=1
tag=platform-tools-$pkgver
pkgdesc='Android platform tools'
arch=(x86_64)
url='http://tools.android.com/'
license=(Apache MIT)
depends=(pcre2 libusb protobuf)
optdepends=('python: for mkbootimg script'
            'python2: for unpack_bootimg & avbtool scripts')
# depend on 'vim' for 'xxd' tool.
makedepends=(git clang gtest ruby cmake ninja go-pie vim)
provides=(fastboot adb)
conflicts=(fastboot adb)
_boringssl_commit=$(curl https://android.googlesource.com/platform/external/boringssl/+/refs/tags/$tag/BORINGSSL_REVISION?format=TEXT | base64 -d)
source=(git+https://android.googlesource.com/platform/frameworks/base#tag=$tag
        git+https://android.googlesource.com/platform/frameworks/native#tag=$tag
        git+https://android.googlesource.com/platform/system/core#tag=$tag
        git+https://android.googlesource.com/platform/system/extras#tag=$tag
        git+https://android.googlesource.com/platform/system/tools/mkbootimg#tag=$tag
        git+https://android.googlesource.com/platform/external/selinux#tag=$tag
        git+https://android.googlesource.com/platform/external/f2fs-tools#tag=$tag
        git+https://android.googlesource.com/platform/external/e2fsprogs#tag=$tag
        git+https://android.googlesource.com/platform/external/avb#tag=$tag
        git+https://boringssl.googlesource.com/boringssl#commit=$_boringssl_commit
        generate_build.rb
# deployagent.jar is a library built from Android sources.
# Building this java library requires a lot of dependencies:
#  java, protobuf-java, dex compiler, Android base libs.
# To avoid the complexity we prebuilt the lib from the Android sources directly
# using following instructiuons:
#   (See https://wiki.archlinux.org/index.php/Android for context)
# 
#   source build/envsetup.sh
#   lunch full-eng
#   mmm system/core/adb/
#   cp ./target/product/generic/system/framework/deployagent.jar .
        deployagent.jar
        fix_build_core.patch
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
          'SKIP'
          'SKIP'
          'acb02d8c048411fd21d00d57909a45aa6a6a6a7a'
          'd9dfac30245faa0a96968b96f3acd9ad536f4910'
          '31779cd6c0df710be9589bd2ee4f697f59b100fd'
          '7004dbd0c193668827174880de6f8434de8ceaee')

prepare() {
  PLATFORM_TOOLS_VERSION="$pkgver-$pkgrel" LDFLAGS='-Wl,-z,relro,-z,now' ./generate_build.rb > build.ninja

  cd "$srcdir"/core
  patch -p1 < ../fix_build_core.patch

  cd "$srcdir"/avb
  sed -i 's|/usr/bin/env python$|/usr/bin/env python2|g' avbtool

  cd "$srcdir"/mkbootimg
  sed -i 's|/usr/bin/env python$|/usr/bin/env python2|g' unpack_bootimg.py

  mkdir -p "$srcdir"/boringssl/build && cd "$srcdir"/boringssl/build && cmake -GNinja ..; ninja crypto/libcrypto.a
}

build() {
  ninja
}

package() {
  install -m755 -d "$pkgdir"/usr/bin
  install -m755 -t "$pkgdir"/usr/bin fastboot adb mke2fs.android e2fsdroid ext2simg avb/avbtool
  install -Dm 755 mkbootimg/mkbootimg.py "$pkgdir"/usr/bin/mkbootimg
  install -Dm 755 mkbootimg/unpack_bootimg.py "$pkgdir"/usr/bin/unpack_bootimg
  install -Dm 644 bash_completion.fastboot "$pkgdir"/usr/share/bash-completion/completions/fastboot
}
