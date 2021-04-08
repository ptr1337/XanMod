#_                   _ _ _  _ _____ _  _
#| | _______   ____ _| | | || |___  | || |
#| |/ / _ \ \ / / _` | | | || |_ / /| || |_
#|   <  __/\ V / (_| | | |__   _/ / |__   _|
#|_|\_\___| \_/ \__,_|_|_|  |_|/_/     |_|

#Maintainer: kevall474 <kevall474@tuta.io> <https://github.com/kevall474>
#Credits: Jan Alexander Steffens (heftig) <heftig@archlinux.org> ---> For the base PKGBUILD
#Credits: Andreas Radke <andyrtr@archlinux.org> ---> For the base PKGBUILD
#Credits: Linus Torvalds ---> For the linux kernel
#Credits: Joan Figueras <ffigue at gmail dot com> ---> For the base PKFBUILD
#Credits: Piotr Gorski <lucjan.lucjanov@gmail.com> <https://github.com/sirlucjan/kernel-patches> ---> For the patches and the base pkgbuild
#Credits: Tk-Glitch <https://github.com/Tk-Glitch> ---> For some patches. base PKGBUILD and prepare script
#Credits: Alexandre Frade <kernel@xanmod.org> <https://github.com/xanmod> <https://xanmod.org/> ---> For the XanMod patched kernel
#Credits: Hamad Al Marri <https://github.com/hamadmarri/cachy-sched> ---> For CacULE CPU Scheduler patch

################# CPU Scheduler #################

#Set CPU Scheduler
#Set '1' for CacULE CPU Scheduler
#Set '2' for CacULE-RDB CPU Scheduler
#Leave empty for no CPU Scheduler
#Default is empty. It will build with no cpu scheduler. To build with cpu shceduler just use : env _cpu_sched=(1 or 2) makepkg -s
if [ -z ${_cpu_sched+x} ]; then
  _cpu_sched=
fi

################################# Arch ################################

ARCH=x86

################################# CC/CXX/HOSTCC/HOSTCXX ################################

#Set compiler to build the kernel
#Set '1' to build with GCC
#Set '2' to build with CLANG and LLVM
#Default is empty. It will build with GCC. To build with different compiler just use : env _compiler=(1 or 2) makepkg -s
if [ -z ${_compiler+x} ]; then
  _compiler=
fi

if [[ "$_compiler" = "1" ]]; then
  CC=gcc
  CXX=g++
  HOSTCC=gcc
  HOSTCXX=g++
  buildwith="build with GCC"
elif [[ "$_compiler" = "2" ]]; then
  CC=clang
  CXX=clang++
  HOSTCC=clang
  HOSTCXX=clang++
  buildwith="build with CLANG/LLVM"
else
  _compiler=1
  CC=gcc
  CXX=g++
  HOSTCC=gcc
  HOSTCXX=g++
  buildwith="build with GCC"
fi

###################################################################################

# This section set the pkgbase based on the cpu scheduler. So user can build different package based on the cpu schduler for testing.
if [[ $_cpu_sched = "1" ]]; then
  if [[ "$_compiler" = "1" ]]; then
    pkgbase=linux-xanmod-cacule-gcc
  elif [[ "$_compiler" = "2" ]]; then
    pkgbase=linux-xanmod-cacule-clang
  fi
elif [[ $_cpu_sched = "2" ]]; then
  if [[ "$_compiler" = "1" ]]; then
    pkgbase=linux-xanmod-cacule-rdb-gcc
  elif [[ "$_compiler" = "2" ]]; then
    pkgbase=linux-xanmod-cacule-rdb-clang
  fi
else
  if [[ "$_compiler" = "1" ]]; then
    pkgbase=linux-xanmod-kernel-gcc
  elif [[ "$_compiler" = "2" ]]; then
    pkgbase=linux-xanmod-kernel-clang
  fi
