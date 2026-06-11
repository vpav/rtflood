# Roughtime UDP Load Test Tool

`rtflood.py` is a high-throughput roughtime request sender designed to exercise a target service with many one-shot UDP datagrams. The tool currently sends draft-12 messages.

## Features

- Sends each packet on a separate UDP socket, then closes the socket immediately.
- Appends a random 32-byte nonce to every packet.
- Computes a server tag from a base64-encoded public key using SHA-512 and truncation.
- Uses worker threads for concurrent packet transmission.
- Supports total packet count, duration limits, and quiet mode.

## Requirements

- Python 3.8+
- `pycryptodomex`

Install the dependency with:

```bash
pip install pycryptodomex
```

## Usage

```bash
python rtflood.py [OPTIONS] HOSTNAME PORT PUBLIC_KEY
```

### Positional arguments

- `hostname` - target hostname or IP address
- `port` - target UDP port
- `public_key` - target server public key in base64 format

### Options

- `--workers WORKERS` - number of concurrent sender threads (default: 16)
- `--count COUNT` - total number of packets to send (0 = run until interrupted)
- `--duration DURATION` - run time in seconds (0 = no duration limit)
- `--quiet` - suppress periodic statistics output

## Example

```bash
python rtflood.py --workers 32 --count 10000 --duration 10 roughtime.example.com 4556 pJVdDXLNvbuNfU6Na7K+MjzqUg2VVOM5A6umuL4dZgU=
```

## Notes


- The tool does not wait for server replies and is optimized for raw transmit throughput.
- Use with caution and only against systems you are authorized to test.
