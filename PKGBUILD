# custom variables
_hkgname=strptime
_licensefile=LICENSE

# PKGBUILD options/directives
pkgname=haskell-strptime
pkgver=1.0.2
pkgrel=1
pkgdesc="Efficient parsing of LocalTime using a binding to C's strptime,
with some extra features (i.e. fractional seconds)"
url="http://hackage.haskell.org/package/${_hkgname}"
license=("BSD3")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc=7.0.3-2"
         "sh"
         "haskell-bytestring=0.9.1.10"
         "haskell-time=1.2.0.3")
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install="${pkgname}.install"
sha256sums=("63c20bc49f85347532743becb88421192e8f5a0fd026fa77cad9dff5e42d77c5")

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    
    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
