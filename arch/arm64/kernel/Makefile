#
# Makefile for the linux kernel.
#

CPPFLAGS_vmlinux.lds	:= -DTEXT_OFFSET=$(TEXT_OFFSET)
AFLAGS_head.o		:= -DTEXT_OFFSET=$(TEXT_OFFSET)
CFLAGS_efi-stub.o 	:= -DTEXT_OFFSET=$(TEXT_OFFSET)
CFLAGS_armv8_deprecated.o := -I$(src)

KASAN_SANITIZE_efi-stub.o	:= n

CFLAGS_REMOVE_ftrace.o = -pg
CFLAGS_REMOVE_insn.o = -pg
CFLAGS_REMOVE_return_address.o = -pg

# Object file lists.
arm64-obj-y		:= debug-monitors.o entry.o irq.o fpsimd.o		\
			   entry-fpsimd.o process.o ptrace.o setup.o signal.o	\
			   sys.o stacktrace.o time.o traps.o io.o vdso.o	\
			   hyp-stub.o psci.o cpu_ops.o insn.o	\
			   return_address.o cpuinfo.o cpu_errata.o		\
			   cpufeature.o alternative.o psci-call.o		\
			   smp.o smp_spin_table.o topology.o cacheinfo.o

arm64-obj-$(CONFIG_COMPAT)		+= sys32.o signal32.o	\
					   sys_compat.o				\
					   ../../arm/kernel/opcodes.o
ifneq ($(CONFIG_VDSO32),y)
arm64-obj-$(CONFIG_COMPAT)		+= sigreturn32.o
endif
arm64-obj-$(CONFIG_KUSER_HELPERS)	+= kuser32.o
arm64-obj-$(CONFIG_FUNCTION_TRACER)	+= ftrace.o entry-ftrace.o
arm64-obj-$(CONFIG_MODULES)		+= arm64ksyms.o module.o
arm64-obj-$(CONFIG_PERF_EVENTS)		+= perf_regs.o
arm64-obj-$(CONFIG_HW_PERF_EVENTS)	+= perf_event.o perf_trace_counters.o 	 \
					   perf_trace_user.o
ifdef CONFIG_HW_PERF_EVENTS
arm64-obj-$(CONFIG_DEBUG_FS)		+= perf_debug.o
endif
arm64-obj-$(CONFIG_HAVE_HW_BREAKPOINT)	+= hw_breakpoint.o
arm64-obj-$(CONFIG_ARM64_CPU_SUSPEND)	+= sleep.o suspend.o
arm64-obj-$(CONFIG_CPU_IDLE)		+= cpuidle.o
arm64-obj-$(CONFIG_JUMP_LABEL)		+= jump_label.o
arm64-obj-$(CONFIG_KGDB)		+= kgdb.o
arm64-obj-$(CONFIG_EFI)			+= efi.o efi-stub.o efi-entry.o
arm64-obj-$(CONFIG_PCI)			+= pci.o
arm64-obj-$(CONFIG_ARMV8_DEPRECATED)	+= armv8_deprecated.o
arm64-obj-$(CONFIG_MSM_APP_API)		+= app_api.o
arm64-obj-$(CONFIG_MSM_APP_SETTINGS)	+= app_setting.o
arm64-obj-$(CONFIG_EARLY_PRINTK)	+= early_printk.o

ifeq ($(CONFIG_KVM),y)
arm64-obj-$(CONFIG_HARDEN_BRANCH_PREDICTOR)	+= bpi.o
endif

obj-y					+= $(arm64-obj-y) vdso/
obj-$(CONFIG_VDSO32)			+= vdso32/
obj-m					+= $(arm64-obj-m)
head-y					:= head.o
extra-y					:= $(head-y) vmlinux.lds
