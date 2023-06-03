pkgname=moeOS
pkgver=r46.c085302
pkgrel=1
pkgdesc="moeOS Configurations"
arch=('x86_64')
url="https://github.com/Kimiblock/moeOS.config"
License=('none')
install=${pkgname}.install
backup=('etc/moeOS-clash-meta/subscribe.conf')
depends=(
    'easyeffects'
    'gst-plugin-pipewire'
    'pipewire'
    'lib32-pipewire'
    'pipewire-alsa'
    'pipewire-audio'
    'pipewire-jack'
    'pipewire-pulse'
    'wireplumber'
    'inter-font'
    'power-profiles-daemon'
    'networkmanager'
    'plymouth'
    'mpv'
    'noto-fonts'
    'noto-fonts-cjk'
    'noto-fonts-emoji'
    'kernel-modules-hook'
    'linux-firmware'
    'base-devel'
    'paru'
    'kdenlive'
    'icalingua++'
    'sox'
    'iio-sensor-proxy'
    'sbupdate-git'
    'marktext-git'
    'clash-meta'
    'clash-verge'
    'timeshift'
    'cups'
    'avahi'
    'lsb-release'
    'obs-gstreamer'
    'mediainfo'
    'ibus'
    'ibus-pinyin'
    'ibus-rime'
    'ibus-typing-booster'
    'rtaudio'
    'linux-firmware-whence'
    'sof-firmware'
    'opencv'
    'movit')
makedepends=(
    'git')
optdepends=('nerd-fonts-sf-mono' 'uutils-coreutils')
source=('git+https://github.com/Kimiblock/moeOS.config.git' 'git+https://github.com/ShmilyHTT/PingFang.git')
sha256sums=('SKIP' 'SKIP')


function package(){
    _info "Initialize VA-API installation"
    if [[ `lspci | grep VGA` =~ Intel ]]; then
        _info "Pending intel-media-driver"
        depends+=("intel-media-driver" "libva-utils" 'libva' 'gstreamer-vaapi' 'gst-plugin-va')
    elif [[ `lspci | grep VGA` =~ "Advanced Micro Devices" ]]; then
        _info "Pending libva-mesa-driver"
        depends+=('libva-mesa-driver' "libva-utils" 'libva' 'gstreamer-vaapi' 'gst-plugin-va')
    fi
    for dir in /usr/share/libalpm/hooks /usr/share/fonts/moeOS-pingfang; do
        _info Creating directory ${dir}
        mkdir -p "${pkgdir}${dir}"
    done
    cp -r "${srcdir}"/moeOS.config/usr "${pkgdir}"/
    cp -r "${srcdir}"/moeOS.config/etc "${pkgdir}"/
    cp -r "${srcdir}"/PingFang/*.ttf "${pkgdir}"/usr/share/fonts/moeOS-pingfang
    for file in lsb-release os-release sbupdate.conf mkinitcpio.conf mkinitcpio.d; do
        mv "${pkgdir}"/etc/${file} "${pkgdir}/usr/share/moeOS-Docs"
    done
    mv "${pkgdir}/etc/systemd/sleep.conf" "${pkgdir}/usr/share/moeOS-Docs"
    _info 'Creating Hook(s)'
    echo '''[Trigger]
Operation = Install
Operation = Upgrade
Type = Path
Target = etc/os-release
Target = etc/lsb-release
Target = usr/lib/os-release
Target = etc/tlp.conf
Target = etc/Packagekit.conf

[Action]
When = PostTransaction
Exec = /usr/bin/moeRelease
Depends = moeOS
Description = Restoring moeOS Release
''' >"${pkgdir}"/usr/share/libalpm/hooks/moeOS-OS-replace.hook
    chmod 755 -R "${pkgdir}"/usr/bin
}

function _info() {
	if [ -f /usr/bin/pamac ]; then
		echo "  ==> [Info]: $@"
	else
		all_off="$(tput sgr0)"
		bold="${all_off}$(tput bold)"
		blue="${bold}$(tput setaf 4)"
		yellow="${bold}$(tput setaf 3)"
		printf "${blue}==>${yellow} [Info]:${bold} $1${all_off}\n"
    fi
}
