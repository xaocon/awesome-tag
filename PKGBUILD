# Contributor: Evan <nerdx00 nospam gmail com>
# Contributor: noonov <noonov@gmail.com>
# Contributor: wtchappell <wtchappell@gmail.com>

# Generate ldoc docs (yes/no)
_docs="no"

_pkgname=awesome
pkgname=${_pkgname}-tag
pkgver=3.5.2
pkgrel=1
pkgdesc="A highly configurable, next generation framework window manager for X"
arch=('i686' 'x86_64')
url="http://awesome.naquadah.org/"
license=('GPL2')
depends=(
  'startup-notification' 'libxdg-basedir' 'dbus' 'lua'
  'cairo>=1.12.2' 'pango' 'gdk-pixbuf2' 'lua-lgi'
  'xcb-util'{,-cursor-git,-keysyms,-wm}'>=0.3.8' 'libxcursor' 'xorg-xmessage')
makedepends=('git' 'cmake' 'asciidoc' 'xmlto' 'doxygen' 'imagemagick')
optdepends=(
  'rlwrap: readline support for awesome-client'
  'dex: autostart your desktop files'
  'vicious-git: widgets for the Awesome window manager')
provides=('awesome' 'notification-daemon')
conflicts=('awesome')
options=('docs')
install=${pkgname}.install
source=("git+http://git.naquadah.org/git/awesome.git"
		awesome.desktop)
md5sums=('SKIP'
		 '2763cab6a20d4b0f2676329d57ed3a45')

if [[ ${_docs,,} == yes ]]; then
  makedepends+=('ldoc')
fi

pkgver() {
  cd ${srcdir}/${_pkgname}
  #git log -1 --format='%cd' --date=short | tr -d -- '-'
  git describe --abbrev=0 --tags | sed 's/^v//'
}

prepare() {
	cd ${srcdir}/${_pkgname}
	git checkout v${pkgver}
}

build() {
  cd ${srcdir}/${_pkgname}

  export AWESOME_IGNORE_LGI=1
  cmake \
	-DCMAKE_INSTALL_PREFIX=/usr \
	-DSYSCONFDIR=/etc \
	-DLUA_LIBRARIES=/usr/lib/liblua.so \
	-DGENERATE_DOC=${_docs:=no}
  make
}

package() {
  cd ${srcdir}/${_pkgname}

  make DESTDIR=${pkgdir} install

  install -D -m644 ${srcdir}/awesome.desktop \
	${pkgdir}/usr/share/xsessions/awesome.desktop
  install -D -m644 ${srcdir}/awesome.desktop \
	${pkgdir}/usr/share/apps/ksmserver/windowmanagers/awesome.desktop
}
