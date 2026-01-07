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
