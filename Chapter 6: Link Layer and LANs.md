# Chapter 6: Link Layer and LANs

## 6.1 Link Layer

The Link Layer, part of the OSI model's Layer 2, provides communication between devices in a local network. It includes protocols like Ethernet (IEEE 802.3), Wi-Fi (IEEE 802.11), PPP, HDLC, and ARP, managing tasks like framing, link access(MAC), reliable delivery, and error detection.

- Link layer is implemented on a chip called the network adapter or `network interface controller (NIC)`.
- Integrated into the motherboard chipset or implemented via a dedicated Ethernet chip.
- Implemented in hardware, handling framing, link access, error detection, etc.

## 6.2 Error Detection & Correction Techniques

### 6.2.1 Parity Checks

- **Even Parity:** Ensures that the total number of 1s in a data unit (including the parity bit) is even.
  - If an odd number of bits are in error, the parity check will detect it.
- **Odd Parity:**
  - Requires the total number of 1s to be odd.
- **Forward Error Correction** (FEC) is an error correction technique where extra bits are added to transmitted data, allowing the receiver to correct errors without retransmission.

### 6.2.2 Checksumming Methods

- **Definition:** Checksums involve summing up the values of a set of data units and appending the result to the data being transmitted.
- **Checksum Calculation:**
  - Sender calculates a checksum value based on the data.
  - Sender sends both the data and the checksum.
  - Receiver calculates its checksum on the received data.
  - If the calculated checksum mismatches the received checksum, an error is detected.
- See more on: [Checksums in UDP](https://github.com/VasanthVanan/computer-networking-top-down-approach-notes/blob/main/Chapter%203%3A%20Transport%20Layer.md#35-udp-user-datagram-protocol)

### 6.2.3 Cyclic Redundancy Check (CRC)

- **Definition:** CRC is a more sophisticated error-checking technique.
- **Polynomial Division:**
  - Data is treated as coefficients of a polynomial.
  - Division is performed using a predetermined divisor polynomial.
  - Remainder is appended to the data for transmission.
- **Error Detection:**
  - Receiver performs the same division and checks the remainder.
  - If the remainder is not zero, an error is detected.

