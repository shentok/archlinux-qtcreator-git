# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Imanol Celaya <ornitorrincos@archlinux-es.org>
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Dan Vratil <progdan@progdansoft.com>
# Contributor: thotypous <matiasΘarchlinux-br·org>
# Contributor: delor <bartekpiech gmail com>

pkgbase=qtcreator
pkgname=(qtcreator
         qtcreator-devel)
pkgver=12.0.0
_clangver=16.0.6
pkgrel=2
pkgdesc='Lightweight, cross-platform integrated development environment'
arch=(x86_64)
url='https://www.qt.io'
license=(GPL3)
depends=(clang=$_clangver
         clazy
         gcc-libs
         glibc
         qt6-5compat
         qt6-base
         qt6-declarative
         qt6-quick3d
         qt6-tools
         qt6-serialport
         qt6-svg
         qt6-webengine
         yaml-cpp
         zstd)
# litehtml syntax-highlighting
makedepends=(cmake
             llvm
             python)
optdepends=('qt6-doc: integrated Qt documentation'
            'qt6-examples: welcome page examples'
            'gdb: debugger'
            'cmake: cmake project support'
            'x11-ssh-askpass: ssh support'
            'git: git support'
            'mercurial: mercurial support'
            'breezy: bazaar support'
            'valgrind: analyze support'
            'perf: performer analyzer'
            'mlocate: locator filter')
options=(docs)

build() {
  export CC=/usr/lib/ccache/bin/gcc
  export CXX=/usr/lib/ccache/bin/g++
  cd $srcdir/../
  cmake -B build -S $srcdir \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_LIBEXECDIR=lib \
    -DWITH_DOCS=ON \
    -DBUILD_DEVELOPER_DOCS=ON \
    -DBUILD_QBS=OFF \
    -DQTC_CLANG_BUILDMODE_MATCH=ON \
    -DCLANGTOOLING_LINK_CLANG_DYLIB=ON
  cmake --build build
  cmake --build build --target docs
}

package_qtcreator() {
  cd $srcdir/../
  DESTDIR="$pkgdir" cmake --install build
# Install docs
  cp -r build/share/doc "$pkgdir"/usr/share

  install -Dm644 $srcdir/LICENSE.GPL3-EXCEPT "$pkgdir"/usr/share/licenses/qtcreator/LICENSE.GPL3-EXCEPT
}

package_qtcreator-devel() {
  pkgdesc+=' (development files)'
  depends=(qtcreator)
  optdepends=()

  cd $srcdir/../
  DESTDIR="$pkgdir" cmake --install build --component Devel
}
