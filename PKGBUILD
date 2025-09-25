# Maintainer: Kimiblock Moe
pkgname=("moeOS-git" "lsb-release-moe" "nvidia-prime-moe" "moe-multimedia-meta" "moe-fonts-meta" "moe-input-method" "moe-desktop-meta")
pkgver=r1607.0ed562d
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
		'samba'
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
		'ttf-noto-sans-cjk-vf'
		'ttf-noto-sans-mono-cjk-vf'
		'ttf-noto-serif-cjk-vf'
		'noto-fonts-emoji'
		'ttf-noto-sans-vf'
		'ttf-noto-serif-vf'
		"inter-font"
		"ttf-roboto-mono"
	)
	provides+=(
		adwaita-fonts
		cantarell-fonts
		adobe-source-code-pro-fonts
	)
	conflicts+=(
		#noto-fonts
		#noto-fonts-cjk
		#ttf-twemoji
		adwaita-fonts
		cantarell-fonts
		adobe-source-code-pro-fonts
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
		"librime-luajit"
		"rime-pinyin-moegirl"
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
		"scx-scheds"
		"libwebp-utils"
		"ppp"
		"geoclue"
		"xpadneo-dkms"
		"xpad-noone"
		#"xone-dongle-firmware"
		"xone-dkms-git"
		"portable"
		"drm_info"
		"steam-devices-git"
		"cgproxy"
		"usbguard"
		"highlight"
		"clash-geoip"
		"v2ray-domain-list-community"
		"dae"
		"game-devices-udev"
		"libdecor"
		"ffmpegthumbnailer"
		"libreoffice-fresh"
		"adw-gtk-theme"
		"tela-circle-icon-theme-pink"
		"xdg-desktop-portal"
		"iio-sensor-proxy"
		"zen-browser-portable"
		"zen-browser-single-file"
		"zen-browser-ublock-origin"
		"zen-browser-violentmonkey"
		"zen-browser-bitwarden"
		"zen-browser-dark-reader"
		"thunderbird-portable"
		"snotify"
		"flatpak"
		"orca"
		"espeak-ng"
		"exfat-utils" # exfatprogs doesn't seem good for Nautilus
		"7zip" # Conflits p7zip
		"zju-connect-bin"
		"openrgb"
	)
	#conflicts+=("vk-hdr-layer-kwin6-git")
	if [[ $(cat /etc/environment.d/moeOS-DE.conf) =~ "moePreferDE=KDE" ]] || [[ ${moePreferDE} = KDE ]]; then
		if [[ ${moePreferDE} = GNOME ]]; then
			gnomeMeta
		else
			plasmaMeta
		fi
	else
		gnomeMeta
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
		"solanum"
		"delfin"
		"wildcard"
		"clapper"
		"resources"
		"qt6ct"
		"switcheroo"
		"eartag"
		"metadata-cleaner"
		"obfuscate"
		# GSConnect
		"nautilus-python"
		"gnome-shell-extension-gsconnect"
		"gnome-sound-recorder"
		"file-roller"
		"gnome-mahjongg"
		"kvantum"
		"geary"
		"decibels"
		"gapless"
		"fractal"
		"foliate"
		"gnome-shell"
		"mutter"
		"showtime"
		"gnome-shell-extension-appindicator"
		"xdg-desktop-portal-gnome"
		"papers"
		# GNOME pkg group
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
		"mutter-performance"
		"kdeconnect"
		"gnome-terminal"
		"epiphany"
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
	conflicts+=(
		gnome-shell
		gnome-keyring
		nautilus
		gnome-console
	)
	depends+=(
		"appmenu-gtk-module"
		"vlc-plugins-all"
		"merkuro"
		"akregator"
		"kalarm"
		"kontact"
		"kdepim-addons"
		"kaddressbook"
		"korganizer"
		"zanshin"
		"kdeconnect"
		#"vk-hdr-layer-kwin6-git"
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
		#"phonon-qt6-mpv"
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
	'etc/moeOS-seconnect/config.toml'
	)
	depends=(
		"lld"
		"tuned"
		"tuned-ppd"
		"x86_energy_perf_policy"
		"systemtap"
		"virt-what"
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
		'modemmanager'
		'usb_modeswitch'
		'plymouth'
		"kernel-modules-hook-bindmount"
		'linux-firmware'
		'linux-firmware-bnx2x'
		'linux-firmware-marvell'
		'linux-firmware-mellanox'
		'linux-firmware-nfp'
		'linux-firmware-qcom'
		'linux-firmware-qlogic'
		'linux-firmware-cirrus'
		'linux-firmware-liquidio'
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
		"cups-pdf"
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

		"hunspell"
		"hunspell-en_gb"
		"hunspell-en_us"
		"hunspell-en_ca"
		"hunspell-de"
		"hunspell-el"
		"hunspell-en_au"
		"hunspell-es_any"
		"hunspell-fr"
		"hunspell-he"
		"hunspell-hu"
		"hunspell-it"
		"hunspell-nl"
		"hunspell-pl"
		"hunspell-ro"
		"hunspell-ru"
		fwupd
	)
	cd "${srcdir}/moeOS.config"
	if [[ ${_branch} ]]; then
		git checkout ${_branch}
	fi
	copyFiles
	for file in os-release mkinitcpio.conf mkinitcpio.d; do
		mv "${pkgdir}"/etc/${file} "${pkgdir}/usr/share/moeOS-Docs"
	done
	_info "Done moving files"
	configureGraphics
	_info "Done configuring graphics"
	genBuildId
	fixPermission
}

