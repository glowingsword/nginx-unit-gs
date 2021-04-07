# Maintainer: Andrei Gutu <glowingsword@gmail.com>
# Contributor: KawaiDesu <zmey1992@ya.ru>
# Contributor: @whoami
# Contributor: Roman Voropaev <voropaev.roma@gmail.com>
# Contributor: Julian Brost <julian@0x4a42.net>
# Contributor: Lorenzo Gabriele <lorenzolespaul@gmail.com>

pkgbase='nginx-unit-gs'
pkgname=('nginx-unitd-gs'
         'nginx-libunit-gs'
         'nginx-unit-python-gs'
         'nginx-unit-php-gs')
_shortname='unit'
pkgver=1.23.0
pkgrel=1
pkgdesc="Lightweight, dynamic, open-source server for diverse web applications."
arch=('x86_64')
url="https://unit.nginx.org/"
license=('Apache')
source=("https://unit.nginx.org/download/unit-$pkgver.tar.gz"
        'unit.service')
sha256sums=('40df5c5c4132bf9ca15fa82edfe0cc0daae488c2f473d6d27706d537b5859b42'
            '8c9b2f732d6e50aa747aa7703303e5fff69f5abc6f5fc1741b774b422e029606')
makedepends=('php-embed' 'php7-embed' 'python')

build() {
  cd "${srcdir}/${_shortname}-${pkgver}"
  ./configure --prefix=/usr \
              --sbindir=/usr/bin \
              --modules="/usr/lib/$pkgbase" \
              --state="/var/lib/$pkgbase" \
              --pid="/run/$pkgbase.pid" \
              --log="/var/log/$pkgbase.log" \
              --control="/run/$pkgbase.control.sock" \
              --tmp="/tmp" \
              --openssl
  ./configure python --config=python3-config
  ./configure php --module=php74  --config=/usr/bin/php-config7 --lib-path=/usr/lib/php7 
  ./configure php --module=php --config=/usr/bin/php-config --lib-path=/usr/lib/php
  make all
}

package_nginx-unitd-gs() {
  depends=('glibc' 'openssl')

  cd "${srcdir}/${_shortname}-${pkgver}"
  make DESTDIR="${pkgdir}" unitd-install
  install -m 644 -D "${srcdir}/unit.service" "${pkgdir}/usr/lib/systemd/system/unit.service"
}

package_nginx-libunit-gs() {
  cd "${srcdir}/${_shortname}-${pkgver}"
  make DESTDIR="${pkgdir}" libunit-install
}

package_nginx-unit-python-gs() {
  depends=('nginx-unitd-gs' 'python')

  cd "${srcdir}/${_shortname}-${pkgver}"
  make DESTDIR="${pkgdir}" python3-install
}


package_nginx-unit-php-gs() {
  depends=('nginx-unitd-gs' 'php-embed' 'php7-embed')

  cd "${srcdir}/${_shortname}-${pkgver}"
  make DESTDIR="${pkgdir}" php-install
  make DESTDIR="${pkgdir}" php74-install
}
