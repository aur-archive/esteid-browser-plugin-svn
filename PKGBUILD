# Maintainer: kevku <kevku@gmx.com>
pkgname=esteid-browser-plugin-svn
_browser=firefox
pkgver=4011
pkgrel=6
pkgdesc="Estonian ID-card Firefox plugin & extension (Community dev version)"
arch=('x86_64' 'i686')
url="http://code.google.com/p/esteid"
license=('LGPL')
depends=('boost-libs' 'smartcardpp-svn' 'firefox' 'gtkmm' 'libdigidocpp-svn' 'opensc012')
makedepends=('cmake' 'subversion' 'zip' 'unzip' 'boost')
conflicts=('sk-esteidpkcs11loader-svn' 'sk-esteidfirefoxplugin-svn')
source=("http://firebreath.googlecode.com/files/firebreath-1.5.2.tar.bz2")
md5sums=('14e5854f90655f87eddf6d0d5f735f46')

_svntrunk=https://esteid.googlecode.com/svn/${pkgname%-svn}/trunk
_svnmod=${pkgname%-svn}

build() {
  cd "$srcdir/"
  
  if [ -d $srcdir/firebreath-1.5.2/projects/$_svnmod/.svn ]; then
    (cd $srcdir/firebreath-1.5.2/projects/$_svnmod && svn up -r $pkgver)
  else
    mkdir $srcdir/firebreath-1.5.2/projects
    cd "$srcdir/firebreath-1.5.2/projects"
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/plugin-build"
  cp -r "$srcdir/firebreath-1.5.2" "$srcdir/plugin-build"
  cd "$srcdir/plugin-build/"
  sed -i 's|(bits == 64) ? "/usr/lib64/" :||g' projects/esteid-browser-plugin/Mozilla/chrome/content/config.js
  #sed -i 's|onepin-opensc-pkcs11|opensc-pkcs11|g' projects/esteid-browser-plugin/Mozilla/chrome/content/config.js
  cmake . -DCMAKE_INSTALL_PREFIX=/usr -DLIB_SUFFIX="" -DWITH_SYSTEM_BOOST=YES -DSYSCONF_INSTALL_DIR=/etc
  make
}

package() {
  cd "$srcdir/plugin-build/"
  make DESTDIR="$pkgdir/" install
  local dstdir=$pkgdir/usr/lib/$_browser/extensions/\{aa84ce40-4253-11da-8cd6-0800200c9a66\}/
  install -d $dstdir
  unzip projects/esteid/*.xpi -d $dstdir
}
