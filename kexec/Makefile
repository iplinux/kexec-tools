#
# kexec (linux booting linux)
#
PURGATORY_HEX_C = kexec/purgatory.c

$(PURGATORY_HEX_C): $(PURGATORY) $(BIN_TO_HEX)
	$(MKDIR) -p $(@D)
	$(BIN_TO_HEX) purgatory < $(PURGATORY) > $@

KEXEC_SRCS =
KEXEC_GENERATED_SRCS =

KEXEC_SRCS += kexec/kexec.c
KEXEC_SRCS += kexec/ifdown.c
KEXEC_SRCS += kexec/kexec-elf.c
KEXEC_SRCS += kexec/kexec-elf-exec.c
KEXEC_SRCS += kexec/kexec-elf-core.c
KEXEC_SRCS += kexec/kexec-elf-rel.c
KEXEC_SRCS += kexec/kexec-elf-boot.c
KEXEC_SRCS += kexec/kexec-iomem.c
KEXEC_SRCS += kexec/crashdump.c
KEXEC_SRCS += kexec/crashdump-xen.c

KEXEC_GENERATED_SRCS += $(PURGATORY_HEX_C)

dist += kexec/Makefile $(KEXEC_SRCS) $(KEXEC_GENERATED_SRCS)	\
	kexec/crashdump-elf.c					\
	kexec/crashdump.h kexec/kexec-elf-boot.h		\
	kexec/kexec-elf.h kexec/kexec-sha256.h			\
	kexec/kexec-syscall.h kexec/kexec.h kexec/kexec.8

include $(srcdir)/kexec/arch/alpha/Makefile
include $(srcdir)/kexec/arch/arm/Makefile
include $(srcdir)/kexec/arch/i386/Makefile
include $(srcdir)/kexec/arch/ia64/Makefile
include $(srcdir)/kexec/arch/mips/Makefile
include $(srcdir)/kexec/arch/ppc/Makefile
include $(srcdir)/kexec/arch/ppc64/Makefile
include $(srcdir)/kexec/arch/s390/Makefile
include $(srcdir)/kexec/arch/sh/Makefile
include $(srcdir)/kexec/arch/x86_64/Makefile

KEXEC_SRCS += $($(ARCH)_KEXEC_SRCS)

KEXEC_OBJS = $(call objify, $(KEXEC_SRCS) $(KEXEC_GENERATED_SRCS))
KEXEC_DEPS = $(call depify, $(KEXEC_OBJS))

clean += $(KEXEC_OBJS) $(KEXEC_DEPS) $(KEXEC_GENERATED_SRCS) \
	 $(KEXEC) $(KEXEC_MANPAGE)

KEXEC = $(SBINDIR)/kexec
KEXEC_MANPAGE = $(MANDIR)/man8/kexec.8

-include $(KEXEC_DEPS)

$(KEXEC): LDFLAGS+=$(LIBS)
$(KEXEC): $(KEXEC_OBJS) $(UTIL_LIB)
	@$(MKDIR) -p $(@D)
	$(LINK.o) -o $@ $^

$(KEXEC): CPPFLAGS+=-I$(srcdir)/kexec/arch/$(ARCH)/include

$(KEXEC_MANPAGE): kexec/kexec.8
	@$(MKDIR) -p     $(MANDIR)/man8
	cp kexec/kexec.8 $(KEXEC_MANPAGE)
echo::
	@echo "KEXEC_SRCS $(KEXEC_SRCS)"
	@echo "KEXEC_DEPS $(KEXEC_DEPS)"
	@echo "KEXEC_OBJS $(KEXEC_OBJS)"

