prefix := $(shell rtai-config --prefix)

ifeq ($(prefix),)
$(error Please add <rtai-install>/bin to your PATH variable)
endif

CC = $(shell rtai-config --cc)
LXRT_CFLAGS = $(shell rtai-config --lxrt-cflags)
LXRT_CFLAGS += -D_GNU_SOURCE
LXRT_LDFLAGS = $(shell rtai-config --lxrt-ldflags)

all:: cmd insn aocmd rminsn rmcmd rmaocmd rmtest

ifneq ($(findstring 2.6.,$(shell rtai-config --linux-version 2>/dev/null)),)

LINUX_DIR = $(shell rtai-config --linux-dir)

all::
	@if test -e $(LINUX_DIR)/Module.symvers; then mv -f $(LINUX_DIR)/Module.symvers $(LINUX_DIR)/__Module.symvers; fi; if test -e $(LINUX_DIR)/vmlinux; then mv -f $(LINUX_DIR)/vmlinux $(LINUX_DIR)/__vmlinux; fi; \
	$(MAKE) -C $(LINUX_DIR) CC=$(CC) SUBDIRS=$$PWD V=$(V) modules \
&& if test -e $(LINUX_DIR)/__Module.symvers; then mv -f $(LINUX_DIR)/__Module.symvers $(LINUX_DIR)/Module.symvers; fi && if test -e $(LINUX_DIR)/__vmlinux; then mv -f $(LINUX_DIR)/__vmlinux $(LINUX_DIR)/vmlinux; fi

clean::
	$(RM) $(LINUX_DIR)/.tmp_versions/*_rt.mod *.o *.ko *.mod.c .*.cmd
else

MODULE_CFLAGS = $(shell rtai-config --module-cflags)

all:: rt_agent.o

kcomedi.o: kcomedi.c
	$(CC) $(MODULE_CFLAGS) -c $<

clean::
	$(RM) *.o

endif

cmd: cmd.c
	$(CC) $(LXRT_CFLAGS) -o $@ $< $(LXRT_LDFLAGS) -lkcomedilxrt

insn: insn.c
	$(CC) $(LXRT_CFLAGS) -o $@ $< $(LXRT_LDFLAGS) -lkcomedilxrt -lm

aocmd: aocmd.c
	$(CC) $(LXRT_CFLAGS) -o $@ $< $(LXRT_LDFLAGS) -lkcomedilxrt -lm

rminsn: rminsn.c
	$(CC) $(LXRT_CFLAGS) -o $@ $< $(LXRT_LDFLAGS) -lkcomedilxrt -lm

rmcmd: rmcmd.c
	$(CC) $(LXRT_CFLAGS) -o $@ $< $(LXRT_LDFLAGS) -lkcomedilxrt -lm

rmaocmd: rmaocmd.c
	$(CC) $(LXRT_CFLAGS) -o $@ $< $(LXRT_LDFLAGS) -lkcomedilxrt -lm

rmtest: rmtest.c
	$(CC) $(LXRT_CFLAGS) -o $@ $< $(LXRT_LDFLAGS) -lkcomedilxrt -lm

clean::
	rm -f *.o cmd insn aocmd rminsn rmcmd rmaocmd rmtest modules.order Module.symvers

.PHONY: clean
