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
_arch="$( \
  uname \
    -m)"
_node="nodejs"
if [[ "${_os}" == "GNU/Linux" && \
      "${_arch}" == "x86_64" ]]; then
  _source="npm"
elif [[ "${_os}" == "Android" && \
      ( "${_arch}" == "armv7l" || \
        "${_arch}" == "arm" || \
	"${_arch}" == "aarch64" ) ]]; then
  _source="github"
fi
# As of 25/3/4
# chainsafe says it's not safe to
# install the npm package because of
# the possibility of dependencies
# tamperings
# https://chainsafe.github.io/lodestar/run/getting-started/installation/#build-from-source
_source="github"
_pkg=lodestar
pkgname="${_node}-${_pkg}"
pkgver=1.27.1.1
_nodejs_c_kzg_ver="2.1.3"
_commit="c8e2203c59526a50768757535068d97db1614c66"
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
  'powerpc'
)
_http="https://github.com"
# _ns="chainsafe"
_ns="themartiancompany"
url="${_http}/${_ns}/${_pkg}"
license=(
  "APACHE"
  "LGPL"
)
# Termux people don't add
# provides to their pacman packages so
# installing nodejs-lts on Life and DogeOS
# Android base will create issues to all
# nodejs packages.
# https://github.com/termux-pacman/termux-packages/issues/38#issuecomment-1882637215
# So nothing, since they seem to keep wanting
# their environment to be a google managed toy,
# continuous integration to entirely replace their
# environment needs to be ready kinda asap.
depends=(
  "${_node}-lts"
)
makedepends=(
  'npm'
)
if [[ "${_source}" == "github" ]]; then
  depends+=(
    "${_node}-c-kzg>=${_nodejs_c_kzg_ver}"
  )
  makedepends+=(
    "yarn"
  )
fi
provides=(
  "${_pkg}=${pkgver}"
)
conflicts=(
  "${_pkg}"
)
source=()
sha256sums=()
_npm="https://registry.npmjs.org"
if [[ "${_source}" == "npm" ]]; then
  _tag_name="pkgver"
  _tag="${pkgver}"
  noextract+=(
    "${_tarball}"
  )
elif [[ "${_source}" == "github" ]]; then
  _tag_name="commit"
  _tag="${_commit}"
fi
_tarname="${_pkg}-${_tag}"
if [[ "${_source}" == "npm" ]]; then
  _tarball="${_tarname}.tgz"
  _src="${_tarball}.tgz::${_npm}/@${_ns}/${_pkg}/-/${_tarname}.tgz"
  _sum="22d6d7007fc40fa22d565d73e008a953fa0db2ff1c5a8b2e1a2c0ea203fb6174"
  noextract=(
    "${_tarball}.tgz"
  )
elif [[ "${_source}" == "github" ]]; then
  _tarball="${_tarname}.zip"
  _src="${_tarball}::${url}/archive/${_commit}.zip"
  _sum="85da5d2ba00323eeeddbcdb9ef7a05b1ea6ab59dab5f0211b5c8f494291d3603"
fi
source=(
  "${_src}"
)
sha256sums=(
  "${_sum}"
)

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
      grep \
        "android_ndk_path")"
    if [[ "${_ndk_check}" == "" ]]; then
      echo \
        "You should probably add" \
        "\"{'variables':{'android_ndk_path':''}}\"" \
        "to '${_gyp_include}'."
    fi
  fi
}

_c_kzg_prepare() {
  local \
    _node_path \
    _npm_install_opts=() \
    _local_package_install_mode \
    _pattern \
    _replacement
  _pattern="\"c-kzg\": \"^2.1.3\""
  _replacement="\"c-kzg\": \"file:../../node_modules/c-kzg\""
  _local_package_install_mode="cp"
  _npm_install_opts+=(
    # I'm nobody to say anything,
    # but this npm program seems kinda
    # untrustable in its naming and
    # defaulting options choice
    # https://github.com/npm/cli/issues/5965
    # https://github.com/npm/cli/issues/5844
    --install-links="false"
  )
  _node_path="$( \
    npm \
      -g \
      list | \
      head \
        -n \
          1)/node_modules"
  echo \
    "Installing c-kzg from local" \
    "repository."
  cd \
    "${srcdir}/${_tarname}"
  if [[ "${_local_package_install_mode}" == "cp" ]]; then
    mkdir \
      -p \
      "node_modules" || \
      true
    cp \
      -r \
      "${_node_path}/c-kzg" \
      "node_modules" || \
      true
  elif [[ "${_local_package_install_mode}" == "npm" ]]; then
    npm \
      install \
        "${_npm_install_opts[@]}" \
        "${_node_path}/c-kzg"
  fi
  echo \
    "Patching 'packages/beacon-node'" \
    "'package.json' to use local c-kzg build. See" \
    "( https://github.com/ChainSafe/lodestar/issues/7517 )."
  sed \
    -e \
      "s%${_pattern}%${_replacement}%" \
    -i \
    "packages/beacon-node/package.json"
}

prepare() {
  _android_gyp_quirk
  if [[ "${_source}" = "github" ]]; then
    _c_kzg_prepare
  fi
}

_usr_get() {
  local \
    _bin
  _bin="$( \
    dirname \
      "$(command \
           -v \
           "env")")"
  echo \
    "$(dirname \
         "${_bin}")"
}

_lodestar_build() {
  cd \
    "${srcdir}/${_tarname}"
  echo \
    "Installing Lodestar" \
    "dependencies."
  yarn \
    install
  echo \
    "Building Lodestar."
  yarn \
    run \
      build
}

build() {
  cd \
    "${_tarname}"
  if [[ "${_source}" == "github" ]]; then
    _lodestar_build
    echo \
      "Creating bundle."
    yarn \
      run \
        "build:bundle"
  fi
}

package() {
  local \
    _npmdir \
    _npm_opts=() \
    _src
  _npm_opts=(
    # --user
    #   root
    -g
    --prefix
      "${pkgdir}/usr"
  )
  _npmdir="${pkgdir}/usr/lib/node_modules/"
  mkdir \
    -p \
    "${_npmdir}"
  cd \
    "${_npmdir}"
  if [[ "${_source}" == "github" ]]; then
    # I am not sure this is gonna appear as a consequence
    # of running yarn run 'build:bundle' actually
    _src="${srcdir}/${_tarname}/${_pkg}-${pkgver}.tgz"
  elif [[ "${_source}" == "npm" ]]; then
    _src="${srcdir}/${_pkg}-${pkgver}.tgz"
  fi
  npm \
    install \
      "${_npm_opts[@]}" \
      "${_src}"
}
