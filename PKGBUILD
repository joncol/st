# Maintainer: Jose Riha <jose1711 gmail com>
# Maintainer: Sebastian J. Bronner <waschtl@sbronner.com>
# Contributor: Patrick Jackson <PatrickSJackson gmail com>
# Contributor: Christoph Vigano <mail@cvigano.de>

pkgname=st
pkgver=0.8.4
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64' 'armv7h')
license=('MIT')
depends=(libxft)
url=https://st.suckless.org
source=(https://dl.suckless.org/$pkgname/$pkgname-$pkgver.tar.gz
        terminfo.patch
        https://st.suckless.org/patches/alpha/st-alpha-0.8.2.diff
        https://st.suckless.org/patches/clipboard/st-clipboard-0.8.3.diff
        https://st.suckless.org/patches/gruvbox/st-gruvbox-dark-0.8.2.diff
        https://st.suckless.org/patches/vertcenter/st-vertcenter-20180320-6ac8c8a.diff
        README.terminfo.rst)
md5sums=('e00b074c0e5d55513745c99f027b7a34'
         'da701034fc5463ded7d9944fe42014ab'
         '9258f1585627afab76a0a36a96970420'
         '3022fff42f642c0189b4c135e94292db'
         '08a5f4549f1a6b8b3885f0a2b1f80783'
         '51106ec8ff04d64029401421bbc57ab5'
         '25df7a6ec8e78f8910769730d9c00022')

_sourcedir=$pkgname-$pkgver
_makeopts="--directory=$_sourcedir"

prepare() {
  patch --directory="$_sourcedir" -Np0 < terminfo.patch
  patch --directory="$_sourcedir" -Np1 < st-alpha-0.8.2.diff
  patch --directory="$_sourcedir" -Np1 < st-clipboard-0.8.3.diff
  patch --directory="$_sourcedir" -Np1 < st-vertcenter-20180320-6ac8c8a.diff

  # This package provides a mechanism to provide a custom config.h. Multiple
  # configuration states are determined by the presence of two files in
  # $BUILDDIR:
  #
  # config.h  config.def.h  state
  # ========  ============  =====
  # absent    absent        Initial state. The user receives a message on how
  #                         to configure this package.
  # absent    present       The user was previously made aware of the
  #                         configuration options and has not made any
  #                         configuration changes. The package is built using
  #                         default values.
  # present                 The user has supplied his or her configuration. The
  #                         file will be copied to $srcdir and used during
  #                         build.
  #
  # After this test, config.def.h is copied from $srcdir to $BUILDDIR to
  # provide an up to date template for the user.
  if [ -e "$BUILDDIR/config.h" ]
  then
    cp "$BUILDDIR/config.h" "$_sourcedir"
  elif [ ! -e "$BUILDDIR/config.def.h" ]
  then
    msg='This package can be configured in config.h. Copy the config.def.h '
    msg+='that was just placed into the package directory to config.h and '
    msg+='modify it to change the configuration. Or just leave it alone to '
    msg+='continue to use default values.'
    warning "$msg"
  fi
  cp "$_sourcedir/config.def.h" "$BUILDDIR"
}

build() {
  make $_makeopts X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  local installopts='--mode 0644 -D --target-directory'
  local shrdir="$pkgdir/usr/share"
  local licdir="$shrdir/licenses/$pkgname"
  local docdir="$shrdir/doc/$pkgname"
  make $_makeopts PREFIX=/usr DESTDIR="$pkgdir" install
  install $installopts "$licdir" "$_sourcedir/LICENSE"
  install $installopts "$docdir" "$_sourcedir/README"
  install $installopts "$docdir" README.terminfo.rst
  install $installopts "$shrdir/$pkgname" "$_sourcedir/st.info"
}
