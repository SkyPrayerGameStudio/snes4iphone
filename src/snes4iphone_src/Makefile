VERSION=2.0
COPT = -F/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS${VERSION}.sdk/System/Library/Frameworks -F/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS${VERSION}.sdk/System/Library/PrivateFrameworks -I../../ -I../../Classes/ -I/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS3.0.sdk/usr/lib/gcc/arm-apple-darwin9/4.2.1/include -isysroot /Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS${VERSION}.sdk  -L/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS${VERSION}.sdk/usr/lib
COPT += -march=armv6 -miphoneos-version-min=${VERSION} -I. -O3 -D__IPHONE__ -D_SNESPPC -DASM_SPC700 -DZLIB -DUNZIP_SUPPORT -DOLD_COLOUR_BLENDING -DUSE_SA1 -DSUPER_FX -DLSB_FIRST -DCPU_SHUTDOWN -DVAR_CYCLES
COPT += -fnested-functions -funsigned-char -ffast-math -fexpensive-optimizations -ftemplate-depth-36 -mstructure-size-boundary=32 -falign-functions=32 -falign-loops -falign-labels -falign-jumps -finline -finline-functions -fno-builtin -fno-common -fomit-frame-pointer -fpeel-loops -fstrength-reduce -funroll-loops -fstrict-aliasing 
GCC = /Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/arm-apple-darwin9-gcc-4.2.1
GXX = /Developer/Platforms/iPhoneOS.platform/Developer/usr/bin/arm-apple-darwin9-g++-4.2.1
#-DSPC700_SHUTDOWN
#-DRC_OPTIMIZED 
# SNES stuff (c-based)
# cpuops.o sa1cpu.o 
OBJS = apu.o c4.o c4emu.o cheats.o cheats2.o clip.o cpu.o cpuexec.o data.o 
OBJS += dma.o dsp1.o fxemu.o fxinst.o gfx.o globals.o input.o ioapi.o loadzip.o memmap.o menu.o ppu.o  
OBJS += sa1.o sdd1.o sdd1emu.o snapshot.o soundux.o spc700.o srtc.o tile.o unzip.o os9x_asm_cpu.o os9x_65c816.o spc700a.o 
#
# ASM CPU Core, ripped from Yoyo's OpenSnes9X
#
#OBJS += os9x_65c816.o 
# os9x_asm_cpu.o spc700a.o
#
# and some asm from LJP...
#
# OBJS += m3d_func.o
#
# Dave's minimal SDK
#
OBJS += iphone_sdk.o iphone_menutile.o iphone_highlightbar.o iphone_menu_header.o 
OBJS += ../../main.o ../../NowPlayingController.o ../../Classes/helpers.o   ../../Classes/OptionsController.o ../../Classes/RecentController.o ../../Classes/RomController.o ../../Classes/SaveStatesController.o ../../Classes/ShoutOutAppDelegate.o ../../Classes/SOApplication.o ../../Classes/TabBar.o ../../Classes/WebBrowserViewController.o


#
# and the glue code that sticks it all together :)
#
OBJS += main.o

# Inopia's menu system, hacked for the GP2X under rlyeh's sdk
PRELIBS = -multiply_defined suppress -lobjc -fobjc-exceptions \
	        -lpthread \
          -framework CoreFoundation \
          -framework Foundation \
          -framework UIKit \
          -framework QuartzCore \
          -framework CoreGraphics \
          -framework CoreSurface \
          -framework CoreLocation \
          -framework AudioToolbox \
          -framework GraphicsServices \
          -framework OpenGLES \
          -framework AddressBook -lsqlite3 -framework SystemConfiguration \
          -lz \
          -framework WebKit \
          -allow_stack_execute

all: pocketsnes
clean: tidy

.c.o:
	$(GCC) $(COPT) -c $< -o $@

.cpp.o:
	$(GXX) $(COPT) -c $< -o $@

%.o: %.m
	$(GCC) ${COPT} -c $< -o $@

%.o: %.S
	$(GCC) ${COPT} -c $< -o $@

%.o: %.s
	$(GCC) ${COPT} -c $< -o $@

# make seems to lowercase the extensions, so files with '.S' end up being passed to the compiler as '.s', which means thousands of errors.
# this is a small workaround. 

#spc700a.o: spc700a.s
#	$(GCC) $(COPT) -c $< -o $@

#os9x_65c816.o: os9x_65c816.s
#	$(GCC) $(COPT) -c $< -o $@

#osnes9xgp_asmfunc.o: osnes9xgp_asmfunc.s
#	$(GCC) $(COPT) -c $< -o $@

#m3d_func.o: m3d_func.S
#	$(GCC) $(COPT) -c $< -o $@

#spc_decode.o: spc_decode.s
#	$(GCC) $(COPT) -c $< -o $@

#memset.o: memset.s
#	$(GCC) $(COPT) -c $< -o $@

#memcpy.o: memcpy.s
#	$(GCC) $(COPT) -c $< -o $@

#dspMixer.o: dspMixer.s
#	$(GCC) $(COPT) -c $< -o $@

pocketsnesd: $(OBJS)
	$(GXX) $(COPT) $(OBJS) $(PRELIBS) -o $@

pocketsnes: pocketsnesd
	$(STRIP) pocketsnesd -o snes4iphone
	
tidy:
	rm *.o ../../*.o ../../Classes/*.o
