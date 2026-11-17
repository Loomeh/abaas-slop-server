# Server Configuration

Config file: `abaas-server.conf`  
Any key can also be given on the command line, which overrides the file:

```
abaas-server -c abaas-server.conf http_port=8080
```

The default config looks like this:

```
; Configure the ABaaS server

; Addresses
bind_ip = 0.0.0.0
public_host =
public_ip =

; Matching API (HTTP, TCP)
http_port = 80
extra_key_ids =
http_max_body = 65536
http_max_connections = 256
http_max_per_ip = 32
status_page = local

; STUN / TURN (UDP)
turn_port = 3478
turn_alt_port = 3479
turn_alt_ip =
turn_user = abaas
turn_pass = abaas
turn_realm = abaas
turn_strict_auth = false
turn_nonce_secret =
relay_port_min = 50000
relay_port_max = 50999
relay_policy = internal
max_allocations = 512
max_allocations_per_ip = 32
stun_rate_limit = 20

; Matchmaking
polling = 1
ticket_timeout = 10
ready_timeout = 30
fill_wait = 5
strict_matching = true
group_size = 0
network_as_local = false
hide_player_ips = false
max_tickets = 4096
max_tickets_per_ip = 32

; Logging
log_level = info
log_file =
```

Here's a table explaining what each setting does.

## Addresses

| Setting | Default | Meaning |
|---|---|---|
| `bind_ip` | `0.0.0.0` | Local IPv4 address to listen on (`0.0.0.0` = all) |
| `public_host` | empty | Address cabinets use to reach the server (put the same in `ServerHost=`). Empty = this machine's main IPv4 |
| `public_ip` | empty | Relay address handed out over TURN. Empty = `public_host` resolved. Set to the public IP when behind NAT |

## Matching API (HTTP, TCP)

| Setting | Default | Meaning |
|---|---|---|
| `http_port` | `80` | Matching API port (1–65535). Anything but 80 needs `ServerPort=` on the cabinets |
| `extra_key_ids` | empty | Extra 4-character game ids to try when decoding a request whose key id is blank (comma or space separated; normally unused) |
| `http_max_body` | `65536` | Largest request body in bytes (1024–4194304) |
| `http_max_connections` | `256` | Open HTTP connections, server-wide |
| `http_max_per_ip` | `32` | Open HTTP connections per source IP (an arcade's cabinets share one IP) |
| `status_page` | `local` | `GET /status` (lists keychips, IPs and relays): `local` = only from 127.0.0.1, `public`, `off` |

## STUN / TURN (UDP)

| Setting | Default | Meaning |
|---|---|---|
| `turn_port` | `3478` | STUN/TURN port |
| `turn_alt_port` | `3479` | Second port for the client's NAT-type tests (RFC 5780). `0` = off |
| `turn_alt_ip` | empty | Optional second local IPv4 for full RFC 5780 tests (then set `bind_ip` to the primary one) |
| `turn_user` | `abaas` | TURN user name given to cabinets |
| `turn_pass` | `abaas` | TURN password given to cabinets |
| `turn_realm` | `abaas` | TURN realm |
| `turn_strict_auth` | `false` | Reject TURN requests whose MESSAGE-INTEGRITY doesn't verify |
| `turn_nonce_secret` | empty | Secret for TURN nonces. Empty = random each run; set any long random string so TURN sessions survive restarts |
| `relay_port_min` | `50000` | First UDP port for relayed addresses (one per cabinet in a match) |
| `relay_port_max` | `50999` | Last UDP port for relayed addresses |
| `relay_policy` | `internal` | Where relayed traffic may go: `internal` = other allocations on this server only; `known` = also the IPs of connected cabinets; `any` = anywhere (open relay, never on the internet) |
| `max_allocations` | `512` | TURN allocations, server-wide |
| `max_allocations_per_ip` | `32` | TURN allocations per source IP |
| `stun_rate_limit` | `20` | Unauthenticated STUN/TURN answers per second per source IP (anti-reflection) |

## Matchmaking

| Setting | Default | Meaning |
|---|---|---|
| `polling` | `1` | Seconds between the client's `/api/match` polls (1–60) |
| `ticket_timeout` | `10` | Forget a waiting request that hasn't polled for this many seconds |
| `ready_timeout` | `30` | Cancel a match if not everyone reports ready within this many seconds |
| `fill_wait` | `5` | Rooms bigger than 2: start with `member_min` players after waiting this long |
| `strict_matching` | `true` | Only match cabinets with identical `title_key*`/`param*` filters (same mode/settings). `false` = same game and version is enough |
| `group_size` | `0` | Force the room size. `0` = what the game asks for (`member_max` + 1) |
| `network_as_local` | `false` | Every cabinet's NETWORK searches only match the same location, like in-store ones. For single locations use `NetworkAsLocal=1` in their `abaas.ini` |
| `hide_player_ips` | `false` | Players only get each other's relay addresses, never real IPs (all match traffic then goes through the relay) |
| `max_tickets` | `4096` | Waiting + matched requests, server-wide |
| `max_tickets_per_ip` | `32` | Unmatched requests per source IP |

## Logging

| Setting | Default | Meaning |
|---|---|---|
| `log_level` | `info` | `debug` (every request's decoded JSON), `info`, `warn`, `error` |
| `log_file` | empty | Also write the log to this file. Empty = console only |
