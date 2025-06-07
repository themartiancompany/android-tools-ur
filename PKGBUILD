# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer:
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer:
#   Anatol Pomozov
# Contributor:
#   謝致邦
#     <Yeking@Red54.com>
# Contributor:
#   Alucryd
#     <alucryd at gmail dot com>

_py="python"
_proj="android"
_domain="${_proj}.com"
_component="tools"
_pkg="${_proj}-${_component}"
pkgname="${_pkg}"
pkgver=35.0.2
# https://github.com/nmeum/android-tools
# sometimes carries extra patch version on
# top of the upstream versioning
_tag="${pkgver}"
pkgrel=16
_pkgdesc=(
  'Android platform tools'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'x86_64'
)
url="http://${_component}.${_domain}"
license=(
  'Apache'
  'MIT'
)
depends=(
  "${_proj}-udev"
  "fmt"
  "protobuf"
  "brotli"
  "pcre2"
  "zstd"
)
makedepends=(
  "gtest"
  "cmake"
  "go"
  "ninja"
  "git"
)
optdepends=(
  "${_py}: {mk,unpack_,repack_}bootimg and mkdtboimg support"
)
_http="https://github.com"
_ns="nmeum"
_url="${_http}/${_ns}/${_pkg}"
_tarname="${_pkg}-${_tag}"
source=(
  "${_tarname}.tar.xz::${_url}/releases/download/${_tag}/${_tarname}.tar.xz"
  "${_tarname}-fix-protobuf-30.0-compilation.patch"
)
sha256sums=(
  'd2c3222280315f36d8bfa5c02d7632b47e365bfe2e77e99a3564fb6576f04097'
  'cd2034ca35c3b5ca82f095106cd099abdbc5a682b7b9892eb0ebead616370e96'
)

prepare() {
  cd \
    "${_tarname}"
  patch \
    -Np \
      1 \
    -d \
      "vendor/extras" < \
    "../${_tarname}-fix-protobuf-30.0-compilation.patch"

}

build() {
  local \
    _cmake_opts=()
  cd \
    "${_tarname}"
  # android-tools uses libusb API that has not been released yet
  # use newer bundled libusb until the new libusb release arrive
  _cmake_opts+=(
    -DCMAKE_INSTALL_PREFIX="/usr"
    -DCMAKE_BUILD_TYPE="Release"
    -DCMAKE_CXX_FLAGS="$CXXFLAGS"
    -DCMAKE_C_FLAGS="$CFLAGS"
    -DCMAKE_FIND_PACKAGE_PREFER_CONFIG="ON"
    -Dprotobuf_MODULE_COMPATIBLE="ON"
    -DANDROID_TOOLS_LIBUSB_ENABLE_UDEV="ON"
    -DANDROID_TOOLS_USE_BUNDLED_LIBUSB="ON"
    -G
      "Ninja"
    -S
      "."
    -B
      "build"
  )
  cmake \
    "${_cmake_opts[@]}"
  cmake \
    --build \
      "build"
}

package() {
  cd \
    "${_tarname}"
  DESTDIR="${pkgdir}" \
  ninja \
    -C \
      "build" \
    install
}
