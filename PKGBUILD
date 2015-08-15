# Contributor: ying
# Contributor: ying

pkgname=crossover-standard
pkgver=10.2.0
pkgrel=1
pkgdesc="Run Windows Programs on Linux"
provides=("crossover")
conflicts=("crossover" "crossover-pro")
arch=('i686' 'x86_64')
url="http://www.codeweavers.com"

makedepends=('binutils' 'tar')

if [ $CARCH = 'i686' ] ;
then

depends=('alsa-lib' 'libsm' 'libxext' 'libxrandr' 'libice' 'pygtk' 'desktop-file-utils' 'fontconfig' 'libxcursor' 'libxdamage' 'libxxf86dga' 'mesa')
optdepends=('libxcursor: coloured mouse pointer support' 
  'libxi: enables joystick and tablet support' 
  'libxinerama: enables spanning multiple screens' 
  'openssl:  support for secure Internet communication' 
  'libcups: printing support'
  'libpng: enable PNG images' 
  'libjpeg: enable JPEG images' 
  'libxxf86vm: perform gamma adjustments' 
  'hal: automatically detect CD-ROMs, DVDs, and USB keys' 
  'unzip: required to install Guild Wars, automatic installer extraction'
  ) 

else 

depends=('fontconfig' 'desktop-file-utils' 'alsa-lib' 'lib32-alsa-lib' 'lib32-fontconfig' 'lib32-libxcursor' 'libxxf86dga' 'libxrandr' 'libxdamage' 'lib32-libxdamage' 'lib32-libxxf86dga' 'mesa' 'lib32-mesa' 'lib32-glibc' 'libxcursor' 'lib32-libsm' 'lib32-libxext' 'lib32-zlib' 'lib32-gcc-libs' 'lib32-libxrandr' 'lib32-libice' 'lib32-util-linux-ng' 'lib32-e2fsprogs' 'pygtk')
optdepends=('lib32-nvidia-utils: enables 3D under nvidia cards' 
  'lib32-catalyst-utils: enables 3D under ati cards' 
  'lib32-libxcursor: coloured mouse pointer support' 
  'lib32-libxinerama: enables spanning multiple screens' 
  'lib32-openssl:  support for secure Internet communication' 
  'lib32-libcups: printing support' 
  'lib32-libxxf86vm: perform gamma adjustments' 
  'lib32-libxi: enables joystick and tablet support' 
  'lib32-libpng: enable PNG images' 
  'lib32-libjpeg: enable JPEG images' 
  'lib32-hal: automatically detect CD-ROMs, DVDs, and USB keys' 
  'unzip: required to install Guild Wars, automatic installer extraction'
) 
fi

license=('custom')
install=${pkgname}.install
source=("http://media.codeweavers.com/pub/crossover/cxlinux/demo/${pkgname}-demo_${pkgver}-1_i386.deb" cxoffice.conf)

build() {
	cd "${srcdir}"
	ar -p crossover-standard-demo_${pkgver}-1_i386.deb data.tar.gz | tar zxf - -C "${pkgdir}" || return 1

	if [ $CARCH = 'i686' ] ;
	then
		rm -fr $pkgdir/opt/cxoffice/lib/nsplugin-linux64.so
	fi

	rm "${pkgdir}"/opt/cxoffice/doc # remove symbolic link
	mkdir "${pkgdir}"/opt/cxoffice/doc # create real directory
	mv "${pkgdir}"/usr/share/doc/crossover-standard-demo/* "${pkgdir}"/opt/cxoffice/doc
	gzip -d "${pkgdir}"/opt/cxoffice/doc/license.txt.gz
	install -m 644 -D "${pkgdir}"/opt/cxoffice/doc/license.txt "${pkgdir}"/usr/share/licenses/crossover-standard/license
	sed s/\;\;"\"MenuRoot\" = \"\""/"MenuRoot = Windows Games/" -i "${pkgdir}"/opt/cxoffice/share/crossover/bottle_data/cxbottle.conf
	sed s/\;\;"\"MenuStrip\" = \"\""/"MenuStrip = 1/" -i "${pkgdir}"/opt/cxoffice/share/crossover/bottle_data/cxbottle.conf
	rm -rf "${pkgdir}"/usr

	# Sed for Python2
	cd $pkgdir/opt/cxoffice/bin
	sed -i -e "s|#![ ]*/usr/bin/python$|#!/usr/bin/python2|" *
	sed -i -e "s|#![ ]*/usr/bin/env python$|#!/usr/bin/env python2|" *

	cd $pkgdir/opt/cxoffice/lib/python
	sed -i -e "s|#![ ]*/usr/bin/python$|#!/usr/bin/python2|" *.py
	sed -i -e "s|#![ ]*/usr/bin/env python$|#!/usr/bin/env python2|" *.py

	# Fix Auto update error
	install -m 644 -D "$srcdir/cxoffice.conf" "$pkgdir/opt/cxoffice/etc/cxoffice.conf"
}


md5sums=('909be4ed18ce982a3d82bf8520f638e4'
         '2a3d1f5199af8868f8cc21daa2a360ad')
