# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel Bermond <dbermond@archlinux.org>

_pkg=lcevcdec
_Pkg="LCEVCdec"
pkgname="${_pkg}"
pkgver=3.2.1
pkgrel=1
_pkgdesc=(
  'Low Complexity Enhancement'
  'Video Codec Decoder (LCEVC_DEC)'
)
arch=(
  'x86_64'
)
_http="https://github.com"
_ns="v-novaltd"
url="${_http}/${_ns}/${_Pkg}"
license=(
  'BSD-3-Clause-Clear'
)
depends=(
  'fmt'
)
makedepends=(
  'git'
  'cmake'
  'python'
  'range-v3'
  'rapidjson'
)
source=(
  "git+${url}.git#tag=${pkgver}"
  "010-${_pkg}-fix-pkgconfig-prefix.patch"
  "020-${_pkg}-fix__builtin_clzg-arguments.patch::${url}/commit/43ef5a17ec1ced77f834136945b3cbfe2e46b9b4.patch"
)
sha256sums=(
  '1e6b110e235ddcbc124f3fd1c8a7c7fa1603ec08dbc8b58c59c8f1a8995c9c0b'
  '8d4ed24ba3407f9ebb8397960bfebeebcd973f3f3febad6b52768530451d5b73'
  '940faa1bdd443841113f0b3e35200bee3a6f088d6353cc31d49ac5e598343856')

prepare() {
    patch \
      -d "${_Pkg}" \
      -Np1 \
      -i \
      "${srcdir}/010-${_pkg}-fix-pkgconfig-prefix.patch"
    patch \
      -d "${_Pkg}" \
      -Np1 \
      -i \
    "${srcdir}/020-${_pkg}-fix__builtin_clzg-arguments.patch"
}

build() {
  local \
    _cmake_opts=()
  _cmake_opts=(
    -DCMAKE_BUILD_TYPE:STRING='None'
    -DCMAKE_INSTALL_PREFIX:PATH='/usr'
    -DVN_SDK_FFMPEG_LIBS_PACKAGE:STRING=''
    -DVN_SDK_SAMPLE_SOURCE:BOOL='OFF'
    -DVN_SDK_UNIT_TESTS:BOOL='OFF'
    -Wno-dev
  )
  cmake \
    -B \
      build \
    -S "${_Pkg}" \
    "${_cmake_opts[@]}"
  cmake \
    --build \
      build
}

check() {
  ctest \
    --test-dir \
      build \
    --output-on-failure
}

package() {
  DESTDIR="${pkgdir}" \
  cmake \
    --install \
      "build"
  mv \
    "${pkgdir}/usr/pkgconfig"
    "${pkgdir}/usr/lib"
  install \
    -Dm644 \
    "${_Pkg}/LICENSE.md" \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  rm \
    "${pkgdir}/usr/"{"COPYING","README.md","conanfile.txt"}
  rm \
    -r \
    "${pkgdir}/usr"/{"licenses","src"}
}
