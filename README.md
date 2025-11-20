
/*
 * Embedded Systems Workshop - STM32 GPIO Control
 * Demonstrates register-level programming to blink an LED
 * Microcontroller: STM32 (ARM Cortex-M)
 * Key Concepts: Memory-mapped I/O, GPIO configuration, clock control
 */
## Demo
[![Embedded Systems Demo - LED Blinking](https://drive.google.com/file/d/1ZSSdw7-lDCEUeJGGl38fh9hsWLkjDEap/view?usp=drive_link)
*Click the image above to watch the video*
// Memory map definitions for STM32 peripheral access
#define PERIPH_BASE (0x40000000UL)
#define AHB1_PERIPH_OFFSET (0x00020000UL)
#define AHB1_PERIPH_BASE (PERIPH_BASE + AHB1_PERIPH_OFFSET)

// GPIO Port B base address
#define GPIOB_OFFSET (0x000004000UL)
#define GPIOB_BASE (AHB1_PERIPH_BASE + GPIOB_OFFSET)

// RCC (Reset and Clock Control) base address
#define RCC_OFFSET (0x00003800UL)
#define RCC_BASE (GPIOB_BASE + RCC_OFFSET)

// Register offsets and definitions
#define AHB1EN_R_OFFSET (0x30UL)
#define RCC_AHB1EN_R (*(volatile unsigned int * ) (RCC_BASE + AHB1EN_R_OFFSET))

#define MODER_OFFSET (0x00UL)
#define GPIOB_MODE_R (*(volatile unsigned int * ) (GPIOB_BASE + MODER_OFFSET))

#define OD_R_OFFSET (0x14UL)
#define GPIOB_ODR (*(volatile unsigned int *)(GPIOB_BASE + OD_R_OFFSET))

// Bit definitions for GPIO configuration
#define GPIOB_EN (1U << 15)  // Enable GPIOB clock
#define PIN7 (1U << 14)      // Pin 7 mask
#define LED_PIN PIN7         // LED connected to PB7

// Register map structures for type-safe peripheral access
typedef struct {
	volatile uint32_t CR;
	volatile uint32_t PLLCFGR;
	volatile uint32_t CIR;
	volatile uint32_t AHB1RSTR;
	volatile uint32_t AHB2RSTR;
	volatile uint32_t AHB3RSTR;
	uint32_t RESERVED0;
	volatile uint32_t APB1RSTR;
	volatile uint32_t APB2RSTR;
	uint32_t RESERVED1[2];
	volatile uint32_t AHB1ENR;  // Clock enable register
}RCC_TypeDef;

typedef struct {
	volatile uint32_t MODER;    // Mode register
	volatile uint32_t OTYPER;   // Output type register
    volatile uint32_t OSPEEDR;  // Output speed register
	volatile uint32_t PUPDR;    // Pull-up/pull-down register
	volatile uint32_t IDR;      // Input data register
	volatile uint32_t ODR;      // Output data register
	volatile uint32_t BSRR;     // Bit set/reset register
	volatile uint32_t LCKR;     // Configuration lock register
	volatile uint32_t AFRL;     // Alternate function low register
	volatile uint32_t AFRH;     // Alternate function high register
}GPIO_TypeDef;

// Peripheral pointers for structured access
#define RCC ((RCC_TypeDef*)RCC_BASE)
#define GPIOB ((GPIO_TypeDef*) GPIOB_BASE)

int main(void)
{
	// 1. Enable clock for GPIOB peripheral
	RCC -> AHB1ENR |= GPIOB_EN;
	
	// 2. Configure PB7 as output (clear and set MODER bits)
	GPIOB -> MODER |= (1U << 15);   // Set bit 15
	GPIOB -> MODER &= ~(1U << 14);  // Clear bit 14
	
	// 3. Main loop - toggle LED with delay
	while (1) {
		GPIOB_ODR ^= LED_PIN;  // Toggle PB7 using direct register access
		
		// Simple delay loop
		for (int i = 0; i < 1000000000; i++) {
			// Busy-wait delay
		}
	}
}
