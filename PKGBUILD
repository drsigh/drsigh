pkgname=draftsight
pkgver=2019SP3
pkgrel=3
pkgdesc="Freeware CAD software for DWG/DXF files."
arch=('x86_64')
url="http://www.3ds.com/products/draftsight"
options=('!strip')
license=('custom')
depends=(alsa-lib
desktop-file-utils
fontconfig
gcc-libs
glib2
gtk2
libfaketime
libcups
libgl
libice
libmariadbclient
libmng
libpng12
libsm
libx11
libxext
libxrender
libxslt
postgresql-libs
qt5-base
qt5-x11extras
zlib)
source=("draftsight-$pkgver.rpm::https://web.archive.org/web/20200602160014/http://dl-ak.solidworks.com/nonsecure/draftsight/${pkgver}/draftSight.rpm")
sha256sums=('fd0567969da3b011d463e105e7b25a957646a7fbe7bda32eb08b18098886b08b')

_dest_dir="opt/${pkgname}"

prepare() {
  cat > ${pkgname}.sh<<EOF
#!/bin/sh
cd "/${_dest_dir}/Linux"
unset XDG_CURRENT_DESKTOP
unset DESKTOP_SESSION
unset GNOME_DESKTOP_SESSION_ID
unset vblank_mode
export QT_AUTO_SCREEN_SCALE_FACTOR=0
export LD_PRELOAD=/usr/lib/libfreetype.so
unset XDG_CURRENT_DESKTOP
unset DESKTOP_SESSION
unset GNOME_DESKTOP_SESSION_ID
shift > /dev/null 2>&1
vblank_mode=0 faketime '2020-01-14 10:33:19' "/${_dest_dir}/Linux/DraftSight" "$@"
EOF
chmod +x ${pkgname}.sh
  cat > ${pkgname}.desktop<<EOF
[Desktop Entry]
Type=Application
Name=${pkgname}
Comment=Freeware CAD software for your DWG/DXF files.
Path=/${_dest_dir}/Linux
Exec=${pkgname} %U
Icon=${pkgname}
Terminal=false
Categories=Graphics;2DGraphics;
EOF
}

package() {
  cd opt/dassault-systemes/DraftSight

  mkdir -p "${pkgdir}/${_dest_dir}"
  install -Dm644 Eula/english/eula.htm "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"

  for size in "16x16" "32x32" "48x48" "64x64" "128x128"
  do
    install -Dm644 Resources/pixmaps/$size/program.png "$pkgdir/usr/share/icons/hicolor/$size/apps/${pkgname}.png"
    #install -Dm644 Resources/pixmaps/$size/file-dwg.png "$pkgdir/usr/share/icons/hicolor/$size/mimetypes/file-dwg.png"
    #install -Dm644 Resources/pixmaps/$size/file-dxf.png "$pkgdir/usr/share/icons/hicolor/$size/mimetypes/file-dxf.png"
    #install -Dm644 Resources/pixmaps/$size/file-dwt.png "$pkgdir/usr/share/icons/hicolor/$size/mimetypes/file-dwt.png"
  done

  install -Dm644 Resources/dassault-systemes_draftsight-dwg.xml "$pkgdir/usr/share/mime/application/dassault-systemes_$pkgname-dwg.xml"
  install -Dm644 Resources/dassault-systemes_draftsight-dxf.xml "$pkgdir/usr/share/mime/application/dassault-systemes_$pkgname-dxf.xml"
  install -Dm644 Resources/dassault-systemes_draftsight-dwt.xml "$pkgdir/usr/share/mime/application/dassault-systemes_$pkgname-dwt.xml"

  install -Dm644 "${srcdir}/${pkgname}.desktop" "${pkgdir}/usr/share/applications/${pkgname}.desktop"
  mv * "${pkgdir}/${_dest_dir}"

  # fix pdf export module lib load
  ln -s /usr/lib/libpcre.so.1 "${pkgdir}/${_dest_dir}/Libraries/libpcre.so.3"
  
  # install launch script
  install -Dm755 "${srcdir}/${pkgname}.sh" "${pkgdir}/usr/bin/${pkgname}"
}
