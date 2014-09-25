#makefile for dram_freq_entry.code

INCLUDE   = -I. \
	-I$(KDIR)/include \
	-I$(KDIR)/arch/arm/mach-sun7i/include \
	-I$(KDIR)/arch/arm/plat-sunxi/include \
	-I$(KDIR)/arch/arm/include \
	-I$(KDIR)/arch/arm/mach-sun7i/pm

DRAM_OBJ =  dram_freq_entry.o       \
			./../standby/mem_serial.o		\
			./../standby/mem_printk.o  		\
			./../standby/mem_divlibc.o		\
			./../standby/mem_divlib.o	    \
			./../standby/mem_delay.o        \
			./../standby/mem_timing.o       \
			./../standby/mem_int.o			\
			./../standby/mem_mmu_pc_asm.o   \
			./../standby/dram.o      	\
			./../standby/dram_init.o       

CC = $(CROSS_COMPILE)gcc

CFLAGS = -g -c -nostdlib -march=armv7-a -marm -D__STANDBY_MODULE__ -fno-unwind-tables -fno-asynchronous-unwind-tables -mlittle-endian -O2 --min_array_alignment=4 --no_unaligned_access
CFLAGS_DRAM_VADDR = -D DRAM_USE_VA_ADDR

LD_FLAGS = -static --gc-sections

all:
	$(CC) $(INCLUDE) $(CFLAGS) -e dram_freq_main dram_freq_entry.c           		-o dram_freq_entry.o
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) -O0 ./../standby/dram.c        -o ./../standby/dram.o
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) -O0 ./../standby/dram_init.c   -o ./../standby/dram_init.o
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) ./../standby/mem_divlib.S               -o ./../standby/mem_divlib.o 
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) ./../standby/mem_divlibc.c              -o ./../standby/mem_divlibc.o 
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) ./../standby/mem_timing.c    		    -o ./../standby/mem_timing.o 
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) ./../standby/mem_delay.S                -o ./../standby/mem_delay.o
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) ./../standby/mem_mmu_pc_asm.S           -o ./../standby/mem_mmu_pc_asm.o
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) ./../standby/mem_printk.c               -o ./../standby/mem_printk.o 
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) ./../standby/mem_serial.c               -o ./../standby/mem_serial.o 
	$(CC) $(INCLUDE) $(CFLAGS) $(CFLAGS_DRAM_VADDR) ./../standby/mem_int.c               -o ./../standby/mem_int.o 


	$(CROSS_COMPILE)ld -T dram_freq.xn $(LD_FLAGS) $(LIBS) -EL  $(DRAM_OBJ) -o dram_freq_entry.elf  -Map dram_freq_entry.map
	$(CROSS_COMPILE)objdump -D dram_freq_entry.elf > dram_freq_entry.lst
	$(CROSS_COMPILE)objcopy -O binary dram_freq_entry.elf dram_freq_entry.bin

	rm -rf *.o ../*.o
	cp dram_freq_entry.bin dram_freq_entry.code

clean:
	-rm *.code *.map *.lst *.bin *.elf

