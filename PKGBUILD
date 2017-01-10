# Maintainer: Mark Riedesel <mark@klowner.com>
# Contributor: Mark Riedesel <mark@klowner.com>
pkgname=(bosto-2g-dkms)
_pkgname=${pkgname%-*}
pkgver=r64.18d3fd0
pkgrel=1
arch=('i686' 'x86_64')
url="https://github.com/aidyw/bosto-2g-linux-kernel-module"
license=('GPL')
makedepends=('dkms' 'linux-headers')
source=("$_pkgname::git+https://github.com/klowner/bosto-2g-linux-kernel-module.git#branch=granular-makefile"
        "dkms.conf")
md5sums=('SKIP'
         '715dd64d5ee324b4dd781fc5649b2d2f')

pkgver() {
	cd ${srcdir}/$_pkgname
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

build_() {
	cd ${srcdir}/$_pkgname
	make detach_usbhid
}

package() {
	# Copy dkms.conf
	install -Dm644 ${srcdir}/dkms.conf "${pkgdir}"/usr/src/${_pkgname}-${pkgver}/dkms.conf
	sed -e "s/@_PKGNAME@/${_pkgname//-/_}/" \
		-e "s/@PKGVER@/${pkgver}/" \
		-i "${pkgdir}"/usr/src/${_pkgname}-${pkgver}/dkms.conf

	# Copy sources
	cp -r ${srcdir}/${_pkgname}/* "${pkgdir}"/usr/src/${_pkgname}-${pkgver}/
}
md5sums=('SKIP'
         '233d2a579d1450bb728b755037559e26')
