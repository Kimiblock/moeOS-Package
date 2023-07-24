# Maintainer: Kimiblock Moe <pn3535@icloud.com>
pkgname=moeOS
pkgver=r46.c085302
pkgrel=1
pkgdesc="moeOS Configurations"
arch=('x86_64')
url="https://github.com/Kimiblock/moeOS.config"
license=('MIT')
install=${pkgname}.install
conflicts=("lsb-release" "nvidia-prime")
provides=("lsb-release" nvidia-prime)
backup=('etc/moeOS-clash-meta/subscribe.conf' 'etc/moeOS-clash-meta/merge.yaml')
depends=(
	'xdg-desktop-portal-gnome'
	'xdg-desktop-portal'
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
	'sox'
	'iio-sensor-proxy'
	'sbupdate-git'
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
	#'fcitx5'
	#'fcitx5-configtool'
	#'fcitx5-gtk'
	#'fcitx5-qt'
	#'fcitx5-pinyin-zhwiki'
	#'fcitx5-chinese-addons'
	#'fcitx5-pinyin-moegirl'
	'ibus'
	'ibus-rime'
	'rime-pinyin-zhwiki'
	'rime-luna-pinyin'
	'rime-terra-pinyin'
	'fcitx5-pinyin-moegirl-rime'
	'librime-data'
	'sddm'
	'gst-plugin-va'
	'nftables'
	'iptables-nft'
	'yt-dlp'
	'dhclient'
	# Default Librewolf browser
	"librewolf")
makedepends=("git" "make")
optdepends=('nerd-fonts-sf-mono' 'uutils-coreutils' 'ffmpeg-normalize' "librewolf-ublock-origin" "librewolf-extension-dark-reader" "librewolf-extension-bitwarden" "librewolf-extension-violentmonkey-bin" "librewolf-extension-sponsorblock-bin")
source=("git+https://github.com/LinuxStandardBase/lsb-samples.git" 'git+https://github.com/Kimiblock/moeOS.config.git')
sha256sums=('SKIP' 'SKIP')

function build(){
	cd lsb-samples/lsb_release/src
	make
}

function package(){
	createDir
	copyFiles
	for file in os-release sbupdate.conf mkinitcpio.conf mkinitcpio.d; do
		mv "${pkgdir}"/etc/${file} "${pkgdir}/usr/share/moeOS-Docs"
	done
	configureGraphics
	dhcp
	gnomeShellRt
	genLsb
	genBuildId
	fixPermission
}

function copyFiles(){
	_info "Moving Files..."
	cp -r "${srcdir}"/moeOS.config/usr "${pkgdir}"
	cp -r "${srcdir}"/moeOS.config/etc "${pkgdir}"
}

function createDir(){
	for dir in "/usr/lib/udev/rules.d" "/usr/lib/modprobe.d"; do
		_info "Creating directory ${dir}"
		mkdir -p "${pkgdir}${dir}"
	done
}

function genBuildId(){
	_info "Generating Build ID"
	echo "BUILD_ID=$(date +%Y-%m-%d)" >>"${pkgdir}/usr/share/moeOS-Docs/os-release"
}

function configureGraphics(){
	_info "Configuring graphics card..."
	if [[ `lspci | grep VGA` =~ Intel ]]; then
		suspendNvidia
		_info "Adding Intel driver and Power Profiles Daemon as dependencies"
		depends+=("power-profiles-daemon" "intel-media-driver" "libva-utils" 'libva' 'gstreamer-vaapi' 'vulkan-intel' )
	elif [[ `lspci | grep VGA` =~ "Advanced Micro Devices" ]]; then
		radvVA
		suspendNvidia
		_info "Adding libva-mesa-driver as a dependency"
		if [[ $(cpupower frequency-info | grep driver) =~ epp ]]; then
			_info
			depends+=("power-profiles-daemon")
		fi
		depends+=('libva-mesa-driver' "libva-utils" 'libva' 'gstreamer-vaapi' 'vulkan-radeon')
	fi
}

function radvVA(){
	_info "Enabling vulkan video decode..."
	echo "RADV_VIDEO_DECODE=1" >>"${pkgdir}/etc/environment.d/moeOS.conf"
}

function genLsb(){
	_info "Preparing lsb-release..."
	cd lsb-samples/lsb_release/src
	install -Dm644 lsb_release.1.gz -t "$pkgdir/usr/share/man/man1"
	install -Dm755 lsb_release -t "$pkgdir/usr/bin"
	install -Dm644 "${srcdir}/moeOS.config/etc/lsb-release" -t "${pkgdir}/etc"
}

function gnomeShellRt(){
	if [[ $(pacman -Q) =~ gnome-shell-performance ]] & [[ $(pacman -Q) =~ mutter-performance ]]; then
		_info "Enabling rt schedulers for mutter..."
		cp "${pkgdir}/usr/share/moeOS-Docs/mutter-performance.conf" "${pkgdir}/etc/dconf/db/local.d/00-moeOS-HiDPI"
	fi
}

function dhcp(){
	echo "[main]" >>"${pkgdir}/etc/NetworkManager/conf.d/moeOS-dhcp-client.conf"
	echo "dhcp=dhclient" >>"${pkgdir}/etc/NetworkManager/conf.d/moeOS-dhcp-client.conf"
}

function fixPermission(){
	chmod -R 700 "${pkgdir}/etc/moeOS-clash-meta"
	chmod -R 644 "${pkgdir}/etc/udev/rules.d"
	chmod -R 755 "${pkgdir}"/usr/bin
}

function suspendNvidia(){
	if [[ $(lspci -k | grep -A 2 -E "(VGA|3D)") =~ (NVIDIA|nvidia|GeForce) ]]; then
		depends+=('nvidia-prime' 'nvidia-utils' 'nvidia-dkms' 'lib32-nvidia-utils')
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
