
pkgname=scopehal-apps-git
pkgver=0.0.a00dc9c
pkgrel=1
pkgdesc="ngscopeclient and other client applications for libscopehal"
arch=('x86_64')
url="https://github.com/glscopeclient/scopehal-apps"
license=('BSD')
groups=()
depends=('gtkmm3' 'libsigc++' 'ffts' 'openmp' 'glfw' 'libvulkan.so' 'yaml-cpp' 'glew' 'catch2' 'spirv-tools' 'shaderc')
optdepends=('liblxi: LXI transport support'
            'libtirpc: LXI transport dependency'
            'linux-gpib: GPIB transport support')
makedepends=('cmake' 'git' 'vulkan-headers')
source=("git+https://github.com/glscopeclient/scopehal-apps.git"
        "git+https://github.com/glscopeclient/scopehal.git"
        "git+https://github.com/glscopeclient/xptools.git"
        "git+https://github.com/glscopeclient/logtools.git"
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
            '106a6a1873bbb8e0038e461f9be457c96c756f8e008a95729d3f5fbcaf8cc856')

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
    git config submodule.VkFFT.url "$srcdir/VkFFT"
    git -c protocol.file.allow=always submodule update

    echo "Patching to link SPIRV tools and libtirpc"
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
