# Maintainer: Daniel Bermond <dbermond@archlinux.org>

_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
elif [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
fi
_proj="mpeg5"
_pkg=xevd
pkgname="${_pkg}"
pkgver=0.5.0
pkgrel=1
pkgdesc='MPEG-5 EVC (Essential Video Coding) decoder'
arch=(
  'x86_64'
  'i686'
  'arm'
  'aarch64'
  'armv7l'
  'armv6l'
  'mips'
  'pentium4'
  'powerpc'
)
_http="https://github.com"
_ns="${_proj}"
url="${_http}/${_ns}/${_pkg}"
license=(
  'BSD-3-Clause'
)
depends=(
  "${_libc}"
)
makedepends=(
  'cmake'
)
options=(
  '!emptydirs'
)
_tarname="${_pkg}-${pkgver}"
_patches=(
  "010-${_pkg}-disable-werror.patch"
  "020-${_pkg}-fix-pkg-config.patch"
)
source=(
  "${_http}/archive/v${pkgver}/${_tarname}.tar.gz"
  "${_patches[@]}"
)
sha256sums=(
  '8d55c7ec1a9ad4e70fe91fbe129a1d4dd288bce766f466cba07a29452b3cecd8'
  '2a7eff2690c0d4d441df97ad37fd7a0e3e0a03705665dad12201f8d8d997f191'
  '28e46788d188dbbd27c0b47d2c4510029491f434cccfa41967b60d94def36d4a'
)

prepare() {
  local \
    _patch
  printf '%s\n' "v${pkgver}" > "${pkgname}-${pkgver}/version.txt"
  for _patch in "${_patches[@]}"; do
    patch \
      -d \
        "${_tarname}" \
      -Np1 \
      -i \
        "${srcdir}/${_patch}"
  done
}

build() {
  local \
    _cmake_opts=()
  _cmake_opts=(
    -G 'Unix Makefiles'
    -DCMAKE_BUILD_TYPE:STRING='None'
    -DCMAKE_INSTALL_PREFIX:PATH='/usr'
    -DXEVD_APP_STATIC_BUILD:BOOL='OFF'
    -Wno-dev
  )
  cmake \
    -B \
      "build" \
    -S \
      "${_tarname}" \
    "${_cmake_opts[@]}"
  cmake \
    --build \
      build
}

package() {
  DESTDIR="${pkgdir}" \
  cmake \
    --install \
      build
  install \
    -Dm644 \
    "${_tarname}/COPYING" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
  rm \
    "${pkgdir}/usr/lib/${_pkg}/lib${_pkg}.a"
}
