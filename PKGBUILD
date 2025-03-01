# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2025  Pellegrino Prevete
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
# Maintainer: Llewelyn Trahaearn <WoefulDerelict at GMail dot com>
# Contributor: josephgbr <rafael.f.f1 at gmail dot com>
# Contributor: TryA <tryagainprod at gmail dot com>
# Contributor: Jan de Groot <jgc at archlinux dot org>

_os="$( \
  uname \
    -o)"
_ml="lib32-"
if [[ "${_os}" == "GNU/Linux" ]]; then
  _compiler_libs='gcc-multilib'
  _libc="${_ml}glibc"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _compiler_libs="libcompiler-rt"
  _libc="ndk-sysroot"
fi

_proj="gnome"
_docs="false"
_pkg=libnotify
pkgname=(
  "${_ml}${_pkg}"
)
if [[ "${_docs}" == "true" ]]; then
  pkgname+=(
    "${_ml}${_pkg}-docs"
  )
fi
pkgver=0.8.4
_commit="570982f616838abba6bdd8ca2bdb2f32f3b1d1de"
pkgrel=1
pkgdesc="Library for sending desktop notifications (32-bit)"
arch=(
  'x86_64'
)
_http="https://gitlab.${_proj}.org"
_ns="GNOME"
url="${_http}/${_ns}/${_pkg}"
license=(
  'LGPL'
)
depends=(
  "${_compiler_libs}"
  "${_ml}gdk-pixbuf2"
  "${_ml}glib2"
  "${_libc}"
  "${_pkg}"
)
makedepends=(
  'docbook-xsl'
  # glib2 will become
  # glib2-devel if arch upstream
  # at some point wont
  # deprecate it entirely
  # before that happens
  "${_ml}glib2"
  "${_ml}gobject-introspection"
  "${_ml}gtk3"
  'meson'
  'xmlto'
)
if [[ "${_docs}" == "true" ]]; then
  "gi-docgen"
fi
if [[ "${_git}" == "true" ]]; then
  makedepends+=(
    "git"
  )
fi
_tag_name="commit"
_tag="${_commit}"
source=(
  "${_pkg}::git+${url}.git#${_tag_name}=${_tag}"
)
sha512sums=(
  'SKIP'
)

pkgver() {
  cd \
    "${_pkg}"
  git \
    describe \
      --tags | \
    sed \
      's/[^-]*-g/r&/;s/-/+/g'
}

build() {
  local \
    _meson_opts=()
  _meson_opts+=(
    --prefix="/usr"
    --sbindir="bin"
    --buildtype="plain"
    --libexecdir="lib32"
    --libdir="/usr/lib32"
  )
  # Modify environment to generate
  # 32-bit ELF. Respects flags defined in makepkg.conf
  export \
    CFLAGS="-m32 ${CFLAGS}" \
    CXXFLAGS="-m32 ${CXXFLAGS}" \
    LDFLAGS="-m32 ${LDFLAGS}" \
    PKG_CONFIG_PATH='/usr/lib32/pkgconfig'
#  export PKG_CONFIG_LIBDIR='/usr/lib32/pkgconfig'
  arch-meson \
    "${_pkg}" \
    "build" \
    "${_meson_opts[@]}"
  meson \
    compile \
      -C \
        "build"
}

check() {
  meson \
    test \
      -C \
        "build" \
      --print-errorlogs
}

package_lib32-libnotify() {
  meson \
    install \
    -C \
      "build" \
    --destdir \
      "${pkgdir}"
  mkdir \
    -p \
    "doc/usr/share"
  mv \
    {"${pkgdir}","doc"}"/usr/share/doc"
  rm \
    -rf \
    "${pkgdir}/usr/"{"bin","include","share"}
}

package_lib32-libnotify-docs() {
  pkgdesc+=" (documentation)"
  depends=()
  mv \
    "doc/"* \
    "${pkgdir}"
}