function copyFiles(){
	_info "Copying files"
	cp -r "${srcdir}"/moeOS.config/usr "${pkgdir}"
	cp -r "${srcdir}"/moeOS.config/etc "${pkgdir}"
	cp -r "${srcdir}"/moeOS.config/var "${pkgdir}"
	_info "Done copying files"
}

function genBuildId(){
	echo "BUILD_ID=${pkgver}@$(date +%m/%d/%Y)" >>"${pkgdir}/usr/share/moeOS-Docs/os-release"
}

function configureGraphics(){
	if [[ -z "${videoMod}" ]]; then
		videoMod=$(lsmod | grep -v "uvcvideo")
		if [[ "${videoMod}" =~ 'video ' ]]; then
			_info "Configuring graphics"
			videoMod=$(echo "${videoMod}" | grep "video ")
		else
			return 0
		fi
	else
		_info "Using pre-defined videoMod"
	fi

	if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ xe ]]; then
		_info "Adding Intel driver and Power Profiles Daemon as dependencies"
		depends+=("intel-media-driver" "libva-utils" "libva" "gstreamer-vaapi" "vulkan-intel" "vpl-gpu-rt")
	elif [[ "${videoMod}" =~ amdgpu ]]; then
		_info "Adding libva-mesa-driver as a dependency"
		depends+=('libva-mesa-driver' "libva-utils" 'libva' 'gstreamer-vaapi' 'vulkan-radeon')
	fi
	configureNvidia
}

function applyEnv(){
	install -Dm644 "${srcdir}/moeOS.config/usr/share/moeOS-Docs/Environments.d/$@.conf" "${pkgdir}/etc/environment.d/$@.conf"
}

