# Maintainer: Jeffrey Thompson <coldie@coldie.net>

pkgname=('otf-apple-newyork')
pkgver=1
pkgrel=1
pkgdesc="Apple-designed serif typeface based on historical type."
url=https://developer.apple.com/fonts/
arch=('any')
license=('custom')
makedepends=('p7zip')
sha256sums=('fb754a15fd2bf0e5ea2571dd206ea1b586a51ca8e71743b2c44bf3bb34343c24')

source=("$pkgname-$pkgver.dmg::https://developer.apple.com/design/downloads/NY-Font.dmg")
noextract=("${source[@]%%::*}")
groups=("apple-fonts")

prepare() {
	mkdir -p "$pkgname-$pkgver"
	7z e -y -so "$pkgname-$pkgver.dmg" '-i!*/*.pkg' > $pkgname.pkg
	bsdtar -xOf $pkgname.pkg 'Resources/English.lproj/license.rtf' | xz > "$pkgname-$pkgver/LICENSE.rtf.xz"
	bsdtar -xOf $pkgname.pkg 'NYFonts.pkg/Payload' > $pkgname.cpio
	bsdtar -C . -s ',.*/,,g' -xzf $pkgname.cpio \*.otf
	mv *.otf "$pkgname-$pkgver"

	7z e -y -r "-o$pkgname-$pkgver" '-i!*.otf' "$pkgname.cpio"

	rm -f "$pkgname.pkg"
	rm -f "$pkgname.cpio"
}

package() {
	install -Dm644 $pkgname-$pkgver/LICENSE.rtf.xz "$pkgdir/usr/share/licenses/$pkgname/LICENSE.rtf.xz"
	install -d "$pkgdir/usr/share/fonts/OTF"
	install -m644 $pkgname-$pkgver/*.otf "$pkgdir/usr/share/fonts/OTF"
}
