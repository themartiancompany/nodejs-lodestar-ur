# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>

_source="npm"
_pkg="lodestar"
pkgname="nodejs-${_pkg}"
pkgver=1.27.1
pkgrel=1
_pkgdesc=(
  "TypeScript Implementation of Ethereum Consensus."
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'any'
)
_http="https://github.com"
_ns="chainsafe"
url="${_http}/${_ns}/${_pkg}.js"
license=(
  "APACHE"
  "LGPL"
)
depends=(
  'nodejs'
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

