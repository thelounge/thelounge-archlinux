# Maintainer: Reto Brunner <brunnre8@gmail.com>
# Maintainer: Maxime Poulin <maxpoulin64@gmail.com>
pkgname=thelounge
pkgver=4.1.0
pkgrel=1
pkgdesc='Modern self-hosted web IRC client'
url='https://thelounge.chat/'
arch=('any')
license=('MIT')
depends=('nodejs')
makedepends=('npm')
backup=('etc/thelounge/config.js')
source=(
    "https://registry.npmjs.org/$pkgname/-/$pkgname-$pkgver.tgz"
    'system.service'
    'user.service'
    'sysusers.d'
    'tmpfiles.d'
)
noextract=("$pkgname-$pkgver.tgz")
sha256sums=('c4058bb9db0ef94480203f88c3d989945c2df0a5636ba3637040ef3e58237846'
            '8816c0b871689519f1614dce1805fde9924c03e2ca3d836902f56536ada93840'
            'c609f3309f54bd6285e99ff29ca2464828bec7bbbca67243ee688bd2d605dbf0'
            '30fab63b8a4ffcfdda4c5b8d7c66822a323c4f1de6ca62b77fe9500f4befc0a5'
            '1cac32233da5c8eee576b43fd28cadd29ac3f79cdfdcc94fbb2a4ae529f4f146')

package() {
    export NODE_ENV=production

    npm install -g --user root --prefix "$pkgdir/usr" "$pkgname-$pkgver.tgz" --cache "${srcdir}/npm-cache"

    echo /etc/thelounge/home > "$pkgdir/usr/lib/node_modules/$pkgname/.thelounge_home"

    # add default config
    install -Dm 644 "$pkgdir/usr/lib/node_modules/$pkgname/defaults/config.js" "$pkgdir/etc/thelounge/config.js"
    mkdir "$pkgdir/etc/thelounge/home"
    ln -s "../config.js" "$pkgdir/etc/thelounge/home/config.js"

    # services
    install -Dm644 "$srcdir/system.service" "$pkgdir/usr/lib/systemd/system/$pkgname.service"
    install -Dm644 "$srcdir/user.service" "$pkgdir/usr/lib/systemd/user/$pkgname.service"

    # setting up system user
    install -Dm644 "${srcdir}/sysusers.d" "${pkgdir}/usr/lib/sysusers.d/thelounge.conf"
    install -Dm644 "${srcdir}/tmpfiles.d" "${pkgdir}/usr/lib/tmpfiles.d/thelounge.conf"

    # Non-deterministic race in npm gives 777 permissions to random directories.
    # See https://github.com/npm/npm/issues/9359 for details.
    find "$pkgdir/usr" -type d -exec chmod 755 '{}' +
}
