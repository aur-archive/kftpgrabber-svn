# Contributor: Andrea Scarpino <andrea@archlinux.org>

pkgname=kftpgrabber-svn
pkgver=1338485
pkgrel=2
pkgdesc="A graphical FTP client for KDE"
url="http://kftp.org"
arch=('i686' 'x86_64')
license=('GPL')
conflicts=('kftpgrabber')
provides=('kftpgrabber')
depends=('kdelibs' 'libssh2')
makedepends=('subversion' 'cmake' 'automoc4')
source=('FindLibSSH2.patch')
sha256sums=('0cca5a3fd92633210fa4ae297be437db7a6ebd6e58b9f01af553a761fa49cace')

_svntrunk=svn://anonsvn.kde.org/home/kde/trunk/extragear/network/kftpgrabber/
_svnmod=kftpgrabber

build() {
	if [ -d ${_svnmod}/.svn ]; then
		cd ${_svnmod} && svn up -r ${pkgver}
		cd ${srcdir}
	else
		svn co ${_svntrunk} --config-dir ./ ${_svnmod}
	fi
	msg "SVN checkout done or server timeout"
	msg "Starting make..."

	[ -d ${_svnmod}-build ] && rm -rf ${_svnmod}-build

	cp -a ${_svnmod} ${_svnmod}-build

	cd ${_svnmod}-build
	patch -Np1 -i ${srcdir}/FindLibSSH2.patch
	cmake . \
		-DQT_QMAKE_EXECUTABLE=qmake-qt4 \
		-DCMAKE_INSTALL_PREFIX=`kde4-config --prefix` \
		-DCMAKE_BUILD_TYPE=Release
	make
}

package() {
	cd ${_svnmod}-build
	make DESTDIR=$pkgdir install
}
