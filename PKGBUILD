# Maintainer: Kimiblock Moe
pkgname=("moeOS-git" "lsb-release-moe" "nvidia-prime-moe" "moe-multimedia-meta" "moe-fonts-meta" "moe-inter-font" "moe-input-meta" "moe-desktop-meta" "moe-mpv-modern")
pkgver=r308.5aef336
epoch=1
pkgrel=1
pkgdesc="moeOS Configurations"
arch=('x86_64')
url="https://github.com/Kimiblock/moeOS.config"
license=('MIT')
replaces=()
conflicts=()
provides=()
groups=("moeOS")
makedepends=(
	"git"
	"make")
optdepends=()
source=(
	"git+https://github.com/LinuxStandardBase/lsb-samples.git"
	"git+https://github.com/Kimiblock/moeOS.config.git"
	"git+https://github.com/cyl0/ModernX.git")
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
}

function package_moe-fonts-meta(){
	depends=(
		'noto-fonts-cjk'
		'ttf-twemoji'
		"moe-inter-font"
		"ttf-roboto-mono"
	)
}

function package_moe-mpv-modern(){
	depends=("mpv" "mpv-thumbfast-git")
	install -Dm644 "${srcdir}/ModernX/Material-Design-Iconic-Font.ttf" "${pkgdir}/etc/mpv/fonts/Material-Design-Iconic-Font.ttf"
	install -Dm644 "${srcdir}/ModernX/modernx.lua" "${pkgdir}/etc/mpv/scripts/modernx.lua"
}

function package_moe-inter-font(){
	provides=("inter-font")
	conflicts=("inter-font")
	getLatestRel "rsms/inter" github
	_interVer=$(echo ${_release} | cut -c 2-)
	if [ -f inter.zip ]; then
		_info "Skipping Download"
	else
		curl "https://github.com/rsms/inter/releases/download/${_release}/Inter-${_interVer}.zip" -o "${srcdir}/inter.zip" -L
	fi
	mkdir -p "${pkgdir}/usr/share/fonts/inter/"
	unzip -qo inter.zip
	cp -r "${srcdir}"/Inter\ Hinted\ for\ Windows/Desktop/* "${pkgdir}/usr/share/fonts/inter/"
	cp "${pkgdir}/usr/share/fonts/inter/Inter-Medium.ttf" "${pkgdir}/usr/share/fonts/inter/Inter-Regular.ttf"
}

function getLatestRel(){
	if [[ $2 = github ]]; then
		_rawVersion=$(curl -s https://api.github.com/repos/"$1"/releases/latest | jq .tag_name)
		_release=$(echo "${_rawVersion}" | cut -c 2-$(expr ${#_rawVersion} - 1))
	else
		return 1
	fi
}

function package_moe-input-meta(){
	depends=(
		#"gnome-shell-extension-kimpanel-git"
		"rime-essay-simp"
		"rime-essay"
		"librime-data"
		"fcitx5-pinyin-moegirl-rime"
		"rime-minecraft-dict-git"
		"rime-ice"
		"fcitx5-gtk"
		"fcitx5"
		"fcitx5-configtool"
		"fcitx5-qt"
		"fcitx5-rime"
	)
	conflicts=(
		"ibus-rime"
	)
}

function package_moe-desktop-meta(){
	depends=(
		#"gnome-shell"
		"plasma-wayland-session"
		"material-cursors-git"
		"adw-gtk-theme"
		#"gnome-shell-extension-appindicator"
		"tela-circle-icon-theme-pink-git"
		"xdg-desktop-portal"
		#"xdg-desktop-portal-gnome"
		"iio-sensor-proxy"
		"clash-verge"
		"librewolf"
		"librewolf-ublock-origin"
		"librewolf-extension-dark-reader"
		"librewolf-extension-bitwarden"
		"librewolf-extension-violentmonkey-bin"
		"librewolf-extension-sponsorblock-bin"
		"firefox-gnome-theme"
		"snotify-git"
		"flatpak"
		# KDE Printing
		"system-config-printer"
		"print-manager"
		# KDE Deps
		"klipper"
		"phonon-qt5-gstreamer"
		"xdg-desktop-portal-kde"
		"qt6-multimedia-gstreamer"
		"ffmpegthumbs"
		"kdegraphics-thumbnailers"
		"kde-gtk-config"
		"plasma-browser-integration"
		"kmail"
	)
}

function package_moeOS-git(){
	backup=('etc/moeOS-clash-meta/subscribe.conf' 'etc/moeOS-clash-meta/merge.yaml')
	depends=(
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
	)
	install=moeOS-git.install
	createDir
	copyFiles
	for file in os-release sbupdate.conf mkinitcpio.conf mkinitcpio.d; do
		mv "${pkgdir}"/etc/${file} "${pkgdir}/usr/share/moeOS-Docs"
	done
	configureGraphics
	gnomeShellRt
	genBuildId
	gdmWayland
	fixPermission
}

function gdmWayland(){
	mkdir -p "${pkgdir}/etc/udev/rules.d"
	ln -s /dev/null "${pkgdir}/etc/udev/rules.d/61-gdm.rules"
	chmod 644 -R "${pkgdir}/etc/udev/rules.d"
}

function firefoxNightlyConfig(){
	install -Dm644 "${srcdir}/moeOS.config/usr/share/moeOS-Docs/librewolf.d/librewolf.cfg" "${pkgdir}/opt/firefox-nightly/defaults/pref/moeOS.js"
	sed -i 's/lockPref/pref/g' "${pkgdir}/opt/firefox-nightly/defaults/pref/moeOS.js"
	sed -i 's/defaultPref/pref/g' "${pkgdir}/opt/firefox-nightly/defaults/pref/moeOS.js"
}

function copyFiles(){
	_info "Moving Files..."
	cp -r "${srcdir}"/moeOS.config/usr "${pkgdir}"
	cp -r "${srcdir}"/moeOS.config/etc "${pkgdir}"
	firefoxNightlyConfig
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
	#depends+=("mutter-performance")
	_info "Enabling rt schedulers for mutter..."
	if [[ $(pacman -Q) =~ gnome-shell-performance ]]; then
		_info "Replacing modified GNOME shell..."
		conflicts+=("gnome-shell-performance")
		depends+=("gnome-shell")
	fi
}

function fixPermission(){
	chmod -R 700 "${pkgdir}/etc/moeOS-clash-meta"
	chmod -R 755 "${pkgdir}/usr/lib/udev/rules.d"
	chmod -R 755 "${pkgdir}/usr/bin"
	chmod -R 755 "${pkgdir}/usr/share/rime-data"
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
