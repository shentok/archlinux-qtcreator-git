# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Imanol Celaya <ornitorrincos@archlinux-es.org>
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Dan Vratil <progdan@progdansoft.com>
# Contributor: thotypous <matiasΘarchlinux-br·org>
# Contributor: delor <bartekpiech gmail com>

pkgname=qtcreator
pkgver=4.2.0
_pkgver=v4.2.0
pkgrel=3
pkgdesc='Lightweight, cross-platform integrated development environment'
arch=('i686' 'x86_64')
url='http://qt-project.org'
license=('LGPL')
depends=('qt5-tools' 'qt5-declarative' 'qt5-script' 'qt5-quickcontrols' 'qt5-quickcontrols2' 'qt5-webengine' 'clang')
makedepends=('git' 'mesa' 'llvm')
options=('docs')
optdepends=('qt5-doc: integrated Qt documentation'
            'qt5-examples: welcome page examples'
            'qt5-translations: for other languages'
            'gdb: debugger'
            'cmake: cmake project support'
            'openssh-askpass: ssh support'
            'git: git support'
            'mercurial: mercurial support'
            'bzr: bazaar support'
            'valgrind: analyze support')
source=("http://download.qt.io/official_releases/qtcreator/4.2/${pkgver}/qt-creator-opensource-src-${pkgver}.tar.xz"
        'qtcreator.desktop')
sha512sums=('1801611d8db10041332d728a2af6b0c57b28b3ab9847a8875edb788d3b0fe6570103a408a7f52c989e40949743263d444e5db49d290860f2e7231f463fa86f7a'
            '10552f2f77e816bccf21f31ba00bca10607df9c355f5efcbcca337e52a8a7e3cb66da6336b717de5f810dc74d3f175ba81eef04f55e08892bb39ddc66e27dad4')

build() {
  [[ -d build ]] && rm -r build
  mkdir build && cd build

  LLVM_INSTALL_DIR=/usr qmake CONFIG+=journald -r ../qt-creator-opensource-src-${pkgver}/qtcreator.pro
  make
  make docs -j1
}

package() {
  cd build

  make INSTALL_ROOT="${pkgdir}/usr/" install
  make INSTALL_ROOT="${pkgdir}/usr/" install_docs

  # Workaround for FS#40583
  mv "${pkgdir}"/usr/bin/qtcreator "${pkgdir}"/usr/bin/qtcreator-bin
  echo "#!/bin/sh" > "${pkgdir}"/usr/bin/qtcreator
  echo "QT_LOGGING_TO_CONSOLE=1 qtcreator-bin \$@" >> "${pkgdir}"/usr/bin/qtcreator
  chmod +x "${pkgdir}"/usr/bin/qtcreator

  install -Dm644 ${srcdir}/qtcreator.desktop ${pkgdir}/usr/share/applications/qtcreator.desktop
  install -Dm644 ${srcdir}/qt-creator-opensource-src-${pkgver}/LICENSE.GPL3-EXCEPT ${pkgdir}/usr/share/licenses/qtcreator/LICENSE.GPL3-EXCEPT
}
