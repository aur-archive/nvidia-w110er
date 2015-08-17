#
# Maintainer: blackleg <blackleg@openmailbox.org>
# Based in nvidia-ck package
# Maintainer: graysky <graysky AT archlnux.us>
# Contributor: A.J. Korf <jacobkorf at gmail dot com>
# Contrubutor: Thomas Baechler <thomas@archlinux.org>

pkgname=nvidia-w110er
pkgver=343.22
_extramodules=extramodules-3.16-w110er
pkgrel=2
_pkgdesc="NVIDIA drivers for linux-w110er."
pkgdesc="$_pkgdesc"
arch=('x86_64')
url="http://www.nvidia.com/"
depends=('linux-w110er>=3.17' 'linux-w110er<3.18' "nvidia-libgl" "nvidia-utils=${pkgver}")
makedepends=('linux-w110er-headers>=3.17' 'linux-w110er-headers<3.18')
#conflicts=()
#groups=('ck-generic')
#replaces=()
license=('custom')
install=nvidia-w110er.install
options=(!strip)
source=("ftp://download.nvidia.com/XFree86/Linux-x86/${pkgver}/NVIDIA-Linux-x86-${pkgver}.run"
        "ftp://download.nvidia.com/XFree86/Linux-x86_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run")
md5sums=('696d0bfa7195e22e0c4bd858fff2abb6'
         '3d98eaa01a94e86abb04723ae2a702b1')
[[ "$CARCH" = "i686" ]] && _pkg="NVIDIA-Linux-x86-${pkgver}"
[[ "$CARCH" = "x86_64" ]] && _pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only
    cd "${_pkg}"
    # patches here
}

build() {
	_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
	cd "${_pkg}/kernel"
	make SYSSRC=/usr/lib/modules/"${_kernver}/build" module

	cd uvm
	make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
	install -Dm644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
		"${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
	install -D -m644 "${srcdir}/${_pkg}/kernel/uvm/nvidia-uvm.ko" \
		"${pkgdir}/usr/lib/modules/${_extramodules}/nvidia-uvm.ko"
	gzip -9 "${pkgdir}/usr/lib/modules/${_extramodules}/"*.ko
	install -dm755 "${pkgdir}/usr/lib/modprobe.d"
	echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nvidia-w110er.conf"
	sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia-w110er.install"
}
