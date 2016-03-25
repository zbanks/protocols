# Disclaimer

This is not complete, or likely even *correct*. It's a first pass at the most obvious network protocols that come to mind. The goal is to build some kind of dependency tree & highlight which aspects of each protocol are actually useful (I'm looking at you, TCP).

# Network Protocols

(Table forthcoming)

### Attributes
Attempt at a list, so I can make a nice table

- Multiplex (MAC?)
    - Values
        - One-way (simplex?)
        - Half-duplex
        - Full-duplex
        - Multiplex
    - Techniques
        - Channel-division multiplexing (e.g. separate wires, ports, etc.)
        - Request-response
        - Time-division multiplexing
- Payload
    - Values
        - Bits
        - Octets (bytes, words, etc.)
        - Frame (datagram)
        - Stream
    - Techniques
        - Delimiter (e.g. \x00\x00\x00\x00)
        - Length-payload
        - COBS
- Assembly
    - Values
        - None
        - Fragmentation/assembly
        - Stream
    - Techniques
        - ID & offset
- Errors
    - Values
        - Sensitive
        - Tolerant
        - Detection
        - Correction
    - Techniques
        - CRC32
        - ECC
        - Redundant data
        - Replay data
    - This is a weird category: some protocols have error detection as a requirement on the lower layer(s) -- others are more tolerant of errors or detect errors themselves.
        - Compare COBS vs. Length-payload encoding: the former can fail from errors without noticing, but it fails more gracefully than the latter
- Addressing
    - Values
        - Single
        - Addressable (+ size of address space)
    - Techniques
        - MAC Address: ~48 bits
        - IPv4 Address: ~32 bits
        - IPv6 Address: ~128 bits
        - Lux Addresses: ~32 bits
- Versioning
    - Values
        - No support
        - Versioning
        - Backwards-compatible
        - Forwards-compatible
    - Techniques
        - Version field (usually early / low)
- Data loss
    - Values
        - Undetectable
        - Gap detection
        - Gap filling ("guarenteed" delivery)
    - Techniques
        - Sequence numbers
        - ACK
        - NAK (Negative acknowledgement)
- Flow Control
    - Values
        - No support
        - Bidirectional
        - Request-response
    - Techniques
        - Out-of-band, e.g. CTS
        - In-band control messages
        - TCP-like (no ACK, windowing, etc.)
        - Request-response, only request when ready to recieve
- Lower-protocol consistency
    - Values
        - Multihoming support
        - Lower consistency required
    - Techniques
        - See SCTP
    - How do I phrase this better?
- State
    - Values
        - Stateless
        - Persistent connection
        - Transactional (short-lived state)
    - Techniques
        - Stateless: duplicate information (e.g. "What is your name?": "Foo." vs. "My name is Foo.")
        - Stateful: Handshakes
- Security
    - Values
        - No support
        - Data integrity
        - Data privacy
        - Authentication / identification
    - Techniques
        - HMAC (integrity only)
        - Public-key cryptography
        - Diffie-Hellman key exchange
        - Trusted third parties
        - See TLS
- Wire Overhead
    - Values
        - *N* bytes per byte/datagram/packet/etc.
    - Techniques
        - Header
- Memory Overhead
    - Values
        - Maintainig state
        - Buffering
    - Techniques
        - Stateless protocol
        - Ability to send a packet piecewise without buffering in memory first. (Checksums at the end!)
- Computational Overhead
    - Values
        - Calculating checksum of every packet
        - Encryption / decryption
        - *Action* per byte/datagram/packet/etc.
    - Techniques
        - Hard to mitigate
        


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

- **Requires:** Full-duplex stream connection
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

---

[Released under the MIT License](LICENSE)
