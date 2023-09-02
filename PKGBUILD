# Maintainer: Kimiblock Moe
pkgname=("moeOS-git" "lsb-release-moe" "nvidia-prime-moe")
pkgver=r213.12e05c3
epoch=1
pkgrel=1
pkgdesc="moeOS Configurations"
arch=('x86_64')
url="https://github.com/Kimiblock/moeOS.config"
license=('MIT')
replaces=()
conflicts=()
provides=()
backup=('etc/moeOS-clash-meta/subscribe.conf' 'etc/moeOS-clash-meta/merge.yaml')
groups=("moeOS")
makedepends=(
	"git"
	"make"
	"paru")
optdepends=()
source=(
	"git+https://github.com/LinuxStandardBase/lsb-samples.git"
	"git+https://github.com/Kimiblock/moeOS.config.git")
sha256sums=(
	"SKIP"
	"SKIP")

function pkgver(){
	cd moeOS.config
	printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
}

function build(){
	buildLsb
}

function buildLsb(){
	cd lsb-samples/lsb_release/src
	make
}

function package_nvidia-prime-moe(){
	pkgdesc="Replacement of the official nvidia-prime. Supports moeOS's powersave features."
	replaces=("nvidia-prime")
	conflicts=("nvidia-prime")
	provides=("nvidia-prime")
	install -Dm755 "${srcdir}/moeOS.config/usr/share/moeOS-Docs/bin/prime-run" -t "${pkgdir}/usr/bin"
}

function package_lsb-release-moe(){
	pkgdesc="Replacement of the official lsb-release."
	replaces=("lsb-release")
	conflicts=("lsb-release")
	provides=("lsb-release")
	_info "Preparing lsb-release..."
	cd lsb-samples/lsb_release/src
	install -Dm644 lsb_release.1.gz -t "$pkgdir/usr/share/man/man1"
	install -Dm755 lsb_release -t "$pkgdir/usr/bin"
	install -Dm644 "${srcdir}/moeOS.config/usr/share/moeOS-Docs/lsb-release" -t "${pkgdir}/etc"
}

function package_moeOS-git(){
	depends=(
		"nvidia-prime-moe"
		"lsb-release-moe"
		'xdg-desktop-portal-gnome'
		'xdg-desktop-portal'
		'bc'
		'bat'
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
		'noto-fonts-cjk'
		'ttf-twemoji'
		'kernel-modules-hook'
		'linux-firmware'
		'base-devel'
		'paru'
		'kdenlive'
		'sox'
		'iio-sensor-proxy'
		'sbctl'
		'clash-meta'
		'clash-verge'
		'timeshift'
		'cups'
		'avahi'
		'obs-gstreamer'
		'mediainfo'
		'rtaudio'
		'linux-firmware-whence'
		'sof-firmware'
		'opencv'
		"movit"
		"go-yq"
		"fcitx5"
		"fcitx5-configtool"
		"fcitx5-qt"
		'fcitx5-pinyin-moegirl-rime'
		'librime-data'
		'gst-plugin-va'
		'nftables'
		'iptables-nft'
		'yt-dlp'
		'dhclient'
		# Default Librewolf browser
		"librewolf"
		"diffutils"
		"gdm-settings"
		"rime-ice"
		"flatpak"
		"nerd-fonts-sf-mono"
		"librewolf-ublock-origin"
		"librewolf-extension-dark-reader"
		"librewolf-extension-bitwarden"
		"librewolf-extension-violentmonkey-bin"
		"librewolf-extension-sponsorblock-bin"
		"fcitx5-gtk"
		"rime-minecraft-dict-git"
		"firefox-gnome-theme"
	)
	install=moeOS-git.install
	createDir
	copyFiles
	for file in os-release sbupdate.conf mkinitcpio.conf mkinitcpio.d; do
		mv "${pkgdir}"/etc/${file} "${pkgdir}/usr/share/moeOS-Docs"
	done
	configureGraphics
	dhcp
	gnomeShellRt
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
	echo "BUILD_ID=${pkgver}@$(date +%d/%m/%Y)" >>"${pkgdir}/usr/share/moeOS-Docs/os-release"
}

function initVkIcd(){
	if [[ "${vgaDev}" =~ Intel ]] && [[ "${vgaDev}" =~ "Advanced Micro Devices" ]]; then
		_info "This script currently can't handle Intel + AMD card"
	else
		if [[ "${vgaDev}" =~ Intel ]]; then
			_info "Configuring Vulkan to use Intel GPU"
			echo 'VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/intel_icd.x86_64.json:/usr/share/vulkan/icd.d/intel_icd.i686.json' >>"${pkgdir}/etc/environment.d/moeOS-Nvidia-RTD3.conf"
		elif [[ "${vgaDev}" =~ "Advanced Micro Devices" ]]; then
			_info "Configuring Vulkan to use AMD GPU"
			echo 'VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/radeon_icd.i686.json:/usr/share/vulkan/icd.d/radeon_icd.x86_64.json' >>"${pkgdir}/etc/environment.d/moeOS-Nvidia-RTD3.conf"
		else
			_info "Failed to configure Vulkan ICD filenames"
		fi
	fi
}

function configureGraphics(){
	_info "Configuring graphics card..."
	vgaDev=$(lspci | grep VGA)
	if [[ "${vgaDev}" =~ Intel ]]; then
		suspendNvidia
		_info "Adding Intel driver and Power Profiles Daemon as dependencies"
		depends+=("power-profiles-daemon" "intel-media-driver" "libva-utils" 'libva' 'gstreamer-vaapi' 'vulkan-intel' )
	elif [[ "${vgaDev}" =~ "Advanced Micro Devices" ]]; then
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

function applyEnv(){
	cp "${srcdir}/moeOS.config/usr/share/moeOS-Docs/Environments.d/$@.conf" "${pkgdir}/etc/environment.d/$@.conf"
}

function radvVA(){
	_info "Enabling vulkan video decode..."
	applyEnv amdVulkanDecode
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
	chmod -R 644 "${pkgdir}/usr/lib/udev/rules.d"
	chmod -R 755 "${pkgdir}"/usr/bin
}

function suspendNvidia(){
	if [[ $(lspci -k | grep -A 2 -E "(VGA|3D)") =~ (NVIDIA|nvidia|GeForce) ]]; then
		depends+=('nvidia-utils' 'nvidia-dkms' 'lib32-nvidia-utils')
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
	_info "Your flatpak installation has been configured to not install any Nvidia runtime"
	_info "If you need to run an app on discreate graphics card, install it natively and use prime-run"
	initVkIcd
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
