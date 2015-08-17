# Maintainer: Yeison Cardona <yeison.eng@gmail.com>

githubaccount="PinguinoIDE"
pkgshortname="pinguino"
pkgname="pinguino-ide-dev"
reponame="pinguino-ide"
pkgver="11.0.0"
shortver="11.0"
pkgrel="4"
pkgfullver="$pkgver-$pkgrel"
pkggitrelease="v$pkgfullver"
appname="Pinguino IDE development"

installdir="/usr/share/pinguino-11.0"

pkgdesc="Development version from git repository, very unstable, be careful."
arch=("i686" "x86_64")
url="http://www.pinguino.cc/"
license=("GPL")

makedepends=("git")

depends=("pinguino-libraries" "pinguino-compilers"
         "python2"
         "python2-pyside" "python2-beautifulsoup4" "python2-pyusb"
         "python2-gitpython" "python2-pysvn")

optdepends=("python2-pyserial" "python2-pyusb" "python2-pynguino")

if test "$CARCH" == x86_64; then
  depends+=("lib32-gcc-libs" "lib32-zlib")
fi

gitroot="git://github.com/${githubaccount}/${reponame}"
md5sums=('SKIP')

package() {

  # clone from git with name $pkgname
  cd ${srcdir}
  if [ -d ${pkgname} ] ; then
    cd ${pkgname}
    git checkout *
    git pull origin
  else
    git clone ${gitroot} ${pkgname} --depth=1
  fi

  cd ${startdir}
  install -m755 -d ${pkgdir}/${installdir}

  #copying pinguino-ide
  cp -Rvf ${srcdir}/${pkgname}/* ${pkgdir}/${installdir}

  cd ${srcdir}
  # write .desktop
  install -m755 -d ${pkgdir}/usr/share/applications/  
  echo "
  [Desktop Entry]
  Version=$pkgfullver
  Type=Application
  Name=$appname
  Comment=$pkgdesc
  Icon=${installdir}/qtgui/resources/art/windowIcon.svg
  Exec=python2 ${installdir}/pinguino.py
  Path=${installdir}
  Terminal=false
  StartupNotify=false
  Categories=Development;Electronics;Engineering" >> ${pkgdir}/usr/share/applications/${pkgname}.desktop

  # write command launcher
  install -m755 -d ${pkgdir}/usr/bin/ 
  echo "#!/bin/bash" >> ${pkgdir}/usr/bin/${pkgshortname}${shortver}-dev
  echo "python2 ${installdir}/pinguino.py" >> ${pkgdir}/usr/bin/${pkgshortname}${shortver}-dev
  chmod a+x ${pkgdir}/usr/bin/${pkgshortname}${shortver}-dev

}
