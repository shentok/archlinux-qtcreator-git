# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Imanol Celaya <ornitorrincos@archlinux-es.org>
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Dan Vratil <progdan@progdansoft.com>
# Contributor: thotypous <matiasΘarchlinux-br·org>
# Contributor: delor <bartekpiech gmail com>

pkgbase=qtcreator
pkgname=(qtcreator qtcreator-devel)
pkgver=7.0.1
_clangver=13.0.1
pkgrel=2
pkgdesc='Lightweight, cross-platform integrated development environment'
arch=(x86_64)
url='https://www.qt.io'
license=(GPL3)
depends=(qt6-tools qt6-svg qt6-quick3d qt6-webengine qt6-serialport qt6-shadertools qt6-5compat
         clang=$_clangver clazy yaml-cpp) # syntax-highlighting
makedepends=(cmake llvm python)
optdepends=('qt6-doc: integrated Qt documentation'
            'qt6-examples: welcome page examples'
            'gdb: debugger'
            'cmake: cmake project support'
            'x11-ssh-askpass: ssh support'
            'git: git support'
            'mercurial: mercurial support'
            'bzr: bazaar support'
            'valgrind: analyze support'
            'perf: performer analyzer'
            'mlocate: locator filter')
source=(https://download.qt.io/official_releases/qtcreator/${pkgver%.*}/$pkgver/qt-creator-opensource-src-$pkgver.tar.xz)
sha256sums=('da78f293829851e32f6bf25540791697606809e47a32ca8439561fcbab96b22e')
options=(docs debug)

build() {
  cmake -B build -S qt-creator-opensource-src-$pkgver \
    -DCMAKE_INSTALL_PREFIX=/usr \
    -DCMAKE_INSTALL_LIBEXECDIR=lib \
    -DWITH_DOCS=ON \
    -DBUILD_DEVELOPER_DOCS=ON \
    -DBUILD_QBS=OFF \
    -DQTC_CLANG_BUILDMODE_MATCH=ON
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
