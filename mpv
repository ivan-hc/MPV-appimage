#!/bin/sh

APP=mpv

mkdir tmp;
cd tmp;

# DOWNLOADING THE DEPENDENCIES
wget https://github.com/AppImage/AppImageKit/releases/download/continuous/appimagetool-$(uname -m).AppImage -O appimagetool
wget https://raw.githubusercontent.com/ivan-hc/AM-application-manager/main/tools/pkg2appimage
chmod a+x ./appimagetool ./pkg2appimage

# CREATING THE APPIMAGE: APPDIR FROM A RECIPE...
PREVIOUSLTS=$(wget -q https://releases.ubuntu.com/ -O - | grep class | grep LTS | grep -m2 href | tail -n1 | sed -n 's/.*href="\([^"]*\).*/\1/p' | rev| cut -c 2- | rev)
LIBPLACEBO=$(wget -q https://deb-multimedia.org/dists/unstable/main/binary-amd64/ -O - | grep libplacebo | tail -1 | grep -o -P '(?<=package">).*(?=</a>)')
LIBPLACEBOLATEST=$(wget -q https://deb-multimedia.org/pool/main/libp/libplacebo-dmo/ -O - | grep $LIBPLACEBO | grep amd64 | grep -o -P '(?<=deb">).*(?=</a>)')
LIBSHADERC=$(wget -q http://ftp.de.debian.org/debian/pool/main/s/shaderc/ -O - | grep libshaderc1_ | grep amd64 | grep -o -P '(?<=deb">).*(?=</a>)')
LIBSHADERCDEV=$(wget -q http://ftp.de.debian.org/debian/pool/main/s/shaderc/ -O - | grep libshaderc-dev | grep amd64 | grep -o -P '(?<=deb">).*(?=</a>)')
LIBILBC=$(wget -q https://www.deb-multimedia.org/pool/main/libi/libilbc-dmo/ -O - | grep libilbc | grep amd64 | tail -1 | grep -o -P '(?<=deb">).*(?=</a>)')
LIBVMAF=$(wget -q https://www.deb-multimedia.org/pool/main/v/vmaf-dmo/ -O - | grep libvmaf | grep amd64 | tail -1 | grep -o -P '(?<=deb">).*(?=</a>)')

libvmaf
echo "app: mpv
binpatch: true

ingredients:
  dist: $PREVIOUSLTS
  package: mpv
  sources:
    - deb http://archive.ubuntu.com/ubuntu $PREVIOUSLTS main universe
    - deb http://security.ubuntu.com/ubuntu $PREVIOUSLTS-security main universe
  ppas:
    - savoury1/ffmpeg6
    - savoury1/mpv
    - savoury1/build-tools
    - savoury1/backports
    - savoury1/graphics
    - savoury1/backports
    - savoury1/python
    - savoury1/fonts
  script:
    - wget https://deb-multimedia.org/pool/main/libp/libplacebo-dmo/$LIBPLACEBOLATEST
    - wget http://ftp.de.debian.org/debian/pool/main/s/shaderc/$LIBSHADERC
    - wget http://ftp.de.debian.org/debian/pool/main/s/shaderc/$LIBSHADERCDEV
    - wget https://www.deb-multimedia.org/pool/main/libi/libilbc-dmo/$LIBILBC
    - wget https://deb-multimedia.org/pool/main/v/vmaf-dmo/$LIBVMAF
  packages:
    - mpv
    - libstdc++6
    - $LIBPLACEBO
    - libretro-snes9x
    - libshaderc1
    - libshaderc-dev" >> recipe.yml;

./pkg2appimage ./recipe.yml;

# ...DOWNLOADING LIBUNIONPRELOAD...
wget https://github.com/project-portable/libunionpreload/releases/download/amd64/libunionpreload.so
chmod a+x libunionpreload.so
mv ./libunionpreload.so ./$APP/$APP.AppDir/

# ...REPLACING THE EXISTING APPRUN WITH A CUSTOM ONE...
rm -R -f ./$APP/$APP.AppDir/AppRun
cat >> ./$APP/$APP.AppDir/AppRun << 'EOF'
#!/bin/sh
HERE="$(dirname "$(readlink -f "${0}")")"
export UNION_PRELOAD="${HERE}"
export LD_PRELOAD="${HERE}/libunionpreload.so"
export PATH="${HERE}"/usr/bin/:"${HERE}"/usr/sbin/:"${HERE}"/usr/games/:"${PATH}"
export LD_LIBRARY_PATH=/lib/:/lib64/:/usr/lib/:/usr/lib/x86_64-linux-gnu/:"${HERE}"/usr/lib/:"${HERE}"/usr/lib/x86_64-linux-gnu/:"${HERE}"/lib/:"${HERE}"/lib/x86_64-linux-gnu/:"${LD_LIBRARY_PATH}"
export PYTHONPATH="${HERE}"/usr/share/python3/:"${PYTHONPATH}"
export XDG_DATA_DIRS="${HERE}"/usr/share/:"${XDG_DATA_DIRS}"
export MPV_CONFIG_PATH=$HERE/etc/mpv/:$MPV_CONFIG_PATH
exec ${HERE}/usr/bin/mpv "$@"
EOF
chmod a+x ./$APP/$APP.AppDir/AppRun
mv ./AppRun ./$APP/$APP.AppDir

# ...IMPORT THE LAUNCHER AND THE ICON TO THE APPDIR (uncomment if not available)...
#cp ./$APP/$APP.AppDir/usr/share/icons/hicolor/22x22/apps/* ./$APP/$APP.AppDir/ 2>/dev/null
#cp ./$APP/$APP.AppDir/usr/share/icons/hicolor/24x24/apps/* ./$APP/$APP.AppDir/ 2>/dev/null
#cp ./$APP/$APP.AppDir/usr/share/icons/hicolor/32x32/apps/* ./$APP/$APP.AppDir/ 2>/dev/null
#cp ./$APP/$APP.AppDir/usr/share/icons/hicolor/48x48/apps/* ./$APP/$APP.AppDir/ 2>/dev/null
#cp ./$APP/$APP.AppDir/usr/share/icons/hicolor/64x64/apps/* ./$APP/$APP.AppDir/ 2>/dev/null
#cp ./$APP/$APP.AppDir/usr/share/icons/hicolor/128x128/apps/* ./$APP/$APP.AppDir/ 2>/dev/null
#cp ./$APP/$APP.AppDir/usr/share/icons/hicolor/256x256/apps/* ./$APP/$APP.AppDir/ 2>/dev/null
#cp ./$APP/$APP.AppDir/usr/share/icons/hicolor/512x512/apps/* ./$APP/$APP.AppDir/ 2>/dev/null
cp ./$APP/$APP.AppDir/usr/share/icons/hicolor/scalable/apps/* ./$APP/$APP.AppDir/ 2>/dev/null
cp ./$APP/$APP.AppDir/usr/share/applications/$APP* ./$APP/$APP.AppDir/ 2>/dev/null

# ...EXPORT THE APPDIR TO AN APPIMAGE!
ARCH=x86_64 ./appimagetool -n ./$APP/$APP.AppDir
cd ..;
mv ./tmp/*.AppImage .
