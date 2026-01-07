# UART Framing Mismatch – Simple Explanation

## Problem
In UART communication, the first character is received correctly,
but the remaining characters appear as garbage.

## Root Cause
This usually happens when:
- Data bits mismatch (7 vs 8)
- Stop bits mismatch (1 vs 2)

## Why First Character Looks Correct
UART resynchronizes using the START bit.
The first frame may align correctly,
but framing mismatch causes misalignment for following characters.

## Key Lesson
UART transmitter and receiver must match exactly in:
- Baud rate
- Data bits
- Parity
- Stop bits


#CODE
#include <lpc213x.h>

void UART0_Init(void)
{
    PINSEL0 |= 0x05; 	// P0.0 = TXD0, P0.1 = RXD0
    VPBDIV=0x01;
	  U0LCR = 0x83;           // 8-bit, no parity, 1 stop, DLAB = 1
    U0DLL = 78;             // 9600 baud @ 12MHz
    U0DLM = 0x00;
    U0LCR = 0x03;           // DLAB = 0
}

void UART0_Tx(char ch)
{
    while(!(U0LSR & (1 << 5)));  // wait until THR empty
    U0THR = ch;
		
}

int main(void)
{
    UART0_Init();
    UART0_Tx('H');
    UART0_Tx('i');
    UART0_Tx('\n');
    while(1);   
}
