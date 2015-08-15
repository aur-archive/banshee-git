# Maintainer: Charles Bos <charlesbos1 AT gmail>
# Contributor: Mauro Fruet <maurofruet@gmail.com>
# Contributor: oke3 <Sekereg [at] gmx [dot] com>
# Contributor: Manuel Mazzuola <origin.of@gmail.com>
# Contributor: neuromante <lorenzo.nizzi.grifi@gmail.com>

pkgname=banshee-git
_gitname=banshee
pkgver=2.9.1.r157.g34088cf
pkgrel=1
pkgdesc="Music management and playback for GNOME"
arch=('i686' 'x86_64')
url="http://banshee.fm/"
license=('MIT')
depends=('libxxf86vm' 'gst-plugins-base' 'gst-plugins-base-libs' 'gst-plugins-good' 'mono-addins' 'dbus-sharp-glib' 'webkitgtk2' 'libsoup-gnome' 'gdata-sharp' 'taglib-sharp' 'gkeyfile-sharp' 'gconf-sharp' 'libmtp' 'libgpod' 'mono-zeroconf' 'desktop-file-utils' 'hicolor-icon-theme' 'media-player-info' 'mono-upnp' 'gudev-sharp-git')
makedepends=('git' 'intltool' 'gnome-doc-utils' 'gtk-sharp-beans' 'gnome-common')
optdepends=('gst-plugins-bad: Extra media codecs'
            'gst-plugins-ugly: Extra media codecs'
            'gst-libav: Extra media codecs'
            'brasero: CD burning')
options=('!libtool')
provides=('banshee=2.9.2')
conflicts=('banshee')
install=$pkgname.install
source=('git://git.gnome.org/banshee'
        'Use-dbus-2.patch'
        'Fix-empty-sourceview.patch')
sha256sums=('SKIP'
            '12cccab312c1e0114040c618f1da8b5cf678648fafa41925ce6f3391e33c7c37'
            '834d094d79bbea0eef51360f6e1873d9209a067ece25097dbb9ae4c0b73a4422')

pkgver() {
  cd $_gitname
  git describe --long --tags | sed -r 's/([^-]*-g)/r\1/;s/-/./g'
}

prepare() {
  cd $_gitname
  patch -Np1 -i "${srcdir}/Use-dbus-2.patch"
  patch -Np1 -i "${srcdir}/Fix-empty-sourceview.patch"
}

build() {
  DBUS_SHARP_GLIB_LIBS="/usr/lib/mono/dbus-sharp-glib-2.0"
  export MONO_SHARED_DIR="$srcdir/.wabi"
  mkdir -p "$MONO_SHARED_DIR"

  cd "$srcdir/$_gitname"

  MCS=/usr/bin/dmcs ./autogen.sh --prefix=/usr --sysconfdir=/etc --localstatedir=/var \
              --disable-docs \
              --disable-static \
              --disable-scrollkeeper \
              --disable-schemas-install \
              --disable-boo \
              --with-vendor-build-id=ArchLinux \
              --enable-gst-native \
              --disable-gio-hardware \
              --disable-appledevice
  make
}

package() {
  export MONO_SHARED_DIR="$srcdir/.wabi"
  mkdir -p "$MONO_SHARED_DIR"

  cd "$srcdir/$_gitname"

  make DESTDIR="$pkgdir" install

  install -D -m644 COPYING "$pkgdir/usr/share/licenses/$pkgname/COPYING"
}
