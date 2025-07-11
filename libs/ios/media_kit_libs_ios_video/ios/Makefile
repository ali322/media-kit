all: Frameworks/*.xcframework Frameworks/.symlinks

MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=a7bd1a9037ad41877ebd7fa16fe78180ba9d90496f169e18c935fd75b8b8edb2
# MPV_XCFRAMEWORKS_VERSION=v0.6.0
# MPV_XCFRAMEWORKS_SHA256SUM=a95bc18508af26136b8a408341c05b5585d644ec013f00ac07db09d2e28d36ae

TAR=tar
TAR_WILDCARDS_FLAG := $(shell ${TAR} --version 2>&1 | grep -q 'GNU' && echo "--wildcards" || echo "")

SED=sed
SED_INPLACE_FLAG := $(shell ${SED} --version 2>&1 | grep -q 'GNU' && echo "" || echo "''")

.cache/xcframeworks/libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-ios-universal.tar.gz:
	mkdir -p .cache/xcframeworks
	rm -f .cache/xcframeworks/*.tmp .cache/xcframeworks/*-ios-universal.tar.gz
	curl -L \
		https://github.com/media-kit/libmpv-darwin-build/releases/download/${MPV_XCFRAMEWORKS_VERSION}/libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_ios-universal-video-full.tar.gz \
		-o .cache/xcframeworks/libmpv.tar.gz.tmp
	shasum -a 256 -c <<< '${MPV_XCFRAMEWORKS_SHA256SUM}  .cache/xcframeworks/libmpv.tar.gz.tmp'
	mv .cache/xcframeworks/libmpv.tar.gz.tmp .cache/xcframeworks/libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-ios-universal.tar.gz
	touch .cache/xcframeworks/libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-ios-universal.tar.gz

.cache/xcframeworks/libmpv-xcframeworks-ios-universal.tar.gz: .cache/xcframeworks/libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-ios-universal.tar.gz
	rm -f .cache/xcframeworks/libmpv-xcframeworks-ios-universal.tar.gz
	ln -s libmpv-xcframeworks-${MPV_XCFRAMEWORKS_VERSION}-ios-universal.tar.gz .cache/xcframeworks/libmpv-xcframeworks-ios-universal.tar.gz

Frameworks/*.xcframework: .cache/xcframeworks/libmpv-xcframeworks-ios-universal.tar.gz
	mkdir -p Frameworks
	rm -rf Frameworks/*.xcframework
	${TAR} ${TAR_WILDCARDS_FLAG} -xvf .cache/xcframeworks/libmpv-xcframeworks-ios-universal.tar.gz --strip-components 1 -C Frameworks
	touch Frameworks/*.xcframework

Frameworks/.symlinks: Frameworks/*.xcframework
	rm -rf Frameworks/.symlinks
	mkdir -p Frameworks/.symlinks/mpv
	${SED} -i ${SED_INPLACE_FLAG} 's/\r$$//g' create_framework_symlinks.sh # remove CRLF line terminator added by Flutter packaging (https://github.com/media-kit/media-kit/issues/338)
	sh create_framework_symlinks.sh Frameworks/Mpv.xcframework Frameworks/.symlinks/mpv

.PHONY: clean
clean:
	rm -rf .cache
	rm -f ./Headers/mpv/*.h
	rm -rf ./Frameworks/*.xcframework
