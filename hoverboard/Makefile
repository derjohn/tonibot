.PHONY: firmware-container flash-container
HOVERFWDIR=$(shell pwd)/hoverboard-firmware-hack/build
HOVERFWFILE=hover.bin
MAKEFILEDIR=$(shell pwd)


### Firmware Build

firmware-submodule:
	git submodule update --init

firmware-container:
	cd firmware-container; docker build . -t firmware-container; cd ..

firmware:
	docker run -it --rm -v $(MAKEFILEDIR)/hoverboard-firmware-hack:/tmp firmware-container bash -c "cd /tmp; make"

### Flash

flash-container:
	cd flash-container; docker build . -t flash-container

stversion:
	docker run -it --privileged flash-container st-flash --version

stprobe:
	docker run -it --privileged flash-container st-info --probe

stflash:
	docker run -it --privileged -v $(HOVERFWDIR)/$(HOVERFWFILE):/$(HOVERFWFILE) flash-container st-flash --reset write /$(HOVERFWFILE) 0x8000000

stshell:
	docker run -it --privileged -v $(HOVERFWDIR)/$(HOVERFWFILE):/$(HOVERFWFILE) flash-container sh

unlock-1:
	docker run -it --privileged flash-container openocd -f /usr/local/share/openocd/scripts/interface/stlink-v2.cfg -f /usr/local/share/openocd/scripts/target/stm32f1x.cfg -c init -c "reset halt" -c "stm32f1x unlock 0"

unlock-2:
	docker run -it --privileged flash-container openocd -f /usr/local/share/openocd/scripts/interface/stlink-v2.cfg -f /usr/local/share/openocd/scripts/target/stm32f1x.cfg -c init -c "reset halt" -c "mww 0x40022004 0x45670123" -c "mww 0x40022004 0xCDEF89AB" -c "mww 0x40022008 0x45670123" -c "mww 0x40022008 0xCDEF89AB" -c "mww 0x40022010 0x220" -c "mww 0x40022010 0x260" -c "sleep 100" -c "mww 0x40022010 0x230" -c "mwh 0x1ffff800 0x5AA5" -c "sleep 1000" -c "mww 0x40022010 0x2220" -c "sleep 100" -c "mdw 0x40022010" -c "mdw 0x4002201c" -c "mdw 0x1ffff800" -c targets -c "halt" -c "stm32f1x unlock 0"
