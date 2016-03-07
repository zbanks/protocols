# Disclaimer

This is not complete, or likely even *correct*. It's a first pass at the most obvious network protocols that come to mind. The goal is to build some kind of dependency tree & highlight which aspects of each protocol are actually useful (I'm looking at you, TCP).

# Network Protocols

(Table forthcoming)

## 802.3 Ethernet (MAC) over 1xBASE-T

- **Requires:** Full-duplex octet stream
- **Provides:** Full-duplex datagram channel (MTU of 1500 octets); Addressing (48 bits, *MAC Address*); Error detection (CRC32 *FCS*); VLANs (*802.1Q tag*)

## IPv4 Unicast

- **Requires:** Full-duplex datagram channel
- **Provides:** Full-duplex datagram channel; Addressing (32 bits, *IP Address*); Multiplexing (via *Protocol Number*); Fragmentation (up to 64k octets)
- *TTL* field used in routing
- Has checksum for own headers; does not apply checksum to payload.

## TCP 

- **Requires:** Full-duplex datagram channel
- **Provides:** Full-duplex stream connection; Multiplexing (via *sport*, *dport*); Flow control; Error detection (CRC32); "Guaranteed" delivery
- (Push/Urgent, not often used)

## UDP

- **Requires:** Full-duplex datagram channel
- **Provides:** Full-duplex datagram channel; Multiplexing (via *sport*, *dport*); Error detection (CRC32)

## HTTP

- **Requires:** Full-duplex so etream connection
- **Provides:** Request-response connection; Multiplexing (via URLs, verbs, etc.); Metadata (via headers)

## SCTP

- **Requires:** Full-duplex datagram channel(s)
- **Provides:** Full-duplex datagram connection**s**; Multiplexing (via ports); Flow control; Multihoming; *TODO*

## Half-duplex UART

- **Requires:** 1 digital I/O signal, mutually agreed configuration
- **Provides:** Half-duplex octet stream (octet framing); Clock recovery
- Not restricted to octets, technically

## Length-Payload encoding

- **Requires:** Octet stream with error detection
- **Provides:** Datagram stream (with inherited error detection)

## COBS (*Constant Overhead Byte Stuffing*)

- **Requires:** Octet stream
- **Provides:** Datagram stream
- ``'\0'`` is a frame delimiter, which makes the encoding more robust to error

## Lux

- **Requires:** Half-duplex datagram stream
- **Provides:** Request-response (datagram) messaging; Addressing (32-bit); Multiplexing (via *command*, 8-bit); Fragmentation (via *index*); Error detection (CRC32)

## Multicast Lux

- **Requires:** Half-duplex datagram stream
- **Provides:** Multicast datagram publishing; Multicast addressing (32-bit); Multiplexing (via *command*, 8-bit); Fragmentation (via *index*); Error detection (CRC32)


Copyright 2016, Zach Banks
