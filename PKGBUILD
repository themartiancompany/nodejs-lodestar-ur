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

# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Truocolo <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer: Pellegrino Prevete (dvorak) <pellegrinoprevete@gmail.com>
# Maintainer: Pellegrino Prevete (dvorak) <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>

_os="$( \
  uname \
    -o)"
_node="nodejs"
_source="npm"
_pkg="lodestar"
pkgname="${_node}-${_pkg}"
pkgver=1.27.1
pkgrel=1
_pkgdesc=(
  "TypeScript Implementation of Ethereum Consensus."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'armv7l'
  'mips'
  'pentium4'
  'i686'
)
_http="https://github.com"
_ns="chainsafe"
url="${_http}/${_ns}/${_pkg}.js"
license=(
  "APACHE"
  "LGPL"
)
depends=(
  "${_node}"
)
makedepends=(
  'npm'
)
provides=(
  "${_pkg}=${pkgver}"
)
conflicts=(
  "${_pkg}"
)
_npm="https://registry.npmjs.org"
if [[ "${_source}" == "npm" ]]; then
  _src="${_npm}/@${_ns}/${_pkg}/-/${_pkg}-${pkgver}.tgz"
  _sum="22d6d7007fc40fa22d565d73e008a953fa0db2ff1c5a8b2e1a2c0ea203fb6174"
elif [[ "${_source}" == "github" ]]; then
  _src="${url}/archive/refs/tags/v${pkgver}.tar.gz"
  _sum="ciao"
fi
source=(
  "${_src}"
)
noextract=(
  "${_pkg}-${pkgver}.tgz"
)
sha256sums=(
  "${_sum}"
)

_android_quirk() {
  local \
    _tools_bin \
    _clang \
    _compiler_dir \
    _compiler
  cd \
    "${srcdir}/${_tarname}"
  if [[ "${_os}" == "Android" ]] && \
     [[ "${_arch}" == "armv7l" ]]; then
    _clang="$( \
      command \
        -v \
        clang)"
    _tools_bin="undefined/toolchains/llvm/prebuilt/linux-x86_64/bin"
    _compiler_dir="${srcdir}/${_tarname}/${_tools_bin}"
    _compiler="${_compiler_dir}/armv7a-linux-androideabi24-clang"
    mkdir \
      -p \
      "${_compiler_dir}"
    ln \
      -s \
      "${_clang}" \
      "${_compiler}" || \
      true
  fi
  cd \
    "${srcdir}/${_tarname}"
}

_android_gyp_quirk() {
  local \
    _gyp_include \
    _ndk_check
  _gyp_include="${HOME}/.gyp/include.gypi"
  if [[ ! -e "${_gyp_include}" ]]; then
    mkdir \
      -p \
      "$(dirname \
           "${_gyp_include}")"
    echo \
      "{'variables':{'android_ndk_path':''}}" > \
      "${_gyp_include}"
  elif [[ -e "${_gyp_include}" ]]; then
    _ndk_check="$( \
      cat \
        "${_gyp_include}" | \
      grep "android_ndk_path")"
    if [[ "${_ndk_check}" == "" ]]; then
      echo \
        "You should probably add" \
        "\"{'variables':{'android_ndk_path':''}}\"" \
        "to '${_gyp_include}'."
    fi
  fi
}

prepare() {
  _android_gyp_quirk
  # _android_quirk  
}

package() {
  local \
    _npmdir \
    _npm_opts=() 
  _npm_opts=(
    # --user
    #   root
    -g
    --prefix
      "${pkgdir}/usr"
  )
  cd "${srcdir}"
  _npmdir="${pkgdir}/usr/lib/node_modules/"
  mkdir \
    -p \
    "${_npmdir}"
  cd \
    "${_npmdir}"
  npm \
    install \
      "${_npm_opts[@]}" \
      "${srcdir}/${_pkg}-${pkgver}.tgz"
      # "${srcdir}/${_pkg}.js-${pkgver}"
}

