#Maintainer: Xyne <gro xunilhcra enyx, backwards>
#Author fork patch: IA <itachi522ru@gmail.com>
_NAME_ACCOUNT_GITHUB=naruto522ru
old_pkgname=reflector
pkgname=reflector2
pkgver=2023
pkgrel=3
pkgdesc='A Python 3 module and script to retrieve and filter the latest Pacman mirror list. With support for mirrors to the status page via environment.'
arch=(any)
license=(GPL2)
url="https://xyne.dev/projects/reflector"
depends=(python)
makedepends=(python-build python-installer python-setuptools python-wheel)
optdepends=('rsync: rate rsync mirrors')
backup=(etc/xdg/reflector2/reflector2.conf)
#Original Source URL
#source=(https://xyne.dev/projects/reflector/src/reflector-2023.tar.xz{,.sig})
source=(reflector-2023.tar.xz{,.sig})

sha512sums=('11aec550c15080695525409f11eae6d4b545df8b37a8e0727de939eefec2b2fa6aa95c5c3500a6c8a940b6060cdaf2526430ed47e01a3c6f098e1b77189eb479'
            'SKIP')
b2sums=('24ee3610eb94e4412bceba40f4763def380028683358cb1016546a4d0bb12eaa69aba4e83b7998ebb74ee6f025f9f342901d076e44302da47d0ed14b727fb9c1'
        'SKIP')
validpgpkeys=('D89FAAEB4CECAFD199A2F5E612C6F735F7A9A519')

build() {
 cd $old_pkgname-$pkgver
##Patch Section
mv -v $old_pkgname $pkgname
mv -v $old_pkgname.service $pkgname.service
mv -v $old_pkgname.timer $pkgname.timer
mv -v Reflector.py Reflector2.py
mv -v man/$old_pkgname.1.gz man/$pkgname.1.gz
mv -v man.md/$old_pkgname.1.md man.md/$pkgname.1.md
mv -v $old_pkgname.conf $pkgname.conf
zcat man/reflector2.1.gz > ./man/reflector2.1 && rm ./man/reflector2.1.gz
sed -i 's/Reflector/Reflector2/g' setup.py $pkgname.conf Reflector2.py ./man/reflector2.1 reflector2 $pkgname.timer $pkgname.service
sed -i 's/reflector/reflector2/g' setup.py $pkgname.conf Reflector2.py ./man/reflector2.1 reflector2 $pkgname.timer $pkgname.service
sed -i '52d' Reflector2.py
#Paste change in file
sed -i '52iDEFAULT_URL \= \"https://archlinux.org/mirrors/status/json/\"' Reflector2.py
sed -i '53iURL \= os.getenv\(\"MIRROR_URL\"\, DEFAULT_URL\)' Reflector2.py
sed -i '13d' $pkgname.service
sed -i "13iEnvironment\=\"XDG_CACHE_HOME\=/var/cache/reflector2\" \"MIRROR_URL=https://raw.githubusercontent.com/${_NAME_ACCOUNT_GITHUB}/dot_arch/refs/heads/main/json\"" $pkgname.service
#
#Fix URL in service file
sed -i '3s/Reflector2/Reflector/g' $pkgname.service
#
#Compress manual page
gzip ./man/reflector2.1
##
  python -m build --wheel --no-isolation
}

package ()
{
  cd $old_pkgname-$pkgver
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -Dm644 "man/${pkgname}.1.gz" "${pkgdir}/usr/share/man/man1/${pkgname}.1.gz"
  install -Dm644 "${pkgname}.service" "$pkgdir/usr/lib/systemd/system/${pkgname}.service"
  install -Dm644 "${pkgname}.timer" "$pkgdir/usr/lib/systemd/system/${pkgname}.timer"
  install -Dm644 'reflector2.conf' "$pkgdir/etc/xdg/reflector2/reflector2.conf"
}


# vim: set ts=2 sw=2 et:
