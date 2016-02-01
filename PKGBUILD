#Maintainer: Iwan Timmer <irtimmer@gmail.com>

pkgname=kubernetes
pkgver=1.2.0alpha6
_pkgver=1.2.0-alpha.6
pkgrel=1
pkgdesc="Container Cluster Manager for Docker"
depends=('glibc' 'bash')
makedepends=('go' 'rsync')
optdepends=('etcd: etcd cluster required to run Kubernetes')
arch=('x86_64' 'i686')
source=("https://github.com/GoogleCloudPlatform/kubernetes/archive/v$_pkgver.tar.gz"
        "kubernetes.install")
url="http://kubernetes.io/"
license=("APACHE")
install=kubernetes.install
sha256sums=('f75f17751bfe4e7832c57e7e0d983b854281779abeed4377041e08cc51bb8226'
            'f40b4b14a71f8138de69021e967d993e8b14db2cebe66eee20c7e66839ad1fde')

build() {
    cd $srcdir/kubernetes-$_pkgver
    
    ./hack/build-go.sh
}

package() {
    cd $srcdir/kubernetes-$_pkgver

    binaries=(kube-apiserver kube-controller-manager kube-scheduler kube-proxy kubelet kubectl kubemark hyperkube)
    for bin in "${binaries[@]}"; do
        install -Dm755 _output/local/bin/linux/amd64/$bin $pkgdir/usr/bin/$bin
    done
    
    # install the bash completion
    install -dm 0755 $pkgdir/usr/share/bash-completion/completions/
    install -t $pkgdir/usr/share/bash-completion/completions/ contrib/completions/bash/kubectl
    
    # install manpages
    install -d $pkgdir/usr/share/man/man1/
    install -pm 644 docs/man/man1/* $pkgdir/usr/share/man/man1

    # install the place the kubelet defaults to put volumes
    install -d $pkgdir/var/lib/kubelet
}
