# Maintainer: Kimiblock Moe
pkgname=("moeOS-git" "lsb-release-moe" "nvidia-prime-moe" "moe-multimedia-meta" "moe-fonts-meta" "moe-input-method" "moe-desktop-meta")
pkgver=r767.8011393
epoch=1
pkgrel=1
pkgdesc="moeOS Configurations"
arch=('x86_64')
url="https://github.com/Kimiblock/moeOS.config"
license=('MIT')
replaces=()
conflicts=("snapd" "optimus-manager" "optimus-manager-qt" "optimus-manager-qt-kde" "gnome-shell-performance")
provides=()
groups=("moeOS")
makedepends=(
	"git"
	"make")
optdepends=()
source=(
	"git+https://github.com/LinuxStandardBase/lsb-samples.git"
	"git+https://github.com/Kimiblock/moeOS.config.git")
sha256sums=(
	"SKIP"
	"SKIP")

function prepare() {
	if [ ! ${moePreferDE} ] && [ -f /etc/environment.d/moeOS-DE.conf ]; then
		export $(cat /etc/environment.d/moeOS-DE.conf)
	fi
}

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
		"pipewire-v4l2"
		'wireplumber'
		'easyeffects'
		"decibels"
		'mpv>0.38'
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

function package_moe-input-method(){
	replaces=("moe-input-meta")
	if [[ ${moePreferIM} = iBus ]] || [[ ! ${moePreferDE} ]]; then
		depends=(
			"librime-data"
			"fcitx5-pinyin-moegirl-rime"
			"rime-moe-pinyin"
			"rime-pinyin-zhwiki"
			"ibus-rime"
		)
		conflicts=(
			"moe-input-meta"
			"moe-input-config"
			"fcitx5-gtk"
			"fcitx5"
			"fcitx5-configtool"
			"fcitx5-qt"
			"fcitx5-rime"
			"gnome-shell-extension-kimpanel-git"
		)
	elif [[ ${moePreferIM} =~ fcitx ]] || [[ ${moePreferDE}=KDE ]]; then
		depends=(
			"librime-data"
			"fcitx5-pinyin-moegirl-rime"
			"rime-moe-pinyin"
			"rime-pinyin-zhwiki"
			"fcitx5-gtk"
			"fcitx5"
			"fcitx5-configtool"
			"fcitx5-qt"
			"fcitx5-rime"
			"gnome-shell-extension-kimpanel-git"
		)
	fi
}

function package_moe-desktop-meta(){
	depends+=(
		"kvantum"
		"cgproxy"
		"highlight"
		"clash-geoip"
		"v2ray-domain-list-community"
		"game-devices-udev"
		"libdecor"
		"ffmpegthumbnailer"
		"libreoffice-fresh"
		"adw-gtk-theme"
		"tela-circle-icon-theme-pink-git"
		"xdg-desktop-portal"
		"iio-sensor-proxy"
		"firefox"
		"snotify"
		"flatpak"
		"orca"
		"espeak-ng"
	)
	if [[ ${moePreferDE} =~ GNOME ]] || [ ! ${moePreferDE} ]; then
		depends+=(
			"qgnomeplatform-qt6-git"
			"qgnomeplatform-qt5-git"
			"fractal"
			"foliate"
			"gnome-shell"
			"mutter-performance"
			"clapper"
			"gnome-shell-extension-appindicator"
			"xdg-desktop-portal-gnome"
			"firefox-gnome-theme"
			"papers"
		)
		applyEnv moeOS-GNOME
		install -Dm644 "${srcdir}"/moeOS.config/usr/share/moeOS-Docs/mime/mimeapps-GNOME.list "${pkgdir}/usr/share/applications/mimeapps.list"
	elif [[ ${moePreferDE} =~ KDE ]]; then
		depends+=(
			# KDE Printing
			"system-config-printer"
			"print-manager"
			# KDE Deps
			"flatpak-kcm"
			"sddm"
			"plasma5-integration"
			"xdg-desktop-portal-kde"
			"qt6-multimedia-gstreamer"
			"ffmpegthumbs"
			"kdegraphics-thumbnailers"
			"kde-gtk-config"
			"plasma-browser-integration"
			"kmail"
			"spectacle"
			"dolphin"
			"kate"
			"kwalletmanager"
			"konsole"
			"gwenview"
			"okular"
			"ark"
			"plasma-meta"
			"phonon-qt6-mpv-git"
		)
		applyEnv moeOS-KDE
		install -Dm644 "${srcdir}"/moeOS.config/usr/share/moeOS-Docs/mime/mimeapps-KDE.list "${pkgdir}/usr/share/applications/mimeapps.list"
		echo "moePreferDE=KDE" >"${pkgdir}/etc/environment.d/moeOS-DE.conf"
	fi
	conflict=("totem")
}

