# Maintainer: Luis Vervaet <luisvervaet@gmail.com>
# Maintainer: Peter Jung <admin@ptr1337.dev>
# Contributor: NextWorks <nextworks@protonmail.com>
# Contributor: Alad Wenter <alad@archlinux.org>
# Contributor: Luna Jernberg <droidbittin@gmail.com>
# Contributor: Hilton Medeiros <medeiros.hilton@gmail.com>
# Contributor: Simon Brulhart <simon@brulhart.me>
# Contributor: Det <nimetonmaili g-mail>, Achilleas Pipinellis, speed145a, Schnouki, aus
# Contributor: Ameer Arsala <ameer.arsala03@gmail.com>

pkgname=zen-browser-compat-bin
_pkgname=zen-browser
_desktopname=zen
pkgver=1.8.2b
pkgrel=1
pkgdesc="Performance oriented Firefox-based web browser"
arch=('x86_64' 'aarch64')
url="https://github.com/zen-browser/desktop"
license=(MPL-2.0)
depends=(gtk3 libxt mime-types dbus-glib nss ttf-font systemd)
optdepends=('ffmpeg: H264/AAC/MP3 decoding'
  'networkmanager: Location detection via available WiFi networks'
  'libnotify: Notification integration'
  'pulse-native-provider: Audio support'
  'speech-dispatcher: Text-to-Speech'
  'hunspell-en_US: Spell checking, American English')
options=(!strip)
provides=("zen-browser=$pkgver")
conflicts=('zen-browser')

source_x86_64=("zen-browser-$pkgver-x86_64.tar.bz2::https://github.com/zen-browser/desktop/releases/download/$pkgver/zen.linux-x86_64.tar.xz")
source_aarch64=("zen-browser-$pkgver-aarch64.tar.bz2::https://github.com/zen-browser/desktop/releases/download/$pkgver/zen.linux-aarch64.tar.xz")

source=("$_pkgname.sh"
  "$_desktopname.desktop"
  "policies.json")
sha256sums=('c1a4ad7fd56600947b17733684aea6dec2e4cc42396b1be628def8d4be47ac6b'
            'aa44bc42f783074c9510ec1253df43b5e0191da1f08b63277bc8eb5cbd56b83f'
            '7c9fc215ed5f7fb608e07b05c0b9c6e1ea80a9801b5fecacae35fb3287d4e619')
sha256sums_x86_64=('b4bec09c67eb4b0d86f4ca1b2b4a065f9217bbf0836f0ef82462ae285ad80b47')
sha256sums_aarch64=('026981bbe03c97ebd39e3458b197346686f362a5e6cf0cde0629205fde7b3d1d')

package() {
  OPT_ROOT="usr/lib"
  OPT_PATH="$OPT_ROOT/${_pkgname}"

  # Create directories
  mkdir -p "$pkgdir"/usr/bin
  mkdir -p "$pkgdir"/usr/share/applications
  mkdir -p "${pkgdir}/${OPT_ROOT}"

  # Install (install package files)
  cp -r "${_desktopname}/" "${pkgdir}"/${OPT_PATH}

  # Launchers (install launcher script)
  install -m755 $_pkgname.sh "$pkgdir"/usr/bin/$_pkgname

  # Desktops (install .desktop files)
  install -m644 *.desktop "$pkgdir"/usr/share/applications/

  # Icons
  for i in 16x16 32x32 48x48 64x64 128x128; do
    install -d "$pkgdir"/usr/share/icons/hicolor/$i/apps/
    ln -s /$OPT_PATH/browser/chrome/icons/default/default${i/x*/}.png \
      "$pkgdir"/usr/share/icons/hicolor/$i/apps/$_pkgname.png
  done

  # Use system-provided dictionaries
  ln -Ts /usr/share/hunspell "$pkgdir"/${OPT_PATH}/dictionaries
  ln -Ts /usr/share/hyphen "$pkgdir"/${OPT_PATH}/hyphenation

  # Use system certificates
  ln -sf /usr/lib/libnssckbi.so "$pkgdir"/${OPT_PATH}/libnssckbi.so

  # Disable update checks (managed by pacman)
  mkdir "$pkgdir"/${OPT_PATH}/distribution
  install -m644 "$srcdir"/policies.json "$pkgdir"/${OPT_PATH}/distribution/
}
