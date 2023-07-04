pkgname=moeOS
pkgver=r46.c085302
pkgrel=1
pkgdesc="moeOS Configurations"
arch=('x86_64')
url="https://github.com/Kimiblock/moeOS.config"
License=('none')
install=${pkgname}.install
backup=('etc/moeOS-clash-meta/subscribe.conf' 'etc/moeOS-clash-meta/merge.yaml')
depends=(
	'bc'
	'glxinfo'
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
    'rtaudio'
    'linux-firmware-whence'
    'sof-firmware'
    'opencv'
    'movit'
    'go-yq'
    'fcitx5'
    'fcitx5-configtool'
    'fcitx5-gtk'
    'fcitx5-qt'
    'fcitx5-pinyin-zhwiki'
    'fcitx5-chinese-addons'
    'fcitx5-pinyin-moegirl'
    #'ibus'
    #'ibus-rime'
    'sddm'
    'gst-plugin-va'
    'nftables'
    'iptables-nft')
makedepends=(
    'git')
optdepends=('nerd-fonts-sf-mono' 'uutils-coreutils' 'ffmpeg-normalize')
source=('git+https://github.com/Kimiblock/moeOS.config.git' 'git+https://github.com/ShmilyHTT/PingFang.git')
sha256sums=('SKIP' 'SKIP')


function package(){
    for dir in "/usr/share/libalpm/hooks" "/usr/share/fonts/moeOS-pingfang" "/usr/lib/udev/rules.d" "/usr/lib/modprobe.d" "/usr/lib/tmpfiles.d"; do
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
    
    _info "Initializing vendor-specfic installation..."
    if [[ `lspci | grep VGA` =~ Intel ]]; then
    	suspendNvidia
        _info "Adding Intel driver and Power Profiles Daemon"
        depends+=("power-profiles-daemon" "intel-media-driver" "libva-utils" 'libva' 'gstreamer-vaapi')
    elif [[ `lspci | grep VGA` =~ "Advanced Micro Devices" ]]; then
    	suspendNvidia
        _info "Pending libva-mesa-driver"
        if [[ $(cpupower frequency-info | grep driver) =~ epp ]]; then
        	_info 
        	depends+=("power-profiles-daemon")
        else
        	depends+=("tlp")
        fi
        depends+=('libva-mesa-driver' "libva-utils" 'libva' 'gstreamer-vaapi')
    fi
    #_info "Writing tmpfiles..."
    #echo "d	/etc/moeOS-clash-meta 0700 root root" >"${pkgdir}/usr/lib/tmpfiles.d/moeOS-clash-meta.conf"
    _info "Generating Build ID"
    echo "BUILD_ID=$(date +%Y-%m-%d)" >>"${pkgdir}/usr/share/moeOS-Docs/os-release"
    echo "RADV_VIDEO_DECODE=1" >>"${pkgdir}/etc/environment.d/moeOS.conf"
    chmod -R 700 "${pkgdir}/etc/moeOS-clash-meta"
}

function suspendNvidia(){
	if [[ $(lspci -k | grep -A 2 -E "(VGA|3D)") =~ (NVIDIA|nvidia|GeForce) ]]; then
		depends+=('nvidia-prime' 'nvidia-utils' 'nvidia-dkms')
		_info "Fixing RTD3 power management"
		echo "__EGL_VENDOR_LIBRARY_FILENAMES=/usr/share/glvnd/egl_vendor.d/50_mesa.json" >>"${pkgdir}/etc/environment.d/moeOS-Nvidia-RTD3.conf"
		echo '''# Enable runtime PM for NVIDIA VGA/3D controller devices on driver bind
ACTION=="bind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="auto"
ACTION=="bind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030200", TEST=="power/control", ATTR{power/control}="auto"

# Disable runtime PM for NVIDIA VGA/3D controller devices on driver unbind
ACTION=="unbind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="on"
ACTION=="unbind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030200", TEST=="power/control", ATTR{power/control}="on"''' >"${pkgdir}/usr/lib/udev/rules.d/80-nvidia-pm.rules"
	echo 'options nvidia "NVreg_DynamicPowerManagement=0x02"' >"${pkgdir}/usr/lib/modprobe.d/nvidia-pm.conf"
	fi
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
