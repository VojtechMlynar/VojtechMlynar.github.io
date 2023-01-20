Every address you see in Vivado corresponds to single byte (8-bits).

When you map address space of certain AXI interface in Linux using code below, you have to provide the same offset you see in Vivado address editor (i.e. 0x4040000 by default for DMA)
```c
#define DMA_AXI_ADDR 0x40400000

// Open /dev/mem which represents the whole physical memory
int dh = open("/dev/mem", O_RDWR | O_SYNC); 

// Create reference (pointer) to mapped memory at offset DMA_AXI_ADDR
// with length 0xFFFF (in bytes)
unsigned int *dma_address = mmap(NULL, 0xFFFF,
                                     PROT_READ | PROT_WRITE,
                                     MAP_SHARED, dh, DMA_AXI_ADDR);
```

If you afterwards want to access register within the memory space `dma_address`, you have to bitshift the register offset by 2 bits (or divide by 4), because ARM is addressing at 32-bit increments, such as:
```c
#define REGISTER_OFFSET 0x100

int new_value_1 = 42;
int new_value_2 = 911;

// This writes new_value_1 to address 0x40400100 and
// new_value_2 to address 0x40400104
*(dma_address + (REGISTER_OFFSET>>2)) = new_value_1;
*(dma_address + (REGISTER_OFFSET>>2) + 1) = new_value_2;

```

If you want to access specific byte within address, you need to utilize bit-masking:

```c
uint8_t new_byte = 255;

// This writes new_byte into address 0x40400101
uint32_t old_value = *(dma_address + (REGISTER_OFFSET>>2));
*(dma_address + (REGISTER_OFFSET>>2)) = (old_value & ~((uint32_t)(0xFF00)) ) | ((uint32_t)(new_byte)<<8);

// (old_value & ~((uint32_t)(0xFF00))) This reads register value and erases
// second (LSB) byte. 
//
// ((uint32_t)(new_byte)<<8) This shifts a new byte to appropriate position
// casts it to 32 bit integer. Or (|) between the brackets writes into the byte

```