function package_moeOS-git(){
	backup=('etc/moeOS-clash-meta/env.conf' 'etc/moeOS-clash-meta/merge.yaml')
	depends=(
		"systemd-ukify"
		"btrfs-progs"
		"nss-mdns"
		"avahi"
		"systemd-resolvconf"
		"rebuild-detector"
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
		"kernel-modules-hook-bindmount"
		'linux-firmware'
		'base-devel'
		'paru'
		'sbctl'
		'clash-meta'
		'timeshift'
		
		# Printing
		"cups"
		"cups-browsed"
		"ipp-usb"
		"logrotate"
		"avahi"
		
		# Scanning
		"sane-airscan"
		
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

function configureGraphics(){
	_info "Configuring graphics card..."
	videoMod=$(lsmod | grep "video " | grep -v "uvcvideo")
	if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ xe ]]; then
		_info "Adding Intel driver and Power Profiles Daemon as dependencies"
		depends+=("power-profiles-daemon" "intel-media-driver" "libva-utils" "libva" "gstreamer-vaapi" "vulkan-intel" "vpl-gpu-rt")
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
	install -Dm644 "${srcdir}/moeOS.config/usr/share/moeOS-Docs/Environments.d/$@.conf" "${pkgdir}/etc/environment.d/$@.conf"
}

function radvVA(){
	_info "Enabling vulkan video decode..."
	applyEnv amdVulkanDecode
}

function fixPermission(){
	chmod -R 700 "${pkgdir}/etc/moeOS-clash-meta/env.conf"
	chmod 755 "${pkgdir}/usr/lib/udev/rules.d"
	chmod -R 644 "${pkgdir}/usr/lib/udev/rules.d"/*
	chmod -R 755 "${pkgdir}/usr/bin"
	chmod 644 "${pkgdir}/etc/udev/rules.d"
	chmod -R 644 "${pkgdir}/usr/lib/udev/rules.d"/*
}

function configureNvidia(){
	if [[ ${videoMod} =~ "nvidia_modeset" ]] || [[ ${videoMod} =~ "nouveau" ]]; then
		depends+=("nvidia-libgl" "NVIDIA-MODULE")
		optdepends+=("lib32-nvidia-libgl")
		_info "Fixing RTD3 power management"
		install -Dm644 \
			"${srcdir}/moeOS.config/usr/share/moeOS-Docs/udev/80-moe-nvidia-pm.rules" \
			"${pkgdir}/usr/lib/udev/rules.d/80-moe-nvidia-pm.rules"
		install -Dm644 \
			"${srcdir}/moeOS.config/usr/share/moeOS-Docs/modprobe.d/moeOS-nvidia-pm.conf" \
			"${pkgdir}/usr/lib/modprobe.d/moeOS-nvidia-pm.conf"
		if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ amdgpu ]] || [[ "${videoMod}" =~ xe ]]; then
			_info "Your flatpak installation has been configured to not install any Nvidia runtime"
			_info "If you need to run an app on discreate graphics card, install it natively and prepend prime-run when launching"
			if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ xe ]]; then
				applyEnv moeOS-nvidiaOffload-intel
			elif [[ "${videoMod}" =~ amdgpu ]]; then
				applyEnv moeOS-nvidiaOffload-amd
			fi
		fi
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