fi
pkgname=("$pkgbase" "$pkgbase-headers")
for _p in "${pkgname[@]}"; do
  eval "package_$_p() {
    $(declare -f "_package${_p#$pkgbase}")
    _package${_p#$pkgbase}
  }"
done
# This section set the version for xanmod version. Sometimes xanmod-cacule is behind the main xanmod patch
if [[ $_cpu_sched = "1" ]] || [[ $_cpu_sched = "2" ]]; then
  pkgver=5.11.12
  versiontag=5.11.12-xanmod1-cacule
else
  pkgver=5.11.12
  versiontag=5.11.12-xanmod1
fi
major=5.11
pkgrel=1
arch=(x86_64)
url="https://xanmod.org/"
license=(GPL-2.0)
makedepends=("bison" "flex" "valgrind" "git" "cmake" "make" "extra-cmake-modules" "libelf" "elfutils"
             "python" "python-appdirs" "python-mako" "python-evdev" "python-sphinx_rtd_theme" "python-graphviz" "python-sphinx"
             "clang" "lib32-clang" "bc" "gcc" "gcc-libs" "lib32-gcc-libs" "glibc" "lib32-glibc" "pahole" "patch" "gtk3" "llvm" "lib32-llvm"
             "llvm-libs" "lib32-llvm-libs" "lld" "kmod" "libmikmod" "lib32-libmikmod" "xmlto" "xmltoman" "graphviz" "imagemagick" "imagemagick-doc"
             "rsync" "cpio" "inetutils" "gzip" "zstd" "xz")
patchsource=https://raw.githubusercontent.com/kevall474/kernel-patches/main/$major
source=("https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/linux-$major.tar.xz"
        "$patchsource/misc/0005-Disable-CPU_FREQ_GOV_SCHEDUTIL.patch"
        "$patchsource/zen-patches/0001-ZEN-Add-VHBA-driver.patch"
        "$patchsource/bfq-patches/0001-bfq-patches.patch"
        "$patchsource/aufs-patches/0001-aufs-20210308.patch"
        "$patchsource/btrfs-patches/0001-btrfs-patches.patch"
        "$patchsource/loopback-patches/0001-v4l2loopback-5.11-merge-v0.12.5.patch"
        "$patchsource/mm-patches/0001-mm-patches.patch"
        "$patchsource/spadfs-patches/0001-spadfs-5.11-merge-v1.0.13.patch"
        "$patchsource/zswap-patches/0001-zswap-patches.patch"
        "$patchsource/pf-patches/0001-pf-patches.patch"
        "$patchsource/arch-patches/0002-HID-quirks-Add-Apple-Magic-Trackpad-2-to-hid_have_sp.patch"
        "$patchsource/ksm-patches/0001-ksm-patches.patch"
        "$patchsource/lqx-patches/0001-lqx-patches.patch"
        "$patchsource/wine-patches/0007-v5.11-winesync.patch"
        "$patchsource/misc/vm.max_map_count.patch")
md5sums=('d2985a3f16ef1ea3405c04c406e29dcc'
         'f99b82d6f424d1a729a9b8c5a1be2b84'
         '6042ba527a2379f858fbc099caa7dc70'
         '379a49cafda4a5448b7a873722eb1a96'
         '76d68d069b5947349933c6baba07cf2f'
         '766941966c8db36612b0c04227429e51'
         '0ab93e8e3437a5093520c10cca741531'
         'cc70bf905a1237a41b11338b2eba4a8b'
         '49b4c1a2098d0f0584eb8d0eda2a60c9'
         '64e629e48f15cc0ebddfee366386f17a'
         'f7e7e6cddb72ad8ae741849dddb6e6fa'
         'e7ef63d6e6fb1ed9d8c2b4d3f65de86c'
         '9c37d7643710ffa49552cc43b96980ed'
         '4f5b46d26699b4f4e7d7bc153979d3e0'
         'ab8f21e210aec26c7825033d57433e33'
         '27e6001bacfcfca1c161bf6ef946a79b'
         '8e71f0c43157654c4105224d89cc6709'
         '88c64c712b9e38c312006f2c3eebcf70')
