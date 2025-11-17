# ProxyMaker

ProxyMaker is a small Bash helper that spins up a `kubectl proxy` on a remote
server and forwards it to your local machine over SSH. It is handy when you need
quick access to a Kubernetes API that is only reachable from a jump host.

## Usage

```bash
./ProxyMaker <remote_host>
```

Environment variables let you customize ports without editing the script:

- `LOCAL_PORT` (default `8080`): local port to bind the tunnel.
- `REMOTE_PORT` (default `8888`): remote port for `kubectl proxy`.

## What it does

1. Stops any existing local SSH tunnels that match the configured ports.
2. Stops any existing `kubectl proxy` processes on the remote host using the
   chosen remote port.
3. Starts a fresh `kubectl proxy` on the remote host.
4. Creates a background SSH tunnel from `LOCAL_PORT` to the remote proxy port.

When everything is running, you can reach the Kubernetes API at
`http://127.0.0.1:<LOCAL_PORT>`.

## Prerequisites

- SSH access to the remote host.
- `kubectl` installed on the remote host with access to the desired cluster.
- `pgrep` available locally and on the remote host.

## Troubleshooting

- **Permission errors**: ensure your SSH user can run `kubectl proxy` and kill
  existing processes on the remote host.
- **Port conflicts**: set `LOCAL_PORT` or `REMOTE_PORT` to unused values before
  running the script.
