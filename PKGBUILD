# Maintainer: Mark Riedesel <mark@klowner.com>
# Contributor: Mark Riedesel <mark@klowner.com>
pkgname=(bosto-2g-dkms)
_pkgname=${pkgname%-*}
pkgver=r64.18d3fd0
pkgrel=1
arch=('i686' 'x86_64')
url="https://github.com/aidyw/bosto-2g-linux-kernel-module"
license=('GPL3')
makedepends=('dkms' 'linux-headers')
source=("$_pkgname::git+https://github.com/klowner/bosto-2g-linux-kernel-module.git#branch=granular-makefile"
        "dkms.conf")
md5sums=('SKIP'
         '233d2a579d1450bb728b755037559e26')

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
	install -Dm644 ${srcdir}/dkms.conf "${pkgdir}/usr/src/${_pkgname}-${pkgver}"/dkms.conf
	sed -e "s/@_PKGNAME@/${_pkgname//-/_}/" \
		-e "s/@PKGVER@/${pkgver}/" \
		-i "${pkgdir}/usr/src/${_pkgname}-${pkgver}"/dkms.conf

	# Copy utilities
	for x in inputtransform load_bosto_2g.sh; do
		install -Dm755 ${srcdir}/${_pkgname}/$x "${pkgdir}/usr/bin/${x}"
	done

	# Copy udev rules
	install -Dm644 "${srcdir}/${_pkgname}/${_pkgname//-/_}.rules" "${pkgdir}"/usr/lib/udev/rules.d/65-${_pkgname}.rules
	sed -e "s/\/usr\/local\/bin/\/usr\/bin/" -i "${pkgdir}"/usr/lib/udev/rules.d/65-${_pkgname}.rules

	# Copy (relevant) sources
	cp -r ${srcdir}/${_pkgname}/{Makefile,bosto_2g.c} "${pkgdir}"/usr/src/${_pkgname}-${pkgver}/
}
