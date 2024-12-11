# Maintainer: Kimiblock Moe
pkgname=("moeOS-git" "lsb-release-moe" "nvidia-prime-moe" "moe-multimedia-meta" "moe-fonts-meta" "moe-input-method" "moe-desktop-meta")
pkgver=r1181.b94e5a0
epoch=1
pkgrel=1
pkgdesc="moeOS Configurations"
arch=('x86_64')
url="https://github.com/Kimiblock/moeOS.config"
license=('MIT')
replaces=()
conflicts=("snapd" "optimus-manager" "optimus-manager-qt" "optimus-manager-qt-kde" "gnome-shell-performance")
provides=("drkonqi")
groups=("moeOS")
makedepends=(
	"git"
	"make")
optdepends=()
source=(
	"git+https://github.com/LinuxStandardBase/lsb-samples.git"
	"git+https://github.com/Kimiblock/moeOS.config.git"
	"git+https://github.com/Kimiblock/webpfier.git")
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
		'pipewire-alsa'
		'pipewire-audio'
		'pipewire-jack'
		'pipewire-pulse'
		"pipewire-v4l2"
		"pipewire-libcamera"
		'wireplumber'
		'easyeffects'
		'mpv'
		"mpv-mpris"
		'mediainfo'
		'rtaudio'
		'sof-firmware'
		'gst-plugin-va'
		'yt-dlp'
		'python-mutagen'
		"wl-clipboard"
	)
	optdepends+=("lib32-pipewire")
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
	)
}

function package_moe-desktop-meta(){
	depends+=(
		"portable"
		"cgtproxy"
		"usbguard"
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
		"exfat-utils" # exfatprogs doesn't seem good for Nautilus
		"7-zip-full" # Conflits p7zip
	)
	if [[ $(cat /etc/environment.d/moeOS-DE.conf) =~ "moePreferDE=KDE" ]] || [[ ${moePreferDE} = KDE ]]; then
		if [[ ${moePreferDE} = GNOME ]]; then
			gnomeMeta
		else
			plasmaMeta
		fi
	else
		gnomeMeta
	fi
	if [ "${ENABLE_HDR_WSI}" = 1 ]; then
		echo 'ENABLE_HDR_WSI=1' >>"${pkgdir}/etc/environment.d/moeOS-HDR.conf"
	fi
	install -Dm644 "${srcdir}/webpfier/awebpfier.desktop" \
		"${pkgdir}/usr/share/applications/awebpfier.desktop"
	install -Dm644 "${srcdir}/webpfier/webpfier.svg" \
		"${pkgdir}/usr/share/icons/hicolor/scalable/apps/webpfier.svg"
	install -Dm755 "${srcdir}/webpfier/webpfier" \
		"${pkgdir}/usr/bin/webpfier"
}

function gnomeMeta() {
	applyEnv moeOS-GNOME
	install -Dm644 "${srcdir}"/moeOS.config/usr/share/moeOS-Docs/mime/mimeapps-GNOME.list "${pkgdir}/usr/share/applications/mimeapps.list"
	depends+=(
		"obfuscate"
		# GSConnect
		"nautilus-python"
		"gnome-shell-extension-gsconnect"
		"gnome-sound-recorder"
		"kvantum"
		"geary"
		"errands"
		"decibels"
		"gapless"
		"qadwaitadecorations-qt5"
		"qadwaitadecorations-qt6"
		"fractal"
		"foliate"
		"gnome-shell"
		"mutter-performance"
		"clapper"
		"gnome-shell-extension-appindicator"
		"xdg-desktop-portal-gnome"
		"firefox-gnome-theme"
		"papers"
		# GNOME pkg group
		"epiphany"
		"baobab"
		"evince"
		"gdm"
		"gnome-backgrounds"
		"gnome-calculator"
		"gnome-calendar"
		"gnome-characters"
		"gnome-clocks"
		"gnome-color-manager"
		"gnome-connections"
		"gnome-console"
		"gnome-contacts"
		"gnome-control-center"
		"gnome-disk-utility"
		"gnome-font-viewer"
		"gnome-keyring"
		"gnome-logs"
		"gnome-maps"
		"gnome-menus"
		"gnome-remote-desktop"
		"gnome-session"
		"gnome-settings-daemon"
		"gnome-shell"
		"gnome-shell-extensions"
		"gnome-software"
		"gnome-system-monitor"
		"gnome-text-editor"
		"gnome-tour"
		"gnome-user-docs"
		"gnome-weather"
		"gnome-power-manager"
		"grilo-plugins"
		"gvfs"
		"gvfs-afc"
		"gvfs-dnssd"
		"gvfs-goa"
		"gvfs-google"
		"gvfs-gphoto2"
		"gvfs-mtp"
		"gvfs-nfs"
		"gvfs-onedrive"
		"gvfs-smb"
		"gvfs-wsdd"
		"loupe"
		"nautilus"
		"rygel"
		"simple-scan"
		"snapshot"
		"tecla"
		"tracker3-miners"
		"xdg-user-dirs-gtk"
		"fragments"
		"gnome-builder"
		"newsflash"
	)
	conflicts+=(
		"fcitx5-gtk"
		"fcitx5"
		"fcitx5-configtool"
		"fcitx5-qt"
		"fcitx5-rime"
	)
}

