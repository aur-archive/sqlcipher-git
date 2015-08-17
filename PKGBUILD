# Maintainer: Terrance Kennedy <cyphus _at_ cyph _dot_ us>
pkgname=sqlcipher-git
pkgver=20121221
pkgrel=1
pkgdesc="An SQLite extension that provides 256 bit AES encryption of database files."
arch=('i686' 'x86_64')
url="http://sqlcipher.net/"
license=('custom')
depends=('openssl' 'zlib')
makedepends=('git')
provides=('sqlite=3.7.14.1')
conflicts=('sqlite' 'sqlite3')
options=('!libtool')

_gitroot="git://github.com/sqlcipher/sqlcipher.git"
_gitname="sqlcipher"

build() {
  cd "$srcdir"

  msg "Connecting to GIT server..."
  if [[ -d ${_gitname} ]]; then
    (cd ${_gitname} && git pull origin)
  else
    git clone ${_gitroot} ${_gitname}
  fi
  msg "GIT checkout done or server timeout"
  msg "Starting make..."

  rm -rf ${_gitname}-build
  git clone ${_gitname} ${_gitname}-build

  cd ${srcdir}/${_gitname}-build
  CFLAGS="$CFLAGS -DSQLITE_HAS_CODEC"
  LDFLAGS="$LDFLAGS -lcrypto"
  ./configure --prefix=/usr \
      --disable-tcl \
      --enable-shared \
      --enable-static \
      --enable-tempstore=yes \
      LIBS="-lz -ldl"
  make
}

package() {
  cd "$srcdir/$_gitname-build"

  make DESTDIR="$pkgdir/" install
  install -Dm644 ${srcdir}/${_gitname}/LICENSE ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
}
