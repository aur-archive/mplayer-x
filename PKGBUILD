# $Id$
# Maintainer : Ionut Biru <ibiru@archlinux.org> wxg <wxg4dev@gmail.com
# Contributor: Hugo Doria <hugo@archlinux.org>

pkgbase=mplayer
pkgname="mplayer-x"
true && pkgname=('mplayer' 'mencoder')
pkgver=37051
pkgrel=3
arch=('i686' 'x86_64')
makedepends=(
  'libxxf86dga' 'libxxf86vm' 'libmad' 'libxinerama' 'sdl' 'lame' 'libtheora'
  'xvidcore' 'libmng' 'libxss' 'libgl' 'smbclient' 'aalib' 'jack' 'libcaca'
  'x264' 'faac' 'faad2' 'lirc-utils'  'libxvmc' 'enca' 'libvdpau' 'opencore-amr'
  'libdca' 'a52dec' 'schroedinger' 'libvpx' 'libpulse' 'fribidi' 'unzip' 'mesa'
  'live-media' 'yasm' 'git' 'fontconfig' 'mpg123' 'ladspa' 'libass' 'libbluray'
  'libcdio-paranoia' 'opus'
)
license=('GPL')
url='http://www.mplayerhq.hu/'
options=('!buildflags' '!emptydirs')
source=($pkgbase-$pkgver::svn://svn.mplayerhq.hu/mplayer/trunk#revision=$pkgver
        http://ffmpeg.org/releases/ffmpeg-2.2.tar.bz2
        http://www.mplayerhq.hu/MPlayer/skins/Blue-1.10.tar.bz2
        mplayer.desktop
        cdio-includes.patch
        include-samba-4.0.patch)
md5sums=('SKIP'
         '744febca199548c9393b1f1ed05ccdd8'
         'd0d7baf1e84ba95f4456c51b50d99b14'
         '62f44a58f072b2b1a3c3d3e4976d64b3'
         '7b5be7191aafbea64218dc4916343bbc'
         '868a92bdef148df7f38bfa992b26ce9d')

pkgver() {
  cd $pkgbase-$pkgver
  svnversion
}

prepare() {
  cd $pkgbase-$pkgver
  mv ../ffmpeg-2.2 ./ffmpeg

  patch -p0 -i ../cdio-includes.patch
  patch -p1 -i ../include-samba-4.0.patch

  ./version.sh
}

build() {
  cd $pkgbase-$pkgver

  ./configure --prefix=/usr \
    --enable-runtime-cpudetection \
    --enable-gui \
    --disable-arts \
    --disable-liblzo \
    --disable-speex \
    --disable-openal \
    --disable-libdv \
    --disable-musepack \
    --disable-esd \
    --disable-mga \
    --disable-ass-internal \
    --disable-cdparanoia \
    --enable-xvmc \
    --enable-radio \
    --enable-radio-capture \
    --enable-smb \
    --language=all \
    --confdir=/etc/mplayer
  [[ "$CARCH" = "i686" ]] &&  sed 's|-march=i486|-march=i686|g' -i config.mak

  make
}

package_mplayer() {
  pkgdesc='Media player for Linux'
  install=mplayer.install
  backup=('etc/mplayer/codecs.conf' 'etc/mplayer/input.conf')
  depends=(
    'desktop-file-utils' 'ttf-font' 'enca' 'libxss' 'a52dec' 'libvpx'
    'lirc-utils' 'x264' 'libmng' 'libdca' 'aalib' 'lame' 'fontconfig'
    'libgl' 'libxinerama' 'libvdpau' 'libpulse' 'smbclient' 'xvidcore'
    'opencore-amr' 'jack' 'libmad' 'sdl' 'libtheora' 'libcaca' 'libxxf86dga'
    'fribidi' 'libjpeg' 'faac' 'faad2' 'libxvmc' 'schroedinger' 'mpg123'
    'libass' 'libxxf86vm' 'libbluray' 'libcdio-paranoia' 'opus'
  )

  cd $pkgbase-$pkgver
  make DESTDIR="$pkgdir" install-mplayer install-mplayer-man

  install -Dm644 etc/{codecs.conf,input.conf,example.conf} \
    "$pkgdir/etc/mplayer/"

  # desktop file (FS#14770)
  install -Dm644 ../mplayer.desktop \
    "$pkgdir"/usr/share/applications/mplayer.desktop
  install -Dm644 etc/mplayer256x256.png \
    "$pkgdir"/usr/share/pixmaps/mplayer.png
  install -dm755 ${pkgdir}/usr/share/mplayer/skins/   
  cp -r "$srcdir/Blue" \
        "$pkgdir/usr/share/mplayer/skins/default"  
}

package_mencoder() {
  pkgdesc='Free command line video decoding, encoding and filtering tool'
  depends=(
    'enca' 'a52dec' 'libvpx' 'x264' 'libmng' 'libdca' 'bzip2' 'lame'
    'alsa-lib' 'fontconfig' 'giflib' 'libpng' 'smbclient' 'xvidcore'
    'opencore-amr' 'libmad' 'libtheora' 'fribidi' 'libjpeg' 'faac' 'faad2'
    'schroedinger' 'mpg123' 'libass' 'libbluray' 'libcdio-paranoia'
    'libvorbis' 'opus'
  )

  make -C $pkgbase-$pkgver DESTDIR="$pkgdir" install-mencoder install-mencoder-man
  find "$pkgdir"/usr/share/man -name mplayer.1 -exec rename mplayer.1 mencoder.1 {} +
}