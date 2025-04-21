# Copyright: https://mpr.makedeb.org/packages/prismlauncher Shared Under GPL-3
# This is modifed version of prismlauncher makedeb PKGBUILD.
# Its modifed for prismlauncher-cracked github repo.
# This is not endorsed by Prism Launcher or by Prism Launcher Cracked

# Maintainer: Hydriam <163676548+Hydriam@users.noreply.github.com>

# This PKGBUILD is for makepkg on arch linux!

pkgname=prismlauncher-cracked
pkgver=9.4
pkgrel=1
pkgdesc='Minecraft launcher with ability to manage multiple instances.'
arch=('i686' 'x86_64' 'aarch64' 'armv7h' 'riscv64')
url='https://github.com/Diegiwg/PrismLauncher-Cracked'
license=('GPL-3')
depends=(
  'qt6-base'
  'qt6-networkauth'
  'qt6-svg'
  'qt6-imageformats'
  'qt6-5compat'
  'zlib'
)
makedepends=(
  'cmake'
  'extra-cmake-modules'
  'gcc'
  'git'
  'mesa'
  'qt6-base'
  'qt6-networkauth'
  'qt6-5compat'
  'jdk17-openjdk'
  'scdoc'
  'zlib'
  'patch'
)
optdepends=(
  'flite: narrator support'
  'jdk17-openjdk: support for Minecraft versions >= 1.17 and <= 1.20.4'
  'jdk21-openjdk: support for Minecraft versions >= 1.20.5'
  'jre8-openjdk: support for Minecraft versions <= 1.16'
  'xorg-xrandr: xrandr support for Minecraft versions <= 1.12'
  'gamemode: support for GameMode'
  'mangohud: HUD overlay for FPS and temperatures'
)
source=(
  "https://github.com/Diegiwg/PrismLauncher-Cracked/releases/download/${pkgver}/PrismLauncher-${pkgver}.tar.gz"
  'gcc-armv7-fix.patch'
  'version-for-9.4.patch'
  'copyright'
)
sha256sums=('c49d5787882116f1f4a86eb55da888bd84d96efbd0647413064aba3cee9b9515'
            '42394447d4b52c9329ff45f3c700c0eb2090a5803c5de010587d64294c37420f'
            '8af15b3be06c04a159cf96f9fcc2ac92caac6d46af9b7ca6a76fd2ae683048b3'
            '4a063a641496dd622366468b91663082fe9c02aac24c10bc70b221a7eabe1ad4')
postinst=postinst.sh

# allow for ARM support
#TODO: makedeb's hard-coding for x86-64 has been fixed in a future makedeb version
#TODO: these 8 lines make this script match the behavior of future makedeb. When it releases, remove this.
CARCH="$(uname -m)"
CHOST="$(uname -m)-pc-linux-gnu"
CFLAGS=${CFLAGS/-march=x86-64/}
CXXFLAGS=${CXXFLAGS/-march=x86-64/}
CFLAGS=${CFLAGS/-mtune=generic/}
CXXFLAGS=${CXXFLAGS/-mtune=generic/}
CFLAGS=${CFLAGS/-fcf-protection/}
CXXFLAGS=${CXXFLAGS/-fcf-protection/}

# if the user hasn't specified a tuning/architecture, specify our own minimal defaults to cover the earliest CPUs
if [[ ${CFLAGS} != *"-mtune"* && ${CFLAGS} != *"-march"* ]]; then
  case "${CARCH}" in
    amd64)
      CFLAGS+=" -march=x86-64 -fcf-protection"
      CXXFLAGS+=" -march=x86-64 -fcf-protection"
      ;;
    i386)
      CFLAGS+=" -march=i686"
      CXXFLAGS+=" -march=i686"
      ;;
    arm64)
      CFLAGS+=" -march=armv8-a"
      CXXFLAGS+=" -march=armv8-a"
      ;;
    armhf)
      CFLAGS+=" -march=armv7-a+fp"
      CXXFLAGS+=" -march=armv7-a+fp"
      ;;
    riscv64)
      CFLAGS+=" -march=rv64imafdc"
      CXXFLAGS+=" -march=rv64imafdc"
      ;;
  esac
fi

prepare() {
  # workaround https://gcc.gnu.org/bugzilla/show_bug.cgi?id=64860
  # more info: https://github.com/PrismLauncher/PrismLauncher/issues/128
  if [[ "$(uname -m)" = armv7* ]]; then
    echo "GCC / ARMv7 fix is needed for this architecture, applying gcc-armv7-fix.patch"
    patch --directory="PrismLauncher-${pkgver}" --forward --strip=1 --input="${srcdir}/gcc-armv7-fix.patch"
  else
    echo "GCC / ARMv7 fix is not needed for this architecture, skipping gcc-armv7-fix.patch"
  fi

  if [ $pkgver = 9.4 ]; then  
  echo "This release has wrong version number, applying fix for it"
  patch --directory="PrismLauncher-${pkgver}" --forward --strip=1 --input="${srcdir}/version-for-9.4.patch"
  fi
}

build() {
  cd "${srcdir}/PrismLauncher-${pkgver}"
  # As app_binary_name we pass prismlauncher instead of prismlauncher-cracked.
  # So now eveything should will be prismlauncher (like binary) but pacman package should be prismlauncher-cracked
  cmake -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX="/usr" \
    -DLauncher_BUILD_PLATFORM="archlinux" \
    -DLauncher_APP_BINARY_NAME="prismlauncher" \
    -DLauncher_ENABLE_JAVA_DOWNLOADER=ON \
    -DENABLE_LTO=ON \
    -Bbuild -S.
  cmake --build build
}

check() {
  cd "${srcdir}/PrismLauncher-${pkgver}/build"
  ctest . -E Task  # Skip unreliable Task test
}

package() {
  cd "${srcdir}/PrismLauncher-${pkgver}/build"
  DESTDIR="${pkgdir}" cmake --install .
  mkdir -p "${pkgdir}/usr/share/doc/prismlauncher"
  cp -v "${srcdir}/copyright" "${pkgdir}/usr/share/doc/prismlauncher/copyright"
}
# vim: set sw=2 expandtab: