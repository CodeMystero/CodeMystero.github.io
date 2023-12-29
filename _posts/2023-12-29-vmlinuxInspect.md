---
layout: single

title: "[Linux] ELF file analysis in assembly"
excerpt: "using objdump, System.map to look into vmlinux"

categories:
  - Linux server

tag: [C/C++, linux, kernel] 

toc: true
toc_sticky: true

author_profile: false
sidebar:
  nav: "docs"
---

![header](https://capsule-render.vercel.app/api?type=rect&color=20:660099,100:E2231A)



## ELF (Executable and Linkable FOrmat)

"The binary file format used in UNIX and its derivatives for various types of files such as executable files, object files, shared libraries, and even core dump files. It possesses platform independence."

## binary utility 

program that to handle object format file

|||
|---|---|
|objdump|library, convert ELF file into assembly|
|as|assembler - assembly to machine language|
|ld|linker|
|addr2line|print address into file and line|
|nm|print symbol of obj file|
|readelf|print contents of ELF file|

## mkdir and cp vmlinux file to test folder 

```bash 
root@raspberrypi:/project # mkdir objview
root@raspberrypi:/project # ls
linuxSrc  objview
root@raspberrypi:/project # cd objview
root@raspberrypi:/project/objview # cp ../linuxSrc/out/vmlinux ./
root@raspberrypi:/project/objview # ls
vmlinux
```

vmlinux is copied in new folder objview so that original file can be protected. vmlinux is one of ELF file. below shows vmlinux header file 20 lines from the beginning

```bash
root@raspberrypi:/project/objview # objdump -x vmlinux | head -n 20

vmlinux:     file format elf64-littleaarch64
vmlinux
architecture: aarch64, flags 0x00000150:
HAS_SYMS, DYNAMIC, D_PAGED
start address 0xffffffc008000000

Program Header:
    LOAD off    0x0000000000010000 vaddr 0xffffffc008000000 paddr 0xffffffc008000000 align 2**16
         filesz 0x000000000167f200 memsz 0x000000000178ebc8 flags rwx
    NOTE off    0x0000000001028448 vaddr 0xffffffc009018448 paddr 0xffffffc009018448 align 2**2
         filesz 0x0000000000000054 memsz 0x0000000000000054 flags r--
   STACK off    0x0000000000000000 vaddr 0x0000000000000000 paddr 0x0000000000000000 align 2**4
         filesz 0x0000000000000000 memsz 0x0000000000000000 flags rw-
private flags = 0x0:

Sections:
Idx Name          Size      VMA               LMA               File off  Algn
  0 .head.text    00010000  ffffffc008000000  ffffffc008000000  00010000  2**16
                  CONTENTS, ALLOC, LOAD, READONLY, CODE

/*                ... skipped                  */
```

> start address 0xffffffc008000000 <br>code starts from the start adress (64bits -> 16 hexadecimal numbers)

***It is hard to read those format of files. we can see it intuitively in assembly interpreted by objdump by searching address using system map***


### System.map

```bash
root@raspberrypi:/project/objview # ls
vmlinux
root@raspberrypi:/project/objview # cp ../linuxSrc/out/System.map ./
root@raspberrypi:/project/objview # ls
System.map  vmlinux
```
now we will scratch address of schedule 

```bash 
root@raspberrypi:/project/objview # cat System.map | grep " schedule"
ffffffc0080acb90 T schedule_on_each_cpu
ffffffc0080ca130 T schedule_tail
ffffffc0080ca5e0 T scheduler_tick
ffffffc008119450 t schedule_page_work_fn
ffffffc008119490 t schedule_delayed_monitor_work
ffffffc008759020 T schedule_console_callback
ffffffc008bfac00 T schedule
ffffffc008bfb090 T schedule_idle
ffffffc008bfb0e4 T schedule_preempt_disabled
ffffffc008c019f0 T schedule_timeout
ffffffc008c01bb0 T schedule_timeout_interruptible
ffffffc008c01be0 T schedule_timeout_killable
ffffffc008c01c10 T schedule_timeout_uninterruptible
ffffffc008c01c40 T schedule_timeout_idle
ffffffc008c01c80 T schedule_hrtimeout_range_clock
ffffffc008c01db0 T schedule_hrtimeout_range
ffffffc008c01de0 T schedule_hrtimeout
ffffffc00946d938 D scheduler_running
```
we can directly search schrdule address by :/schedule using vim, then we can find start and finish address of schedule as below

```cpp
ffffffc008bfac00 T schedule
ffffffc008bfac00 T yield
```

### objdump

```bash
root@raspberrypi:/project/objview # objdump --start-address=0xffffffc008bfac00 --stop-address=0xffffffc008bfad04 -d vmlinux > a.s
root@raspberrypi:/project/objview # vi a.s
```
crawl the vmlinux data within the given address range and save it to the file a.s.

```vim

vmlinux:     file format elf64-littleaarch64


Disassembly of section .text:

ffffffc008bfac00 <schedule>:
ffffffc008bfac00:	d503201f 	nop
ffffffc008bfac04:	d503201f 	nop
ffffffc008bfac08:	d503233f 	paciasp
ffffffc008bfac0c:	a9be7bfd 	stp	x29, x30, [sp, #-32]!
ffffffc008bfac10:	910003fd 	mov	x29, sp
ffffffc008bfac14:	a90153f3 	stp	x19, x20, [sp, #16]
ffffffc008bfac18:	d5384114 	mrs	x20, sp_el0
ffffffc008bfac1c:	b9401a80 	ldr	w0, [x20, #24]
ffffffc008bfac20:	34000160 	cbz	w0, ffffffc008bfac4c <schedule+0x4c>
ffffffc008bfac24:	b9402e80 	ldr	w0, [x20, #44]
ffffffc008bfac28:	721c041f 	tst	w0, #0x30
ffffffc008bfac2c:	540003c1 	b.ne	ffffffc008bfaca4 <schedule+0xa4>  // b.any
ffffffc008bfac30:	d5384100 	mrs	x0, sp_el0
ffffffc008bfac34:	b9401800 	ldr	w0, [x0, #24]
ffffffc008bfac38:	37600500 	tbnz	w0, #12, ffffffc008bfacd8 <schedule+0xd8>
ffffffc008bfac3c:	f9445e80 	ldr	x0, [x20, #2232]
ffffffc008bfac40:	b4000060 	cbz	x0, ffffffc008bfac4c <schedule+0x4c>
ffffffc008bfac44:	52800021 	mov	w1, #0x1                   	// #1
ffffffc008bfac48:	97e7dc63 	bl	ffffffc0085f1dd4 <__blk_flush_plug>
ffffffc008bfac4c:	d5384113 	mrs	x19, sp_el0
ffffffc008bfac50:	b9400a60 	ldr	w0, [x19, #8]
ffffffc008bfac54:	11000400 	add	w0, w0, #0x1
ffffffc008bfac58:	b9000a60 	str	w0, [x19, #8]
ffffffc008bfac5c:	52800000 	mov	w0, #0x0                   	// #0
ffffffc008bfac60:	97fffd90 	bl	ffffffc008bfa2a0 <__schedule>
ffffffc008bfac64:	b9400a60 	ldr	w0, [x19, #8]
ffffffc008bfac68:	51000400 	sub	w0, w0, #0x1
ffffffc008bfac6c:	b9000a60 	str	w0, [x19, #8]
ffffffc008bfac70:	f9400260 	ldr	x0, [x19]
ffffffc008bfac74:	721f001f 	tst	w0, #0x2
ffffffc008bfac78:	54fffec1 	b.ne	ffffffc008bfac50 <schedule+0x50>  // b.any
ffffffc008bfac7c:	b9402e80 	ldr	w0, [x20, #44]
ffffffc008bfac80:	721c041f 	tst	w0, #0x30
ffffffc008bfac84:	54000080 	b.eq	ffffffc008bfac94 <schedule+0x94>  // b.none
ffffffc008bfac88:	36280160 	tbz	w0, #5, ffffffc008bfacb4 <schedule+0xb4>
ffffffc008bfac8c:	aa1403e0 	mov	x0, x20
ffffffc008bfac90:	97d2c75c 	bl	ffffffc0080aca00 <wq_worker_running>
ffffffc008bfac94:	a94153f3 	ldp	x19, x20, [sp, #16]
ffffffc008bfac98:	a8c27bfd 	ldp	x29, x30, [sp], #32
ffffffc008bfac9c:	d50323bf 	autiasp
ffffffc008bfaca0:	d65f03c0 	ret
ffffffc008bfaca4:	36280140 	tbz	w0, #5, ffffffc008bfaccc <schedule+0xcc>
ffffffc008bfaca8:	aa1403e0 	mov	x0, x20
ffffffc008bfacac:	97d2c77a 	bl	ffffffc0080aca94 <wq_worker_sleeping>
ffffffc008bfacb0:	17ffffe0 	b	ffffffc008bfac30 <schedule+0x30>
ffffffc008bfacb4:	aa1403e0 	mov	x0, x20
ffffffc008bfacb8:	97e90f06 	bl	ffffffc00863e8d0 <io_wq_worker_running>
ffffffc008bfacbc:	a94153f3 	ldp	x19, x20, [sp, #16]
ffffffc008bfacc0:	a8c27bfd 	ldp	x29, x30, [sp], #32
ffffffc008bfacc4:	d50323bf 	autiasp
ffffffc008bfacc8:	d65f03c0 	ret
ffffffc008bfaccc:	aa1403e0 	mov	x0, x20
ffffffc008bfacd0:	97e90f20 	bl	ffffffc00863e950 <io_wq_worker_sleeping>
ffffffc008bfacd4:	17ffffd7 	b	ffffffc008bfac30 <schedule+0x30>
ffffffc008bfacd8:	b0005261 	adrp	x1, ffffffc009647000 <event_class_rpc_xdr_alignment+0x28>
ffffffc008bfacdc:	91304021 	add	x1, x1, #0xc10
ffffffc008bface0:	39401420 	ldrb	w0, [x1, #5]
ffffffc008bface4:	35fffac0 	cbnz	w0, ffffffc008bfac3c <schedule+0x3c>
ffffffc008bface8:	52800022 	mov	w2, #0x1                   	// #1
ffffffc008bfacec:	90001700 	adrp	x0, ffffffc008eda000 <kallsyms_token_index+0x10208>
ffffffc008bfacf0:	911c8000 	add	x0, x0, #0x720
ffffffc008bfacf4:	39001422 	strb	w2, [x1, #5]
ffffffc008bfacf8:	97d23867 	bl	ffffffc008088e94 <__warn_printk>
ffffffc008bfacfc:	d4210000 	brk	#0x800
ffffffc008bfad00:	17ffffcf 	b	ffffffc008bfac3c <schedule+0x3c>
```

>Now we can see the assembly code for the given address.

