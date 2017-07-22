# Encoded-Laser-and-Infrared-Serial-Communication

This is an arduino library which enables you to use a simple laser or LED to transmit characters between two arduinos (transmitter arduino with the laser to receiver arduino with a photodiode). Each byte of data is encoded (but not encrypted) to add some robustness to noise during the transmission and then modulated. The pin connection setup is explained in the example skethes included in the library. I originally wrote this as part of a netduino robotics project for uni, but I've since modified it to work with an arduino.

## What you need:
- 2 x arduinos with USB cables
- 2 x computers, one per arduino
- male to male and male to female jumper leads
- 1 x KY-008 laser module or a visible light LED
- 1 x laser sensor module, non-modulated
- 1 x mini breadboard for photo-receiver / photodiode

## How to install:
- download this library and add it to your libraries folder in the arduino folders on your computers

## How to use:
1. open the arduino application
2. open File -> Examples -> HT_LumenWire and select either the photoreceiver or transmitter examples
3. plug in the arduino and select the appropriate COM port, then upload the transmitter / receiver sketches to their respective arduinos
4. connect the components as described in the sketches
5. open the serial window (spyglass symbol in the IDE) on both computers
6. point the laser or LED to the photodiode
7. enter a character or phrase in the serial window on the computer doing the transmitting and press enter to send
8. you should receive the message on the receiver serial window automatically

## How it works:
### 1. Hamming Encoding:
Each byte of data typed into the serial window is encoded via hamming encoding, producing an unsigned 16 bit integer. The extra 8 bits contain information about the original byte to help detect errors and recover the original byte in case some of the data was scrambled due to noise in the transmission. The encoding / decoding algorithm has its limits on how much unscrambling it can do but it is certainly more efficient than sending multiple times to improve accuracy.
### 2. Manchester Modulation:
The 16-bit integer is then modulated via manchester modulation, producing 44 bits of data (with half bits, start and stop bits etc.)
### 3. Transmission
The 44-bit data is sent via the laser or LED, 1's are on and 0's are off
### 4. Manchester De-Modulation:
The 44-bit signal is received by the photo-detector and manchester-demodulated to reconstruct the 16-bit encoded signal
### 5. Hamming Decoding:
The 16-bit demodulated signal is then hamming decoded to reproduce the original byte
### 6. Printing Message:
The byte is then printed on the serial window

## Extras:
- For a full list of included functions in the library, please refer to the .cpp and .h files.
- You can also set the bitrate via the .set_speed(...) function. It must be a minimum of 500 bits/sec but no more than the laser slew rate. You will need to set the speed manually into both the transmitter and receiver codes.

## Future versions:
- I plan to modify the code so that you only need to set the speed on the transmitter end and the receiver will automatically calculate the bitrate based on the interval between received bits.
- I plan to make this project work on a micro-controller with a higher clock rate than the arduino, like a raspberry pi
