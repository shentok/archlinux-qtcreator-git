# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Imanol Celaya <ornitorrincos@archlinux-es.org>
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Dan Vratil <progdan@progdansoft.com>
# Contributor: thotypous <matiasΘarchlinux-br·org>
# Contributor: delor <bartekpiech gmail com>

pkgname=qtcreator
pkgver=3.6.0
_pkgver=v3.6.0
pkgrel=2
pkgdesc='Lightweight, cross-platform integrated development environment'
arch=('i686' 'x86_64')
url='http://qt-project.org'
license=('LGPL')
depends=('qt5-quick1' 'qt5-tools' 'qt5-quickcontrols')
makedepends=('git' 'mesa' 'clang')
options=('docs')
optdepends=('qt5-doc: for the integrated Qt documentation'
            'gdb: for the debugger'
            'cmake: for cmake project support'
            'openssh-askpass: for ssh support'
            'git: for git support'
            'mercurial: for mercurial support'
            'bzr: for bazaar support'
            'clang: Clang code model'
            'valgrind: for analyze support')
install=qtcreator.install
source=("git://code.qt.io/qt-creator/qt-creator.git#tag=${_pkgver}"
        "git://code.qt.io/qt-labs/qbs.git"
        'qtcreator.desktop')
md5sums=('SKIP'
         'SKIP'
         '800c94165c547b64012a207d9830250a')

prepare() {
  cd qt-creator
  git submodule init
  git config submodule.qbs.url $srcdir/qbs
  git submodule update

  # Debugger: Allow LLDB-MI to be used as debugger
  # https://bugreports.qt.io/browse/QTCREATORBUG-15131
  # https://code.qt.io/cgit/qt-creator/qt-creator.git/commit/?id=97e9f113879c
  # https://code.qt.io/cgit/qt-creator/qt-creator.git/commit/?id=e57b0db0f959
  git cherry-pick -n 97e9f113879c e57b0db0f959

  # Fix FS#47640
  sed -i "s/libexec/lib/g" qtcreator.pri
}

build() {
  [[ -d build ]] && rm -r build
  mkdir build && cd build

  LLVM_INSTALL_DIR=/usr qmake CONFIG+=journald -r ../qt-creator/qtcreator.pro
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
  install -Dm644 ${srcdir}/qt-creator/LGPL_EXCEPTION.TXT ${pkgdir}/usr/share/licenses/qtcreator/LGPL_EXCEPTION.TXT
}
