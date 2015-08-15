# Maintainer: XZS <d dot f dot fischer at web dot de>
# This PKGBUILD is maintained on GitHub <https://github.com/dffischer/gnome-shell-extensions>.
# You may find it convenient to file issues and pull requests there.

pkgname='gnome-shell-extension-buttons-to-panel'
pkgdesc="Shows the window buttons of the focused window in the GNOME top panel"
pkgver=1
pkgrel=4
arch=(any)
url='http://sourceforge.net/projects/buttons-to-panel/'
license=(GPLv2)
depends=('gnome-shell>=3.4' 'gnome-shell<3.15')
install=gschemas-and-warn.install
source=("${pkgname}.zip::http://sourceforge.net/projects/buttons-to-panel/files/latest/download")
md5sums=('ed4df64545666f4472d4b50f3e510a38')

prepare() {
  # adjust for shell versions that are not officially supported.
  local min=$(echo ${depends[@]} | grep -Po '(?<=gnome-shell>=3\.)[[:digit:]]+')
  local max=$(echo ${depends[@]} | grep -Po '(?<=gnome-shell<3\.)[[:digit:]]+')
  if [ -z "$max" ]
  then max=$(
    gnome-shell --version | grep -Po '(?<=GNOME Shell 3\.)[[:digit:]]+'
  ); fi
  find -name 'metadata.json' -exec sed -i 'H;1h;$!d;x;
    s/"shell-version": \[.*\]/"shell-version": [ '"$(seq -s ', ' -f '"3.%g"' $min 2 $max)"' ]/' \
    '{}' +
}
package() {
  for function in $(declare -F | grep -Po 'package_[[:digit:]]+[[:alpha:]_]*$')
  do
    $function
  done
}
package_01_locate() {
  msg2 'Locating extension...'
  cd "$(dirname $(find -name 'metadata.json' -print -quit))"
  extname=$(grep -Po '(?<="uuid": ")[^"]*' metadata.json)
  destdir="$pkgdir/usr/share/gnome-shell/extensions/$extname"
}

package_02_install() {
  msg2 'Installing extension code...'
  find -maxdepth 1 \( -iname '*.js*' -or -iname '*.css' -or -iname '*.ui' \) -exec install -Dm644 -t "$destdir" '{}' +
}
if [ -z "$install" ]
then
  install=gschemas.install
fi

package_10_schemas() {
  msg2 'Installing schemas...'
  find -name '*.xml' -exec install -Dm644 -t "$pkgdir/usr/share/glib-2.0/schemas" '{}' +
}
depends+=(gnome-shell-extensions)

package_03_unify_conveniencejs() {
  ln -fs \
    ../user-theme@gnome-shell-extensions.gcampax.github.com/convenience.js \
    "$destdir/convenience.js"
}

package_09_theme() {
  cp -r --no-preserve=ownership,mode themes "$destdir"
}
