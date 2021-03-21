# Maintainer: Sebastian Wiesner <sebastian@swsnr.de>

pkgname=zsa-wally
pkgver=2.1.0
pkgrel=1
pkgdesc="Flash your ZSA Keyboard the EZ way."
arch=('i686' 'x86_64')
url="https://github.com/zsa/wally"
license=("MIT")
depends=("gtk3" "webkit2gtk" "libusb")
makedepends=("go" "npm")
source=("$pkgname-$pkgver.tar.gz::https://github.com/zsa/wally/archive/refs/tags/$pkgver-linux.tar.gz")
md5sums=('f4a98a0e51d5d0b6d23a95858d230e11')
sha1sums=('deb20d7c9b1a1eb799bb2288540fb7119f1f8ba2')
sha512sums=('d84c469f7d43ba86eb5d0d527a17b588ea12e0272897231392e0572cef0a80f806948a225e263806e5cf7b2fc95eb58e02468ecbe8d12f69ec07919eb364e789')

prepare() {
    cd "wally-$pkgver-linux"

    export GOPATH="$srcdir/gopath"
    go get -u github.com/wailsapp/wails/cmd/wails
}

build() {
    cd "wally-$pkgver-linux"

    export GOPATH="$srcdir/gopath"
    export PATH="$(go env GOPATH)/bin:$PATH"

    export CGO_CPPFLAGS="${CPPFLAGS}"
    export CGO_CFLAGS="${CFLAGS}"
    export CGO_CXXFLAGS="${CXXFLAGS}"
    export CGO_LDFLAGS="${LDFLAGS}"
    export GOFLAGS="-buildmode=pie -trimpath"
    # Don't know what this does but it's in the install.linux.sh script from upstream
    export CGO_ENABLED=1

    wails build
}

package() {
    cd "wally-$pkgver-linux"
    install -Dm755 build/wally "$pkgdir/usr/bin/wally"
    install -Dm644 -t "$pkgdir/usr/lib/udev/rules.d/" dist/linux64/50-oryx.rules dist/linux64/50-wally.rules
    install -Dm644 dist/linux64/wally.desktop "$pkgdir/usr/share/applications/wally.desktop"
    install -Dm644 appicon.png "$pkgdir/usr/share/pixmaps/wally.png"
    # This file is only on master; meanwhile use the license from the Debian package copyright file
    install -Dm644 dist/ppa/wally/template/debian/copyright "$pkgdir/usr/share/licenses/$pkgname/copyright"
    #install -Dm644 license.md "$pkgdir/usr/share/licenses/$pkgname/license.md"
}
