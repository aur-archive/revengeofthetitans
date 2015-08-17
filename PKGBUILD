# Maintainer: Limao Luo <luolimao+AUR@gmail.com>
# Contributor: Marek Otahal <markotahal@gmail.com>
# Contributor: Artificial Intelligence <polarbeard@gmail.com>

pkgname=revengeofthetitans
pkgver=1.80.20
pkgrel=1
pkgdesc="Construct and command your ground defences in a series of increasingly massive battles across the solar system, in a frenetic arcade mash-up of Real Time Strategy and Tower Defense!"
arch=(i686 x86_64)
url=http://www.puppygames.net/revenge-of-the-titans/
license=(custom)
depends=(java-runtime lib32-gtk2 openal)
[[ $CARCH = "i686" ]] && depends=(${depends[@]/lib32-/})
install=$pkgname.install
source=(revenge.desktop)
sha256sums=('63a8df5193aeeb4a633fbb4e793fd279d3c0753b77f462e876942af348b02700')
sha512sums=('8377074fc081864a52e99ff1f7703b0ebbe9cda6bb5fd258fb1c196f832f05f929744e626743f12a2a4a29bea3ea646ce61b940fd00b893ce6afe5bf02025eea')

[[ $CARCH == "i686" ]] && _arch=i386 || _arch=amd64
_gamepkg=(RevengeOfTheTitans{,-HIB-${pkgver//./}}-$_arch.tar.gz)

PKGEXT=.pkg.tar

build() {
    msg "You need a full copy of this game in order to install it"
    msg "Searching for ${_gamepkg[0]} or ${_gamepkg[1]} in $startdir"
    pkgpath="$startdir"
    if [[ ! ( -f "$startdir/${_gamepkg[0]}" || -f "$startdir/${_gamepkg[1]}" ) ]]; then
        error "Game package not found.
        Please type the absolute path that contains ${_gamepkg[*]} (e.g. /home/joe):"
        read pkgpath
        [[ ! ( -f "$pkgpath/${_gamepkg[0]}" || -f "$pkgpath/${_gamepkg[1]}" ) ]] && \
            error "Unable to find game package." && return 1
    fi
    msg "Found game package, installing..."

    [[ -f "$pkgpath/${_gamepkg[0]}" ]] && bsdtar -xf "$pkgpath/${_gamepkg[0]}" || bsdtar -xf "$pkgpath/${_gamepkg[1]}"
    sed -ri "s|^(INSTDIR=\")[^\"]+|\1/opt/rott/|" $pkgname/revenge.sh
}

package() {
    desktop-file-install revenge.desktop --dir "$pkgdir"/usr/share/applications/
    cd $pkgname/
    install -Dm755 revenge.sh "$pkgdir"/usr/bin/revenge
    install -d "$pkgdir"/opt/rott
    install -m755 *.so "$pkgdir"/opt/rott/
    install -m644 *.{jar,png} "$pkgdir"/opt/rott/
    install -Dm644 "$pkgdir"/opt/rott/revenge.png "$pkgdir"/usr/share/pixmaps/revenge.png
}
