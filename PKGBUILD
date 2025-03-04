# Maintainer: Anatol Pomozov
# Contributor: 謝致邦 <Yeking@Red54.com>
# Contributor: Alucryd <alucryd at gmail dot com>

pkgname=android-tools
pkgver=35.0.2
_tag=${pkgver} # https://github.com/nmeum/android-tools sometimes carries extra patch version on top of the upstream versioning
pkgrel=11
pkgdesc='Android platform tools'
arch=(x86_64)
url='http://tools.android.com/'
license=(Apache MIT)
depends=(fmt protobuf brotli zstd android-udev pcre2)
makedepends=(gtest cmake go ninja git)
optdepends=('python: {mk,unpack_,repack_}bootimg and mkdtboimg support')
source=(https://github.com/nmeum/android-tools/releases/download/$_tag/android-tools-$_tag.tar.xz)
sha256sums=('d2c3222280315f36d8bfa5c02d7632b47e365bfe2e77e99a3564fb6576f04097')

build() {
  cd android-tools-$_tag

  # android-tools uses libusb API that has not been released yet
  # use newer bundled libusb until the new libusb release arrive

  cmake \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_CXX_FLAGS="$CXXFLAGS" \
    -DCMAKE_C_FLAGS="$CFLAGS" \
    -DCMAKE_FIND_PACKAGE_PREFER_CONFIG=ON \
    -Dprotobuf_MODULE_COMPATIBLE=ON \
    -DANDROID_TOOLS_LIBUSB_ENABLE_UDEV=ON \
    -DANDROID_TOOLS_USE_BUNDLED_LIBUSB=ON \
    -G Ninja -S . -B build
  cmake --build build
}

package() {
  cd android-tools-$_tag

  DESTDIR="${pkgdir}" ninja -C build install
}