function fixPermission() {
	chmod -R 700 "${pkgdir}/etc/moeOS-clash-meta/env.conf"
	chmod -R 700 "${pkgdir}/etc/moeOS-seconnect/config.toml"
	chmod 755 "${pkgdir}/usr/lib/udev/rules.d"
	chmod -R 644 "${pkgdir}/usr/lib/udev/rules.d"/*
	chmod -R 755 "${pkgdir}/usr/bin"
	chmod 644 "${pkgdir}/etc/udev/rules.d"
	chmod -R 644 "${pkgdir}/usr/lib/udev/rules.d"/*
}

function configureNvidiaOnly() {
	echo "[Info] NVIDIA only mode enabled"
	depends+=("libva-nvidia-driver")
	sed -i "s|vulkan,vaapi,auto|vulkan,nvdec,auto|g" "${pkgdir}/etc/mpv/mpv.conf"
	sed -i "s|dmabuf-wayland|gpu-next|g" "${pkgdir}/etc/mpv/mpv.conf"
	sed -i 's|gpu-hwdec-interop|#gpu-hwdec-interop|g' "${pkgdir}/usr/share/moeOS-Docs/Celluloid.d/celluloid.options"
	sed -i 's|vaapi|auto|g' "${pkgdir}/usr/share/moeOS-Docs/Celluloid.d/celluloid.options"
	applyEnv moeOS-nvidiaOnly
	ln -sf \
		"/usr/share/moeOS-Docs/Environments.d/moeOS-GStreamer.nv.conf" \
		"${pkgdir}/usr/lib/environment.d/moeOS-GStreamer.conf"
	return 0
}

function configureNvidia() {
	if [ ${moeNouveau} ]; then
		echo "[Info] Nouveau enabled"
		conflicts+=("nvidia-libgl" "NVIDIA-MODULE" "lib32-nvidia-libgl" "nvidia-settings" "nvidia-vaapi-driver-git" "libva-nvidia-driver")
		depends+=("vulkan-nouveau" "linux-firmware-nvidia")
		echo "[Warn] Enable kernel parameter nouveau.config=NvGspRm=1!"
		if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ amdgpu ]] || [[ "${videoMod}" =~ xe ]] || [[ ${moeNouveau} =~ intel ]] || [[ ${moeNouveau} =~ amd ]]; then
			_info "If you need to run an app on discreate graphics card, use prime-run"
			conflicts+=("nvidia-vaapi-driver" "libva-nvidia-driver")
			if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ xe ]] || [[ ${moeNouveau} =~ intel ]]; then
				applyEnv moeOS-nouveauOffload-intel
			elif [[ "${videoMod}" =~ amdgpu ]] || [[ ${moeNouveau} =~ amd ]]; then
				applyEnv moeOS-nouveauOffload-amd
			fi
		fi
	elif [[ ${videoMod} =~ "nvidia_modeset" ]] || [[ ${videoMod} =~ "nouveau" ]]; then
		depends+=("nvidia-libgl" "NVIDIA-MODULE")
		optdepends+=("lib32-nvidia-libgl")
		if [[ "${moeDiscreteOnly}" = 1 ]]; then
			configureNvidiaOnly
		elif [ $(ls /dev/dri/renderD* -la | wc -l) = 1 ] && [[ ${videoMod} =~ nvidia ]]; then
			configureNvidiaOnly
		elif [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ amdgpu ]] || [[ "${videoMod}" =~ xe ]]; then
			_info "If you need to run an app on discreate graphics card, consult README"
			conflicts+=("nvidia-vaapi-driver" "libva-nvidia-driver")
			if [[ "${videoMod}" =~ i915 ]] || [[ "${videoMod}" =~ xe ]]; then
				applyEnv moeOS-nvidiaOffload-intel
			elif [[ "${videoMod}" =~ amdgpu ]]; then
				applyEnv moeOS-nvidiaOffload-amd
			fi
		fi
	fi
}

function _info() {
#	if [ -f /usr/bin/pamac ]; then
#		echo "  ==> [Info]: $@"
#	else
#		all_off="$(tput sgr0)"
#		bold="${all_off}$(tput bold)"
#		blue="${bold}$(tput setaf 4)"
#		yellow="${bold}$(tput setaf 3)"
#		printf "${blue}==>${yellow} [Info]:${bold} $1${all_off}\n"
#	fi
	echo "[Info] $@"
}
