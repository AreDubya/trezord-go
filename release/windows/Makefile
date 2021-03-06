PLATFORM  = win
BITS      = 32
NAME32 = trezord-go-windows-4.0-386.exe
VOL_MOUNT = -v $(shell pwd):/release
IMAGETAG  = trezord-go-build-env-$(PLATFORM)

ifeq ($(LOCAL_BUILD),1)
IMPORT_PATH = ../..
else
IMPORT_PATH = github.com/trezor/trezord-go
endif

all: clean .package

clean:
	$(info Cleaning...)
	rm -rf build

.binary:
	$(info Building with xgo ...)
	mkdir -p build
	xgo -ldflags="-H=windowsgui" -targets=windows/386 $(IMPORT_PATH)
	mv -f $(NAME32) build/trezord.exe
	cp ../../VERSION build

.package: .binary .docker-image
	$(info Packaging ...)
	docker run -i -t $(VOL_MOUNT) $(IMAGETAG) /release/release.sh $(PLATFORM)$(BITS)

.docker-image:
	$(info Preparing docker image ...)
	docker build -t $(IMAGETAG) .

shell: .docker-image
	docker run -i -t $(VOL_MOUNT) $(IMAGETAG) /bin/bash
