
pkgname=scopehal-apps-git
pkgver=0.0.bf81dbc
pkgrel=1
pkgdesc="glscopeclient and other client applications for libscopehal"
arch=('x86_64')
url="https://github.com/glscopeclient/scopehal-apps"
license=('BSD')
groups=()
depends=('gtkmm3' 'libsigc++' 'ffts' 'openmp' 'glfw' 'libvulkan.so' 'yaml-cpp' 'glew' 'catch2' 'spirv-tools' 'shaderc' 'liblxi' 'linux-gpib')
makedepends=('cmake' 'git' 'vulkan-headers')
source=("git+https://github.com/glscopeclient/scopehal-apps.git"
        "git+https://github.com/glscopeclient/scopehal.git"
        "git+https://github.com/glscopeclient/xptools.git"
        "git+https://github.com/glscopeclient/logtools.git"
        "git+https://github.com/glscopeclient/graphwidget.git"
        "git+https://github.com/glscopeclient/VkFFT.git"
        "git+https://github.com/glscopeclient/scopehal-docs.git"
        "git+https://github.com/ocornut/imgui.git"
        "git+https://github.com/glscopeclient/implot.git"
        "git+https://github.com/glscopeclient/imgui-node-editor.git"
        "git+https://github.com/aiekick/ImGuiFileDialog"
        "git+https://github.com/btzy/nativefiledialog-extended"
        "target_link_libraries.patch")
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            '39459c2f5b67968ca9b743e335026dc280e340ed6e3e4091ac26afd7d709f290')

pkgver() {
  cd "${srcdir}/scopehal-apps"
  echo "0.0."`git describe --always`
}

prepare() {
    cd "$srcdir/scopehal-apps"

    echo "Initialize git submodule paths"
    git submodule init

    echo "Updating git submodule paths"
    git config submodule.lib.url "$srcdir/scopehal"
    git config submodule.doc.url "$srcdir/scopehal-docs"
    git config submodule.src/imgui.url "$srcdir/imgui"
    git config submodule.src/implot.url "$srcdir/implot"
    git config submodule.src/imgui-node-editor.url "$srcdir/imgui-node-editor"
    git config submodule.src/ImGuiFileDialog.url "$srcdir/ImGuiFileDialog"
    git config submodule.src/nativefiledialog-extended.url "$srcdir/nativefiledialog-extended"

    echo "Updating git submodules"
    git -c protocol.file.allow=always submodule update

    echo "Updating recursive submodules"
    cd "$srcdir/scopehal-apps/lib"
    git submodule init
    git config submodule.xptools.url "$srcdir/xptools"
    git config submodule.log.url "$srcdir/logtools"
    git config submodule.graphwidget.url "$srcdir/graphwidget"
    git config submodule.VkFFT.url "$srcdir/VkFFT"
    git -c protocol.file.allow=always submodule update

    echo "Patch to locate SPIRV tools"
    cd "$srcdir/scopehal-apps"
    patch -Nup1 -i "$srcdir"/target_link_libraries.patch || true
}

build() {
    cd "$srcdir/scopehal-apps"
    mkdir -p build
    cd build
    cmake .. \
      -DCMAKE_BUILD_TYPE=RELEASE \
      -DCMAKE_INSTALL_PREFIX=/usr
      # -DBUILD_DOCS=ON
    make
}

package() {
	cd "$srcdir/scopehal-apps/build"
	make DESTDIR="$pkgdir/" install
}
