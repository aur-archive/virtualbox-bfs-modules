# Contributor: JokerBoy <jokerboy at punctweb dot ro>
# Contributor: Ionut Biru <ibiru@archlinux.org>

pkgname=virtualbox-bfs-modules
pkgver=4.2.0
pkgrel=2
pkgdesc='Host linux-bfs kernel modules for VirtualBox'
arch=('i686' 'x86_64')
url='http://virtualbox.org'
license=('GPL')
depends=('linux-bfs>=3.5' 'linux-bfs<3.6')
makedepends=('linux-bfs-headers'
             "virtualbox-host-source>=$pkgver")
provides=('virtualbox-host-modules')
install=virtualbox-modules.install

_extramodules=extramodules-3.5-bfs
_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"

build() {
  # dkms need modification to be run as user
  cp -r /var/lib/dkms .
  echo "dkms_tree='$srcdir/dkms'" > dkms.conf
  # build host modules
  dkms --dkmsframework dkms.conf build "vboxhost/$pkgver" -k "$_kernver"
}

package(){
  install -dm755 "$pkgdir/usr/lib/modules/$_extramodules"
  cd "dkms/vboxhost/$pkgver/$_kernver/$CARCH/module"
  install -m644 * "$pkgdir/usr/lib/modules/$_extramodules"
  find "$pkgdir" -name '*.ko' -exec gzip -9 {} +
  sed -ie "s/EXTRAMODULES='.*'/EXTRAMODULES='$_extramodules'/" "$startdir/virtualbox-modules.install"
}
