# Li-Fi (Light Fidelity) Wireless Communication System

This project implements a **Li-Fi communication system** using visible light as the transmission medium. An LED transmitter modulates data onto a carrier signal, while a photodiode receiver with analog front-end (AFE) circuitry demodulates and decodes the signal to display received text on an LCD. Demonstrates fundamental optical communication principles using embedded microcontrollers.

## Core Concept & Objective

- **Goal:** Transmit short text messages wirelessly using **intensity modulation** of an LED, received by a photodiode and decoded by a microcontroller.
- **Key Challenge:** Convert digital text data into a modulated light signal that can be reliably detected, demodulated, and framed for error-free reception over short distances (1-2 meters).

## Technical Design

- **Transmitter Side:**
  - Microcontroller (Arduino/ESP32) encodes text into frames with start/stop bits and simple checksum.
  - **LED Modulation:** PWM or direct digital output modulates LED brightness at a carrier frequency (e.g., 1-10 kHz).
  - Framing: Simple protocol with preamble (0xAA), length byte, payload, and CRC/checksum.
  
- **Receiver Side:**
  - **Photodiode + AFE:** Photodiode converts light intensity to current → transimpedance amplifier → bandpass filter → comparator → digital signal.
  - Microcontroller samples the demodulated signal, detects frame sync, extracts payload, validates checksum, and displays on LCD.
  - **Signal Processing:** Software UART-like decoding with bit timing recovery and frame synchronization.

- **Communication Parameters:**
  - Range: 1-2 meters (line-of-sight)
  - Data rate: ~100-500 bps (limited by LED/photodiode response and microcontroller sampling)
  - Modulation: On-Off Keying (OOK) with Manchester encoding for DC balance

## Project Structure
├── transmitter/
│ ├── tx_main.ino # Text encoding + LED PWM modulation
│ └── protocol.h # Framing and checksum functions
├── receiver/
│ ├── rx_main.ino # Photodiode signal processing + LCD display
│ ├── afe_circuit.png # Analog front-end schematic
│ └── timing_calib.cpp # Bit synchronization algorithm
└── docs/
├── protocol_spec.md # Frame format specification
└── performance.md # BER vs distance measurements
## Protocol Frame Format

[PREAMBLE: 0xAA x4] [LEN: 1B] [PAYLOAD: 0-32B] [CHECKSUM: 1B]
Example: AA AA AA AA 05 "HELLO" 0x2A

## Hardware Implementation

Transmitter:
Arduino/ESP32 → PWM pin → Current-limiting resistor → High-brightness LED (white/blue)
Receiver:
Photodiode → Op-amp transimpedance → Bandpass filter → Comparator → MCU interrupt pin
MCU → I2C/SPI → 16x2 LCD (HD44780)

## Example Operation

1. **TX:** User enters "HELLO" → framed → modulated onto LED at 2 kHz carrier
2. **RX:** Photodiode detects blinking → AFE cleans signal → MCU syncs to preamble → extracts "HELLO" → displays on LCD
3. **Distance Test:** Reliable up to 1.5m in normal room lighting

## Key Results

- **Reliable range:** 1.2-1.8 meters (line-of-sight)
- **Bit Error Rate (BER):** <1% at 1m distance
- **Max payload:** 32 characters per frame
- **Latency:** ~200ms end-to-end for 20-char message

## Skills Demonstrated

Optical communication principles • LED modulation (PWM/OOK) • Photodiode analog front-end design • Digital signal processing on MCU • Framing and error detection protocols • Embedded protocol design • Hardware-software integration • Bit timing recovery
