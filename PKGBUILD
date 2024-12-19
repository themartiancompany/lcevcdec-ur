# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel Bermond <dbermond@archlinux.org>

_git="false"
_pkg=lcevcdec
_Pkg="LCEVCdec"
pkgname="${_pkg}"
pkgver=3.2.1
_commit="052a08b9f17dbe1bec46c1f6b6accca196b7794c"
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
  'cmake'
  'python'
  'range-v3'
  'rapidjson'
)
_tarname="${_Pkg}-${pkgver}"
_tag_name="pkgver"
_tag="${pkgver}"
_url="${url}"
if [[ "${_git}" == true ]]; then
  makedepends+=(
    "git"
  )
  _src="${_tarname}::git+${_url}#${_tag_name}=${_tag}?signed"
  _sum="SKIP"
elif [[ "${_git}" == false ]]; then
  if [[ "${_tag_name}" == 'pkgver' ]]; then
    _src="${_tarname}.tar.gz::${_url}/archive/refs/tags/${_tag}.tar.gz"
    _sum="4934b517df614f96ae955c397e2f69088743c4c211184bae58aba76d189efe2d"
  elif [[ "${_tag_name}" == "commit" ]]; then
    _src="${_tarname}.zip::${_url}/archive/${_commit}.zip"
    _sum='a7c3d4689e85d69c45e8dc2003ca339b6613787845f8dc0c88cb6b57a3989634'
  fi
fi
source=(
  "${_src}"
  "010-${_pkg}-fix-pkgconfig-prefix.patch"
  "020-${_pkg}-fix__builtin_clzg-arguments.patch::${url}/commit/43ef5a17ec1ced77f834136945b3cbfe2e46b9b4.patch"
)
sha256sums=(
  "${_sum}"
  '8d4ed24ba3407f9ebb8397960bfebeebcd973f3f3febad6b52768530451d5b73'
  '940faa1bdd443841113f0b3e35200bee3a6f088d6353cc31d49ac5e598343856'
)

prepare() {
  rm \
    -rf \
    "${srcdir}/${_tarname}"
  git \
    init \
      "${_tarname}"
  cd \
    "${_tarname}"
  git \
    remote \
      add \
      "origin" \
      "${url}"
  git \
    fetch \
      origin \
      "${_commit}"
  git \
    reset \
      --hard \
      FETCH_HEAD
  git \
    fetch \
      "origin" \
        "main" \
      --tags
  cd \
    "${srcdir}"
  patch \
    -d "${_tarname}" \
    -Np1 \
    -i \
    "${srcdir}/010-${_pkg}-fix-pkgconfig-prefix.patch"
  patch \
    -d "${_tarname}" \
    -Np1 \
    -i \
  "${srcdir}/020-${_pkg}-fix__builtin_clzg-arguments.patch"
}

build() {
  local \
    _cmake_opts=()
  _cmake_opts=(
    -G Ninja
    -DCMAKE_BUILD_TYPE:STRING='Release'
    -DCMAKE_INSTALL_PREFIX:PATH='/usr'
    -DVN_SDK_FFMPEG_LIBS_PACKAGE:STRING=''
    -DVN_SDK_SAMPLE_SOURCE:BOOL='OFF'
    -DVN_SDK_UNIT_TESTS:BOOL='OFF'
    -Wno-dev
  )
  cmake \
    -B \
      build \
    -S "${_tarname}" \
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
    "${_tarname}/LICENSE.md" \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  rm \
    "${pkgdir}/usr/"{"COPYING","README.md","conanfile.txt"}
  rm \
    -r \
    "${pkgdir}/usr"/{"licenses","src"}
}
