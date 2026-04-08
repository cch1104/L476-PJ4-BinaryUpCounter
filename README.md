# L476-PJ4-BinaryUpCounter
Project Overview
This project demonstrates how to group multiple GPIO pins and control them as a single unit (a byte) to represent numerical data visually. Using an STM32 NUCLEO-L476RG board, the program implements a continuous binary up-counter from 0 to 255, displaying the count using 8 LEDs.
Hardware Requirements
Microcontroller: NUCLEO-L476RG.
LEDs: 8 units connected to GPIO Port C (PC0 to PC7).
Resistors: 8x 470 Ω current-limiting resistors.
Connection Layout:
PC0: Least Significant Bit (LSB).
PC7: Most Significant Bit (MSB).
Circuit Schematic
The LEDs are connected in series with 470 Ω resistors between the GPIO pins (PC0–PC7) and Ground (GND).
Pin Mapping: PC0 → LED0 ... PC7 → LED7.
Key Concepts
1. Bitwise Operations
The core logic for converting a decimal number into a binary LED display relies on bitwise operators:
AND (&): Used to check if a specific bit is set (1).
Left Shift (<<): Used to create a bitmask (e.g., 1 << i) to target a specific bit position.
2. Embedded Data Types
To ensure portability and memory efficiency, this project follows the "Golden Rule for Embedded Development" by using fixed-width integer types from <stdint.h>:
uint8_t: Used for the counter (0–255) and LED control to save memory.
uint16_t: Used for storing GPIO pin definitions.
Why not int? Standard int sizes are platform-dependent and can waste critical memory in microcontrollers.
Code Implementation
LED Pin Definitions
The pins are grouped into an array for easier iteration:
#define led0 GPIO_PIN_0
// ... defines for led1 through led7
uint16_t LEDS[] = {led0, led1, led2, led3, led4, led5, led6, led7};

Display Logic
The Display function iterates through each bit (0–7) of the target number. It uses a bitwise mask to determine if the LED should be SET or RESET:

void Display(uint8_t number, uint8_t size){
  for (uint8_t i=0; i<size;i++)
	  {
		  if(number &(1<<i)){
			  HAL_GPIO_WritePin(GPIOC,LEDS[i],GPIO_PIN_SET);
		  }else{
			  HAL_GPIO_WritePin(GPIOC,LEDS[i],GPIO_PIN_RESET);
		  }
	  }
 }

Main Loop
The counter increments every 1000ms (1 second):
while (1) {
    for (uint8_t i = 0; i < 256; i++) {
        Display(i, 8);
        HAL_Delay(1000);
    }
}

