# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Imanol Celaya <ornitorrincos@archlinux-es.org>
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Dan Vratil <progdan@progdansoft.com>
# Contributor: thotypous <matiasΘarchlinux-br·org>
# Contributor: delor <bartekpiech gmail com>

pkgbase=qtcreator
pkgname=(qtcreator qtcreator-devel)
pkgver=11.0.2
_clangver=16.0.6
pkgrel=2
pkgdesc='Lightweight, cross-platform integrated development environment'
arch=(x86_64)
url='https://www.qt.io'
license=(GPL3)
depends=(qt6-tools qt6-svg qt6-quick3d qt6-webengine qt6-serialport qt6-shadertools qt6-5compat
         clang=$_clangver clazy yaml-cpp) # litehtml syntax-highlighting
makedepends=(cmake llvm python)
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
source=(https://download.qt.io/official_releases/qtcreator/${pkgver%.*}/$pkgver/qt-creator-opensource-src-$pkgver.tar.xz
        yaml-cpp-0.8-1.patch::https://github.com/qt-creator/qt-creator/commit/170f9acfb41704b68e2ba98690fd6d5e98addd85.patch
        yaml-cpp-0.8-2.patch::https://github.com/qt-creator/qt-creator/commit/6c9d4a60e00cd00712d739746d9ded34876901a9.patch)
sha256sums=('9de9925dfce0ad1e6fcc37af7441e1052dddd15f97206493758d8303479a2d03'
            '40120945417e19e0219cb943dc9cb42398376734f3cc636242e32f47a23deaff'
            '10f57a1d6a1da5316b6810997d0654de773c5f950cdd67c96691f0dd6a949292')
options=(docs)

prepare() {
  cd qt-creator-opensource-src-$pkgver
  patch -Np1 < ../yaml-cpp-0.8-1.patch
  patch -Np1 < ../yaml-cpp-0.8-2.patch
}

build() {
  cmake -B build -S qt-creator-opensource-src-$pkgver \
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
  DESTDIR="$pkgdir" cmake --install build
# Install docs
  cp -r build/share/doc "$pkgdir"/usr/share

  install -Dm644 qt-creator-opensource-src-$pkgver/LICENSE.GPL3-EXCEPT "$pkgdir"/usr/share/licenses/qtcreator/LICENSE.GPL3-EXCEPT
}

package_qtcreator-devel() {
  pkgdesc+=' (development files)'
  depends=(qtcreator)
  optdepends=()

  DESTDIR="$pkgdir" cmake --install build --component Devel
}
