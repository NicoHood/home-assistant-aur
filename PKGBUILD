# Maintainer: Grégoire Seux <grego_aur@familleseux.net>
# Contributor: Dean Galvin <deangalvin3@gmail.com>
pkgname="home-assistant"
pkgdesc='Home Assistant is an open-source home automation platform running on Python 3'
pkgver=0.31.1
pkgrel=1
url="https://home-assistant.io/"
license=('MIT')
arch=('any')
replaces=('python-home-assistant')
makedepends=('python-setuptools')
# NB: this package will install additional python packages in /var/lib/hass/lib depending on components present in the configuration files.
depends=('python>=3.4' 'python-pip' 'python-requests' 'python-yaml' 'python-pytz>=2016.6.1' 'python-vincenty' 'python-jinja>=2' 'python-voluptuous>=0.9.3' 'python-netifaces' 'python-webcolors' 'python-eventlet>=0.19.0')
optdepends=('git: install component requirements from github'
            'net-tools: necessary for nmap discovery')
conflicts=('python-home-assistant' 'python-home-assistant-git')
source=("https://github.com/${pkgname}/${pkgname}/archive/${pkgver}.tar.gz"
        "home-assistant.service")
sha256sums=('f4e2b80db79c8c3f550a024ac641823e4a32dfa91830916765bffc7d00336cb1'
            'SKIP')
backup=('var/lib/hass/configuration.yaml')
install='hass.install'

prepare() {
  cd ${srcdir}/${pkgname}-${pkgver}
  set -e

  # package for voluptuous is more recent on AUR
  replace 'voluptuous==0.9.2' 'voluptuous>=0.9.3,<1' setup.py

  # typing package is a backport of standard library < 3.5
  replace 'typing>=3,<4' '' setup.py
}

replace() {
  pattern=$1
  substitute=$2
  file=$3
  echo -n "Replacing '$pattern' by '$substitute' in $file..."
  (grep -q $pattern $file && sed -i "s/$pattern/$substitute/" $file && echo "DONE") || (echo "FAILED" && exit 1)
}

package() {
  mkdir -p "${pkgdir}/usr/lib/systemd/system/"
  cp home-assistant.service "${pkgdir}/usr/lib/systemd/system/"

  cd ${srcdir}/${pkgname}-${pkgver}

  python3 setup.py install --root="$pkgdir" --prefix=/usr --optimize=1
  install -Dm644 "LICENSE" "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}
