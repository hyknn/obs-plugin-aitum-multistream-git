# Maintainer: hyknn <hyknn@protonmail.com>
pkgname=obs-plugin-aitum-multistream-git
_pkgname=obs-aitum-multistream
pkgver=1.0.7.r0.g9f36de8
pkgrel=1
pkgdesc='Plugin for OBS Studio to multistream by Aitum'
arch=('x86_64')
url='https://github.com/Aitum/obs-aitum-multistream'
license=('GPL2')
groups=('obs-plugins')
depends=(
	'obs-studio>=30.0.0'
    'qt6-base'
    'curl'
)
makedepends=(
	'cmake'
	'make'
	'gcc'
	'git'
)
provides=('obs-plugin-aitum-multistream')
conflicts=()
options=(!debug strip)
source=('git+https://github.com/Aitum/obs-aitum-multistream#branch=main')
sha256sums=('SKIP')

pkgver() {
	cd "${srcdir}/${_pkgname}"
	git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd "${srcdir}/${_pkgname}"
    sed -i '/if(BUILD_OUT_OF_TREE)/, /endif()/ {
      /^[ \t]*find_qt(COMPONENTS Widgets Core)[ \t]*$/d
      /^[ \t]*else()[ \t]*$/d
    }' CMakeLists.txt
}

build() {
	cd "${srcdir}/${_pkgname}"
	cmake -S . -B build -DBUILD_OUT_OF_TREE=On
	cmake --build build
}

package() {
    _prjdir="${srcdir}/${_pkgname}"
    install -D -m755 "${_prjdir}/build/vertical-canvas.so" "${pkgdir}/usr/lib/obs-plugins/aitum-vertical-canvas.so"
    install -D -m644 "${_prjdir}/LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
    mkdir -p "${pkgdir}/usr/share/obs/obs-plugins/aitum-vertical-canvas"
    cp -r "${_prjdir}/data/locale" "${pkgdir}/usr/share/obs/obs-plugins/aitum-vertical-canvas/locale"
}
