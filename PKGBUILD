# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Imanol Celaya <ornitorrincos@archlinux-es.org>
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Dan Vratil <progdan@progdansoft.com>
# Contributor: thotypous <matiasΘarchlinux-br·org>
# Contributor: delor <bartekpiech gmail com>

pkgname=qtcreator
pkgver=4.3.0
pkgrel=1
pkgdesc='Lightweight, cross-platform integrated development environment'
arch=('i686' 'x86_64')
url='http://qt-project.org'
license=('LGPL')
depends=('qt5-tools' 'qt5-quickcontrols' 'qt5-quickcontrols2' 'qt5-webengine' 'clang' 'qbs')
makedepends=('git' 'mesa' 'llvm')
options=('docs')
optdepends=('qt5-doc: integrated Qt documentation'
            'qt5-examples: welcome page examples'
            'qt5-translations: for other languages'
            'gdb: debugger'
            'cmake: cmake project support'
            'x11-ssh-askpass: ssh support'
            'git: git support'
            'mercurial: mercurial support'
            'bzr: bazaar support'
            'valgrind: analyze support')
source=("http://download.qt.io/official_releases/qtcreator/${pkgver%.*}/${pkgver}/qt-creator-opensource-src-${pkgver}.tar.xz")
sha512sums=('462d2494d54c5b528ed187a43365a296c08ad2b104ada451a91d381f070472433ef91856f80a4d94091919296c95820e5d546800fc26a43a9709a6329fba02bb')

prepare() {
  [[ -d build ]] && rm -r build
  mkdir build

  # fix hardcoded libexec path
  sed -e 's|libexec\/qtcreator|lib\/qtcreator|g' -i qt-creator-opensource-src-${pkgver}/qtcreator.pri
  # use system qbs
  rm -r qt-creator-opensource-src-${pkgver}/src/shared/qbs
}

build() {
  cd build

  qmake CONFIG+=journald ../qt-creator-opensource-src-${pkgver}/qtcreator.pro
  make
  make docs
}

package() {
  cd build

  make INSTALL_ROOT="${pkgdir}/usr/" install
  make INSTALL_ROOT="${pkgdir}/usr/" install_docs

  install -Dm644 ${srcdir}/qt-creator-opensource-src-${pkgver}/LICENSE.GPL3-EXCEPT ${pkgdir}/usr/share/licenses/qtcreator/LICENSE.GPL3-EXCEPT
}
