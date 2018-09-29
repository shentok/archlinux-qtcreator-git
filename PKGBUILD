# Maintainer: Sven-Hendrik Haase <sh@lutzhaase.com>
# Maintainer: Antonio Rojas <arojas@archlinux.org>
# Contributor: Imanol Celaya <ornitorrincos@archlinux-es.org>
# Contributor: Lukas Jirkovsky <l.jirkovsky@gmail.com>
# Contributor: Dan Vratil <progdan@progdansoft.com>
# Contributor: thotypous <matiasΘarchlinux-br·org>
# Contributor: delor <bartekpiech gmail com>

pkgname=qtcreator
pkgver=4.7.1
_clangver=7.0.0
pkgrel=3
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
        qtcreator-clang-plugins.patch qtcreator-clang7.patch
        qtcreatorbug-19367a.patch::"http://code.qt.io/cgit/qt-creator/qt-creator.git/patch/?id=807b0f78"
        qtcreatorbug-19367b.patch::"http://code.qt.io/cgit/qt-creator/qt-creator.git/patch/?id=813c1685")
sha256sums=('c98254336953f637015f14b8b4ddb5e274454a5416fd20dd09747a6e50762565'
            '6f19fc9d83964a5460d224b3d44ce580553847960181fe0364e2ce26e1efd2e6'
            '88b78c8ebd72cdad8f59bba8172cc5d1f3f9577e2bb31d841d5cabdd76eba36c'
            'a7a00a390fb46f13d53055b1862dcd916deb595dbba20c2340662cab51e5a8c1'
            '89a3fff5e398f11367ab060d910098c295968e909fcca3f35d30073cd80cbf03')

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
  # Don't use unreleased API when building against clang 7
  patch -p1 -i ../qtcreator-clang7.patch
  # https://bugreports.qt.io/browse/QTCREATORBUG-19367
  patch -p1 -i ../qtcreatorbug-19367a.patch
  patch -p1 -i ../qtcreatorbug-19367b.patch
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
