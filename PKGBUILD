#Maintainer: Iwan Timmer <irtimmer@gmail.com>

pkgname=kubernetes
pkgver=1.13.1
pkgrel=1
pkgdesc="Production-Grade Container Scheduling and Management"
depends=('glibc' 'bash')
makedepends=('go' 'rsync' 'go-bindata')
optdepends=('etcd: etcd cluster required to run Kubernetes')
arch=('x86_64')
source=("$pkgname-$pkgver.tar.gz::https://dl.k8s.io/v$pkgver/kubernetes-src.tar.gz"
	"kubelet"
	"kubelet.service")
noextract=("$pkgname-$pkgver.tar.gz")
url="http://kubernetes.io/"
license=("APACHE")
backup=('etc/kubernetes/kubelet')
sha256sums=('70a088fcf60a46e2b470d3e7a673447f6cfc62b3d9fab4b1d0eb82ede8e5ed03'
            '3240aa7b4c6c3c19f23e3a664f8c7ce8f44294c2fa00cc90df1e7bcc8d067be3'
            'b9e6850042a9cfda12ba74737917c09b5eefe44431996ed281bc657cec341e87')

prepare() {
    mkdir -p $srcdir/$pkgname-$pkgver
    tar -xf $srcdir/$pkgname-$pkgver.tar.gz -C $srcdir/$pkgname-$pkgver
}

build() {
    cd $srcdir/$pkgname-$pkgver
    
    make -j1
    #hack/generate-docs.sh
}

package() {
    cd $srcdir/$pkgname-$pkgver

    binaries=(kubelet kubeadm kubectl)
    for bin in "${binaries[@]}"; do
        install -Dm755 _output/local/bin/linux/amd64/$bin $pkgdir/usr/bin/$bin
    done
   
    # install manpages
    # install -d $pkgdir/usr/share/man/man1/
    # install -pm 644 docs/man/man1/* $pkgdir/usr/share/man/man1

    # install the place the kubelet defaults to put volumes
    install -d $pkgdir/var/lib/kubelet

    cd $srcdir/

    # install kubelet config
    install -dm 755 $pkgdir/etc/kubernetes/
    install -m 644 -t $pkgdir/etc/kubernetes/ kubelet

    # install kubelet service
    install -dm 755 $pkgdir/usr/lib/systemd/system
    install -m 644 -t $pkgdir/usr/lib/systemd/system kubelet.service
}
