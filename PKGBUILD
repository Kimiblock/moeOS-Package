# Maintainer: Kimiblock Moe
pkgname=("moeOS-git" "lsb-release-moe" "nvidia-prime-moe" "moe-multimedia-meta" "moe-fonts-meta" "moe-input-config" "moe-desktop-meta")
pkgver=r499.5a9bf93
epoch=1
pkgrel=1
pkgdesc="moeOS Configurations"
arch=('x86_64')
url="https://github.com/Kimiblock/moeOS.config"
license=('MIT')
replaces=()
conflicts=("snapd")
provides=()
groups=("moeOS")
makedepends=(
	"git"
	"make")
optdepends=()
source=(
	"git+https://github.com/LinuxStandardBase/lsb-samples.git"
	"git+https://github.com/Kimiblock/moeOS.config.git"
	"git+https://github.com/Kimiblock/moeOS-pinyin.git")
sha256sums=(
	"SKIP"
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

function package_moe-rime-essay(){
	replaces=("rime-essay-moe")
	conflicts=("rime-essay-moe")
	pkgdesc="moeOS Rime essay, optimized for simplified Chinese."
	_info "Preparing Rime essay..."
	mkdir -p "${pkgdir}/usr/share/rime-data"
	cp "${srcdir}/rime-essay-simp/essay-zh-hans.txt" "${pkgdir}/usr/share/rime-data/moe-essay.txt"
	chmod -R 755 "${pkgdir}/usr/share/rime-data"
}

function package_moe-multimedia-meta(){
	depends=(
		'gst-plugin-pipewire'
		'pipewire'
		'lib32-pipewire'
		'pipewire-alsa'
		'pipewire-audio'
		'pipewire-jack'
		'pipewire-pulse'
		'wireplumber'
		'easyeffects'
		'mpv'
		'obs-gstreamer'
		'mediainfo'
		'rtaudio'
		'sof-firmware'
		'gst-plugin-va'
		'yt-dlp'
	)
	conflicts=("moe-mpv-modern")
}

function package_moe-fonts-meta(){
	depends=(
		'noto-fonts-cjk'
		'ttf-twemoji'
		"inter-font"
		"ttf-roboto-mono"
	)
}

function getLatestRel(){
	if [[ $2 = github ]]; then
		_rawVersion=$(curl -s https://api.github.com/repos/"$1"/releases/latest | jq .tag_name)
		_release=$(echo "${_rawVersion}" | cut -c 2-$(expr ${#_rawVersion} - 1))
	else
		return 1
	fi
}

function package_moe-input-config(){
	depends=(
		"librime-data"
		"fcitx5-pinyin-moegirl-rime"
		"rime-pinyin-zhwiki"
		"ibus-rime"
		"gnome-shell-extension-kimpanel-git"
	)
	conflicts=(
		"moe-input-meta"
		"fcitx5-gtk"
		"fcitx5"
		"fcitx5-configtool"
		"fcitx5-qt"
		"fcitx5-rime"
	)
	replaces=("moe-input-meta")
	cd "${srcdir}/moeOS-pinyin"
	git submodule update --init --depth 1 --remote
	mkdir -p "${pkgdir}/usr/share"
	cp "${srcdir}/moeOS-pinyin/rime-data" -r "${pkgdir}/usr/share"
	install -Dm644 "${srcdir}/moeOS-pinyin/default.yaml" "${pkgdir}/usr/share/moeOS-Docs/ibus-rime.conf.d/default.yaml"
	rm -r "${pkgdir}/usr/share/rime-data/others/rime-ice/others"
	rm -r "${pkgdir}/usr/share/rime-data/others/rime-setting/fonts"
	for dir in $(ls "${pkgdir}/usr/share/rime-data/others"); do
		rm -rf "${pkgdir}/usr/share/rime-data/others/${dir}/.git"
	done
	chmod -R 755 "${pkgdir}/usr/share/rime-data"
}

function package_moe-desktop-meta(){
	depends=(
		"clash-geoip"
		"v2ray-domain-list-community"
		"game-devices-udev"
		"gnome-shell"
		"libdecor"
		"ffmpegthumbnailer"
		"mutter-performance>45.1"
		"clapper"
		"libreoffice-fresh"
		"qadwaitaplatform-qt6-git"
		"qadwaitaplatform-qt5-git"
		#"plasma-wayland-session"
		"adw-gtk-theme"
		"gnome-shell-extension-appindicator"
		"tela-circle-icon-theme-pink-git"
		"xdg-desktop-portal"
		"xdg-desktop-portal-gnome"
		"iio-sensor-proxy"
		"clash-verge"
		"firefox"
		"firefox-gnome-theme"
		"snotify-git"
		"flatpak"
		"papers"
# 		# KDE Printing
# 		"system-config-printer"
# 		"print-manager"
# 		# KDE Deps
# 		"klipper"
# 		"phonon-qt5-gstreamer"
# 		"xdg-desktop-portal-kde"
# 		"qt6-multimedia-gstreamer"
# 		"ffmpegthumbs"
# 		"kdegraphics-thumbnailers"
# 		"kde-gtk-config"
# 		"plasma-browser-integration"
# 		"kmail"
# 		"spectacle"
# 		"dolphin"
# 		"kate"
# 		"kwalletmanager"
# 		"konsole"
# 		"gwenview"
# 		"okular"
# 		"ark"
# 		"plasma-meta"
# 		"plasma-wayland-session"
	)
	conflict=("totem")
}

function package_moeOS-git(){
	backup=('etc/moeOS-clash-meta/env.conf' 'etc/moeOS-clash-meta/merge.yaml')
	depends=(
		"dhclient"
		"thermald"
		"pacman-contrib"
		"msr-tools"
		"nano"
		"less"
		"nvidia-prime-moe"
		"lsb-release-moe"
		"bc"
		"paru"
		'bat'
		'glxinfo'
		'networkmanager'
		'plymouth'
		'kernel-modules-hook'
		'linux-firmware'
		'base-devel'
		'paru'
		'sbctl'
		'clash-meta'
		'timeshift'
		'cups'
		'avahi'
		'linux-firmware-whence'
		"go-yq"
		'nftables'
		'iptables-nft'
		"diffutils"
		"zram-generator"
	)
	cd "${srcdir}/moeOS.config"
	if [[ ${_branch} ]]; then
		git checkout ${_branch}
	fi
	install=moeOS-git.install
	createDir
	copyFiles
	for file in os-release mkinitcpio.conf mkinitcpio.d; do
		mv "${pkgdir}"/etc/${file} "${pkgdir}/usr/share/moeOS-Docs"
	done
	configureGraphics
	gnomeShellRt
	genBuildId
	gdmWayland
	blacklistSteamUdev
	fixPermission
}

function gdmWayland(){
	ln -sf /dev/null "${srcdir}/61-gdm.rules"
	install -Dm 644 "${srcdir}/61-gdm.rules" "${pkgdir}/etc/udev/rules.d/61-gdm.rules"
}

function blacklistSteamUdev(){
	ln -sf /dev/null "${srcdir}/70-steam-input.rules"
	install -Dm 644 "${srcdir}/70-steam-input.rules" "${pkgdir}/etc/udev/rules.d/70-steam-input.rules"
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
	echo "BUILD_ID=${pkgver}@$(date +%m/%d/%Y)" >>"${pkgdir}/usr/share/moeOS-Docs/os-release"
}

function initVkIcd(){
	if [[ "${videoMod}" =~ i915 ]] && [[ "${videoMod}" =~ amdgpu ]]; then
		_info "This script currently can't handle Intel + AMD card"
	else
		if [[ "${videoMod}" =~ i915 ]]; then
			_info "Configuring Vulkan to use Intel GPU"
			echo 'VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/intel_icd.x86_64.json:/usr/share/vulkan/icd.d/intel_icd.i686.json' >>"${pkgdir}/etc/environment.d/moeOS-Nvidia-RTD3.conf"
		elif [[ "${videoMod}" =~ amdgpu ]]; then
			_info "Configuring Vulkan to use AMD GPU"
			echo 'VK_ICD_FILENAMES=/usr/share/vulkan/icd.d/radeon_icd.i686.json:/usr/share/vulkan/icd.d/radeon_icd.x86_64.json' >>"${pkgdir}/etc/environment.d/moeOS-Nvidia-RTD3.conf"
		else
			_info "Failed to configure Vulkan ICD filenames"
		fi
	fi
}

function configureGraphics(){
	_info "Configuring graphics card..."
	videoMod=$(lsmod | grep "video " | grep -v "uvcvideo")
	if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ xe ]]; then
		_info "Adding Intel driver and Power Profiles Daemon as dependencies"
		depends+=("power-profiles-daemon" "intel-media-driver" "libva-utils" 'libva' 'gstreamer-vaapi' 'vulkan-intel' )
	elif [[ "${videoMod}" =~ amdgpu ]]; then
		radvVA
		_info "Adding libva-mesa-driver as a dependency"
		if [[ $(cpupower frequency-info | grep driver) =~ epp ]]; then
			_info
			depends+=("power-profiles-daemon")
		fi
		depends+=('libva-mesa-driver' "libva-utils" 'libva' 'gstreamer-vaapi' 'vulkan-radeon')
	fi
	configureNvidia
}

function applyEnv(){
	cp "${srcdir}/moeOS.config/usr/share/moeOS-Docs/Environments.d/$@.conf" "${pkgdir}/etc/environment.d/$@.conf"
}

function radvVA(){
	_info "Enabling vulkan video decode..."
	applyEnv amdVulkanDecode
}


function gnomeShellRt(){
	if [[ $(pacman -Q) =~ gnome-shell-performance ]]; then
		_info "Replacing modified GNOME shell..."
		conflicts+=("gnome-shell-performance")
		depends+=("gnome-shell")
	fi
}

function fixPermission(){
	chmod -R 700 "${pkgdir}/etc/moeOS-clash-meta"
	chmod -R 755 "${pkgdir}/usr/lib/udev/rules.d"
	chmod -R 644 "${pkgdir}/usr/lib/udev/rules.d"/*
	chmod -R 755 "${pkgdir}/usr/bin"
}

function configureNvidia(){
	if [[ ${videoMod} =~ "nvidia_modeset" ]] || [[ ${videoMod} =~ "nouveau" ]]; then
		depends+=('nvidia-utils' 'nvidia-open-dkms' 'lib32-nvidia-utils')
		_info "Fixing RTD3 power management"
		echo "__EGL_VENDOR_LIBRARY_FILENAMES=/usr/share/glvnd/egl_vendor.d/50_mesa.json" >>"${pkgdir}/etc/environment.d/moeOS-Nvidia-RTD3.conf"
		echo '''# Enable runtime PM for NVIDIA VGA/3D controller devices on driver bind
ACTION=="bind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="auto"
ACTION=="bind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030200", TEST=="power/control", ATTR{power/control}="auto"

# Disable runtime PM for NVIDIA VGA/3D controller devices on driver unbind
ACTION=="unbind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030000", TEST=="power/control", ATTR{power/control}="on"
ACTION=="unbind", SUBSYSTEM=="pci", ATTR{vendor}=="0x10de", ATTR{class}=="0x030200", TEST=="power/control", ATTR{power/control}="on"''' >"${pkgdir}/usr/lib/udev/rules.d/80-nvidia-pm.rules"
		echo 'options nvidia "NVreg_DynamicPowerManagement=0x02"' >"${pkgdir}/usr/lib/modprobe.d/moeOS-nvidia-pm.conf"
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