#zenify workarround
if [[ $_cpu_sched != "1" ]] && [[ $_cpu_sched != "2" ]]; then
  source+=("$patchsource/misc/zenify.patch")
  md5sums+=("8e71f0c43157654c4105224d89cc6709")  #zenify.patch
fi
if [[ $_cpu_sched = "1" ]] || [[ $_cpu_sched = "2" ]]; then
  source+=("https://github.com/xanmod/linux/releases/download/$versiontag/patch-$versiontag.xz")
  md5sums+=("211e07ada1f7653bc28208c3cf5cb49b")  #patch-5.11.12-xanmod1-cacule.xz
else
  source+=("https://github.com/xanmod/linux/releases/download/$versiontag/patch-$versiontag.xz")
  md5sums+=("03f7a5b8c0b87d52cb5e21d221e48648")  #patch-5.11.12-xanmod1.xz
fi

export KBUILD_BUILD_HOST=archlinux
export KBUILD_BUILD_USER=$pkgbase
export KBUILD_BUILD_TIMESTAMP="$(date -Ru${SOURCE_DATE_EPOCH:+d @$SOURCE_DATE_EPOCH})"

prepare(){

  cd linux-$major

  # hacky work around for xz not getting extracted
  # https://bbs.archlinux.org/viewtopic.php?id=265115
  if [[ ! -f "$srcdir/patch-$versiontag" ]]; then
    unlink "$srcdir/patch-$versiontag.xz"
    xz -dc "$startdir/patch-$versiontag.xz" > "$srcdir/patch-$versiontag"
  fi

  # Apply Xanmod patch
  patch -Np1 -i ../patch-$versiontag

  # Apply any patch
  local src
  for src in "${source[@]}"; do
    src="${src%%::*}"
    src="${src##*/}"
    [[ $src = *.patch ]] || continue
    msg2 "Applying patch $src..."
    patch -Np1 < "../$src"
  done

  # Customize the kernel
  source "${startdir}"/prepare

  configure

  cpu_arch

  # Setting localversion
  msg2 "Setting localversion..."
  scripts/setlocalversion --save-scmversion
  echo "-${pkgbase}" > localversion

  # Config
  if [[ "$_compiler" = "1" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} olddefconfig
  elif [[ "$_compiler" = "2" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} LLVM=1 LLVM_IAS=1 HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} olddefconfig
  fi

  make -s kernelrelease > version
  msg2 "Prepared $pkgbase version $(<version)"
}

build(){

  cd linux-$major

  # make -j$(nproc) all
  msg2 "make -j$(nproc) all..."
  if [[ "$_compiler" = "1" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} -j$(nproc) all
  elif [[ "$_compiler" = "2" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} LLVM=1 LLVM_IAS=1 HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} -j$(nproc) all
  fi
}

_package(){
  pkgdesc="The Linux kernel and modules with Xanmod patches"
  depends=("coreutils" "kmod" "initramfs" "mkinitcpio")
  optdepends=("linux-firmware: firmware images needed for some devices"
              "crda: to set the correct wireless channels of your country")
  provides=("VIRTUALBOX-GUEST-MODULES" "WIREGUARD-MODULE")

  cd linux-$major

  local kernver="$(<version)"
  local modulesdir="${pkgdir}"/usr/lib/modules/${kernver}

  msg2 "Installing boot image..."
  # systemd expects to find the kernel here to allow hibernation
  # https://github.com/systemd/systemd/commit/edda44605f06a41fb86b7ab8128dcf99161d2344
  install -Dm644 arch/${ARCH}/boot/bzImage "$modulesdir/vmlinuz"

  # Used by mkinitcpio to name the kernel
  echo "$pkgbase" | install -Dm644 /dev/stdin "$modulesdir/pkgbase"

  msg2 "Installing modules..."
  if [[ "$_compiler" = "1" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} INSTALL_MOD_PATH="${pkgdir}"/usr INSTALL_MOD_STRIP=1 -j$(nproc) modules_install
  elif [[ "$_compiler" = "2" ]]; then
    make ARCH=${ARCH} CC=${CC} CXX=${CXX} LLVM=1 LLVM_IAS=1 HOSTCC=${HOSTCC} HOSTCXX=${HOSTCXX} INSTALL_MOD_PATH="${pkgdir}"/usr INSTALL_MOD_STRIP=1 -j$(nproc) modules_install
  fi

  # remove build and source links
  msg2 "Remove build dir and source dir..."
  rm -rf "$modulesdir"/{source,build}
}

_package-headers(){
  pkgdesc="Headers and scripts for building modules for the $pkgbase package"
  depends=("${pkgbase}" "pahole")

  cd linux-$major

  local builddir="$pkgdir"/usr/lib/modules/"$(<version)"/build

  msg2 "Installing build files..."
  install -Dt "$builddir" -m644 .config Makefile Module.symvers System.map localversion version vmlinux
  install -Dt "$builddir/kernel" -m644 kernel/Makefile
  install -Dt "$builddir/arch/x86" -m644 arch/x86/Makefile
  cp -t "$builddir" -a scripts

  # add objtool for external module building and enabled VALIDATION_STACK option
  install -Dt "$builddir/tools/objtool" tools/objtool/objtool

  # add xfs and shmem for aufs building
  mkdir -p "$builddir"/{fs/xfs,mm}

  msg2 "Installing headers..."
  cp -t "$builddir" -a include
  cp -t "$builddir/arch/x86" -a arch/x86/include
  install -Dt "$builddir/arch/x86/kernel" -m644 arch/x86/kernel/asm-offsets.s

  install -Dt "$builddir/drivers/md" -m644 drivers/md/*.h
  install -Dt "$builddir/net/mac80211" -m644 net/mac80211/*.h

  # http://bugs.archlinux.org/task/13146
  install -Dt "$builddir/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # http://bugs.archlinux.org/task/20402
  install -Dt "$builddir/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "$builddir/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "$builddir/drivers/media/tuners" -m644 drivers/media/tuners/*.h

  msg2 "Installing KConfig files..."
  find . -name 'Kconfig*' -exec install -Dm644 {} "$builddir/{}" \;

  msg2 "Removing unneeded architectures..."
  local arch
  for arch in "$builddir"/arch/*/; do
    [[ $arch = */x86/ ]] && continue
    msg2 "Removing $(basename "$arch")"
    rm -r "$arch"
  done

  msg2 "Removing documentation..."
  rm -r "$builddir/Documentation"

  msg2 "Removing broken symlinks..."
  find -L "$builddir" -type l -printf 'Removing %P\n' -delete

  msg2 "Removing loose objects..."
  find "$builddir" -type f -name '*.o' -printf 'Removing %P\n' -delete

  msg2 "Stripping build tools..."
  local file
  while read -rd '' file; do
    case "$(file -bi "$file")" in
      application/x-sharedlib\;*)      # Libraries (.so)
        strip -v $STRIP_SHARED "$file" ;;
      application/x-archive\;*)        # Libraries (.a)
        strip -v $STRIP_STATIC "$file" ;;
      application/x-executable\;*)     # Binaries
        strip -v $STRIP_BINARIES "$file" ;;
      application/x-pie-executable\;*) # Relocatable binaries
        strip -v $STRIP_SHARED "$file" ;;
    esac
  done < <(find "$builddir" -type f -perm -u+x ! -name vmlinux -print0)

  msg2 "Stripping vmlinux..."
  strip -v $STRIP_STATIC "$builddir/vmlinux"

  msg2 "Adding symlink..."
  mkdir -p "$pkgdir/usr/src"
  ln -sr "$builddir" "$pkgdir/usr/src/$pkgbase"
}
