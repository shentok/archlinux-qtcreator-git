# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Imanol Celaya <ornitorrincos@archlinux-es.org>
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Dan Vratil <progdan@progdansoft.com>
# Contributor: thotypous <matiasΘarchlinux-br·org>
# Contributor: delor <bartekpiech gmail com>

pkgname=qtcreator
pkgver=4.8.1
_clangver=7.0.1
pkgrel=1
pkgdesc='Lightweight, cross-platform integrated development environment'
arch=(x86_64)
url='http://qt-project.org'
license=(LGPL)
depends=(qt5-tools qt5-quickcontrols qt5-quickcontrols2 qt5-webengine clang=$_clangver qbs clazy)
makedepends=(git mesa llvm python)
options=(docs)
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
source=("http://download.qt.io/official_releases/qtcreator/${pkgver%.*}/$pkgver/qt-creator-opensource-src-$pkgver.tar.xz"
        qtcreator-clang-plugins.patch)
sha256sums=('8f691de9b30b99dd44e010525ba9bf3054142a1082dc5273c46c533b91c07bd9'
            '6f19fc9d83964a5460d224b3d44ce580553847960181fe0364e2ce26e1efd2e6')

prepare() {
  mkdir -p build

  cd qt-creator-opensource-src-$pkgver
  # fix hardcoded libexec path
  sed -e 's|libexec\/qtcreator|lib\/qtcreator|g' -i qtcreator.pri
  # use system qbs
  rm -r src/shared/qbs
  # Load analyzer plugins on demand, since upstream clang doesn't link to all plugins
  # see http://code.qt.io/cgit/clang/clang.git/commit/?id=7f349701d3ea0c47be3a43e265699dddd3fd55cf
  # and https://bugs.archlinux.org/task/59492
  patch -p1 -i ../qtcreator-clang-plugins.patch
}

build() {
  cd build

  qmake LLVM_INSTALL_DIR=/usr QBS_INSTALL_DIR=/usr CONFIG+=journald QMAKE_CFLAGS_ISYSTEM=-I \
    DEFINES+=QBS_ENABLE_PROJECT_FILE_UPDATES "$srcdir"/qt-creator-opensource-src-$pkgver/qtcreator.pro
  make
  make docs
}

package() {
  cd build

  make INSTALL_ROOT="$pkgdir/usr/" install
  make INSTALL_ROOT="$pkgdir/usr/" install_docs

  install -Dm644 "$srcdir"/qt-creator-opensource-src-$pkgver/LICENSE.GPL3-EXCEPT "$pkgdir"/usr/share/licenses/qtcreator/LICENSE.GPL3-EXCEPT
}