function plasmaMeta() {
	mkdir -p "${pkgdir}/etc/environment.d"
	echo "moePreferDE=KDE" \
		>"${pkgdir}/etc/environment.d/moeOS-DE.conf"
	install \
		-Dm644 \
		"${srcdir}"/moeOS.config/usr/share/moeOS-Docs/mime/mimeapps-KDE.list \
		"${pkgdir}/usr/share/applications/mimeapps.list"
	applyEnv moeOS-KDE
	depends+=(
		"vk-hdr-layer-kwin6-git"
		# IME
		"fcitx5-gtk"
		"fcitx5"
		"fcitx5-configtool"
		"fcitx5-qt"
		"fcitx5-rime"
		# KDE Printing
		"system-config-printer"
		"print-manager"
		"kcolorchooser"
		"kdiskmark"
		# KDE Deps
		"arianna"
		"kamoso"
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
		"elisa"
		"kwalletmanager"
		"konsole"
		"gwenview"
		"kcalc"
		"okular"
		"ark"
		"plasma-meta"
		"phonon-qt6-mpv-git"
		"nheko"
		"kimageformats"
		"gst-plugin-qmlgl"
		"kio"
		"kio-zeroconf"
		"kio-admin"
		"kio-extras"
		"kio-fuse"
		"kio-gdrive"
		"partitionmanager"
		"filelight"
		"ksystemlog"
		"kgpg"
		"krdc"
		"skanlite"
		"kompare"
		"krfb"
		"kweather"
		"kmouth"
		"audiotube"
		"digikam"
		"krecorder"
		"isoimagewriter"
		"tokodon"
		"alligator"
	)
}

function package_moeOS-git(){
	backup=(
	'etc/moeOS-clash-meta/env.conf'
	'etc/moeOS-clash-meta/merge.yaml'
	"etc/default/seconnect"
	)
	depends=(
		"scx-scheds"
		"lld"
		"systemd-ukify"
		"btrfs-progs"
		"hfsprogs"
		"nss-mdns"
		"dnsmasq"
		"avahi"
		"systemd-resolvconf"
		"rebuild-detector"
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
		"sudo"
		
		# Time syncing
		"chrony"
		
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
		"watchdog"
	)
	cd "${srcdir}/moeOS.config"
	if [[ ${_branch} ]]; then
		git checkout ${_branch}
	fi
	createDir
	copyFiles
	for file in os-release mkinitcpio.conf mkinitcpio.d; do
		mv "${pkgdir}"/etc/${file} "${pkgdir}/usr/share/moeOS-Docs"
	done
	configureGraphics
	genBuildId
	fixPermission
}

function copyFiles(){
	cp -r "${srcdir}"/moeOS.config/usr "${pkgdir}"
	cp -r "${srcdir}"/moeOS.config/etc "${pkgdir}"
	cp -r "${srcdir}"/moeOS.config/var "${pkgdir}"
}

