#
# The stub may be linked into the kernel proper or into a separate boot binary,
# but in either case, it executes before the kernel does (with MMU disabled) so
# things like ftrace and stack-protector are likely to cause trouble if left
# enabled, even if doing so doesn't break the build.
#
cflags-$(CONFIG_X86_32)		:= -march=i386
cflags-$(CONFIG_X86_64)		:= -mcmodel=small
cflags-$(CONFIG_X86)		+= -m$(BITS) -D__KERNEL__ $(LINUX_INCLUDE) -O2 \
				   -fPIC -fno-strict-aliasing -mno-red-zone \
				   -mno-mmx -mno-sse -DDISABLE_BRANCH_PROFILING

cflags-$(CONFIG_ARM64)		:= $(subst -pg,,$(KBUILD_CFLAGS))
cflags-$(CONFIG_ARM)		:= $(subst -pg,,$(KBUILD_CFLAGS)) \
				   -fno-builtin -fpic \
				   $(call cc-option,-mno-single-pic-base)

KBUILD_CFLAGS			:= $(cflags-y) \
				   $(call cc-option,-ffreestanding) \
				   $(call cc-option,-fno-stack-protector) \
				   $(DISABLE_LTO)

GCOV_PROFILE			:= n
KASAN_SANITIZE			:= n

# Prevents link failures: __sanitizer_cov_trace_pc() is not linked in.
KCOV_INSTRUMENT			:= n

lib-y				:= efi-stub-helper.o
lib-$(CONFIG_EFI_ARMSTUB)	+= arm-stub.o fdt.o

CFLAGS_fdt.o			+= -I$(srctree)/scripts/dtc/libfdt/
