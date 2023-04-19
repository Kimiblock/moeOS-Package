pkgname=moeOS
pkgver=r46.c085302
pkgrel=1
pkgdesc="moeOS Configurations"
arch=('x86_64')
url="https://github.com/Kimiblock/moeOS.config"
License=('none')
install=${pkgname}.install
depends=(
    'tela-circle-icon-theme-pink-git'
    'tlp'
    'networkmanager'
    'plymouth'
    'mpv'
    'noto-fonts'
    'noto-fonts-cjk'
    'noto-fonts-emoji'
    'noto-fonts-extra'
    'pamac-aur'
    'base-devel'
    'paru'
    'firefox'
    'kdenlive'
    'kwalletmanager'
    'kvantum'
    'icalingua++'
    'nheko'
    'iio-sensor-proxy'
    'sbupdate-git'
    'firefox-gnome-theme-git'
    'marktext-git'
    'qbittorrent-enhanced-git'
    'sddm-git'
    'clash-meta'
    'clash-verge'
    'timeshift'
    'cups'
    'avahi'
    'lsb-release'
    'obs-gstreamer'
    'ark'
    'packagekit-qt5'
    'flatpak-kcm'
    'sddm-kcm'
    'fcitx5-im')
makedepends=(
    'git')
source=('git+https://github.com/Kimiblock/moeOS.config.git')
sha256sums=('SKIP')

function package(){
    for dir in /usr/share/libalpm/hooks /usr/share/moeOS-Docs /usr/share/plymouth/themes /usr/share/icons/hicolor/512x512/apps; do
        _info Creating directory ${dir}
        mkdir -p "${pkgdir}${dir}"
    done
    for source in etc usr/bin moeOS.bmp usr/share/plymouth/themes/moe usr/share/icons/hicolor/512x512/apps/moeos.png; do
        _info Copying "${source}"
        cp "${srcdir}"/moeOS.config/${source} "${pkgdir}"/${source} -r
    done
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

