# Maintainer: Jonathan Schilling <jonathan.schilling@mail.de>
pkgname=pspline
pkgver=1.0.0
pkgrel=1
pkgdesc="Princeton University NTCC spline library"
arch=("x86_64")
url="https://w3.pppl.gov/ntcc/PSPLINE/"
license=('custom')
depends=(netcdf-fortran)
source=("https://w3.pppl.gov/rib/repositories/NTCC/files/pspline.tar.gz"
        "$pkgname-$pkgver.patch")
md5sums=("ede16b8de216aa81ecfc2cbcfc577cb8"
         "428c4a65878e9087d6201fae6bb3ef5d")

prepare() {
	cd "$srcdir"
	patch -uNp1 -i "$srcdir/$pkgname-$pkgver.patch" || return 1
}

build() {
	pushd "$srcdir/cdf_dummy"
	make FORTRAN_VARIANT=GCC PREFIX=/usr NETCDF_DIR=/usr LIBROOT=/usr NETCDF="-lnetcdf -lnetcdff" MODDIR=/usr/include
	popd
	
	pushd "$srcdir/ezcdf"
	make FORTRAN_VARIANT=GCC PREFIX=/usr NETCDF_DIR=/usr LIBROOT=/usr NETCDF="-lnetcdf -lnetcdff" MODDIR=/usr/include
	popd

	pushd "$srcdir/portlib"
	make FORTRAN_VARIANT=GCC PREFIX=/usr NETCDF_DIR=/usr LIBROOT=/usr NETCDF="-lnetcdf -lnetcdff" MODDIR=/usr/include all
	popd

	pushd "$srcdir/pspline"
	make FORTRAN_VARIANT=GCC PREFIX=/usr NETCDF_DIR=/usr LIBROOT=/usr NETCDF="-lnetcdf -lnetcdff" MODDIR=/usr/include
	make FORTRAN_VARIANT=GCC PREFIX=/usr NETCDF_DIR=/usr LIBROOT=/usr NETCDF="-lnetcdf -lnetcdff" MODDIR=/usr/include so
	popd
}

package() {
	mkdir -p "$pkgdir/usr/"

	cd "$srcdir"
	make PREFIX="$pkgdir/usr/" install

	# move man pages to usr/share/man
	mkdir "$pkgdir/usr/share/"
	mv "$pkgdir/usr/man" "$pkgdir/usr/share/"

	# MODDIR is not implemented consistently, so move *.mod manually
	mv "$pkgdir/usr/mod/"*.mod "$pkgdir/usr/include/"
	rmdir "$pkgdir/usr/mod"

	# 'install' target of Makefile does not care about shared library,
	# so install it manually
	install "$srcdir/LINUX/lib/libpspline.so" "$pkgdir/usr/lib/"
}