function createDir(){
	for dir in "/usr/lib/udev/rules.d" "/usr/lib/modprobe.d"; do
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

function fixPermission() {
	chmod -R 700 "${pkgdir}/etc/moeOS-clash-meta/env.conf"
	chmod 755 "${pkgdir}/usr/lib/udev/rules.d"
	chmod -R 644 "${pkgdir}/usr/lib/udev/rules.d"/*
	chmod -R 755 "${pkgdir}/usr/bin"
	chmod 644 "${pkgdir}/etc/udev/rules.d"
	chmod -R 644 "${pkgdir}/usr/lib/udev/rules.d"/*
}

function configureNvidiaOnly() {
	echo "[Info] NVIDIA only mode enabled"
	depends+=("nvidia-vaapi-driver")
	sed -i "s|vaapi,vulkan,auto|nvdec,vulkan,auto|g" "${pkgdir}/etc/mpv/mpv.conf"
	sed -i "s|dmabuf-wayland|gpu-next|g" "${pkgdir}/etc/mpv/mpv.conf"
	sed -i 's|gpu-hwdec-interop|#gpu-hwdec-interop|g' "${pkgdir}/usr/share/moeOS-Docs/Celluloid.d/celluloid.options"
	sed -i 's|vaapi|auto|g' "${pkgdir}/usr/share/moeOS-Docs/Celluloid.d/celluloid.options"
	applyEnv moeOS-nvidiaOnly
	echo 'GST_PLUGIN_FEATURE_RANK=nvh264dec:512,nvav1dec:512,nvh265dec:512,nvvp8dec:512,nvvp9dec:512,nvmpegvideodec:512,nvmpeg4videodec:512,nvmpeg2videodec:512,nvjpegdec:512,nvh265enc:512,nvh264enc:512' >"${pkgdir}/etc/environment.d/moeOS-GStreamer.conf"
	return 0
}

function configureNvidia() {
	if [ ${moeNouveau} ]; then
		echo "[Info] Nouveau enabled"
		conflicts+=("nvidia-libgl" "NVIDIA-MODULE" "lib32-nvidia-libgl" "nvidia-settings" "nvidia-vaapi-driver-git" "egl-wayland")
		depends+=("vulkan-nouveau")
		echo "[Warn] Enable kernel parameter nouveau.config=NvGspRm=1!"
		if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ amdgpu ]] || [[ "${videoMod}" =~ xe ]] || [[ ${moeNouveau} =~ intel ]] || [[ ${moeNouveau} =~ amd ]]; then
			_info "If you need to run an app on discreate graphics card, use nouveau-prime"
			conflicts+=("nvidia-vaapi-driver")
			if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ xe ]] || [[ ${moeNouveau} =~ intel ]]; then
				applyEnv moeOS-nouveauOffload-intel
			elif [[ "${videoMod}" =~ amdgpu ]] || [[ ${moeNouveau} =~ amd ]]; then
				applyEnv moeOS-nouveauOffload-amd
			fi
		fi
	elif [[ ${videoMod} =~ "nvidia_modeset" ]] || [[ ${videoMod} =~ "nouveau" ]]; then
		depends+=("nvidia-libgl" "NVIDIA-MODULE")
		optdepends+=("lib32-nvidia-libgl")
		_info "Fixing RTD3 power management"
		install -Dm644 \
			"${srcdir}/moeOS.config/usr/share/moeOS-Docs/udev/80-moe-nvidia-pm.rules" \
			"${pkgdir}/usr/lib/udev/rules.d/80-moe-nvidia-pm.rules"
		if [[ "${moeDiscreteOnly}" = 1 ]]; then
			configureNvidiaOnly
		elif [[ ! "${videoMod}" =~ i915 ]] && [[ ! "${videoMod}" =~ amdgpu ]] && [[ ! "${videoMod}" =~ xe ]]; then
			configureNvidiaOnly
		elif [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ amdgpu ]] || [[ "${videoMod}" =~ xe ]]; then
			_info "If you need to run an app on discreate graphics card, consult README"
			conflicts+=("nvidia-vaapi-driver")
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
