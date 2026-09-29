# plugin-ssh

SSH port forwarding for OpenCharly — the externalized `charly ssh tunnel`
command.

The plugin owns the `charly ssh tunnel spice/vnc` command end to end: the Kong
grammar (`SshCmd` → `tunnel spice/vnc`), the SSH-tunnel machinery, the
libvirt-URI parse, and the UNIX→TCP bridge. It opens an SSH-forwarded local
SPICE/VNC endpoint pointing at a VM's display on a remote libvirt host — for
clients that do not natively understand `qemu+ssh://` (standalone `remote-viewer`
with a TCP address, TigerVNC, Spicy). `virt-manager` and
`remote-viewer --connect qemu+ssh://…` do not need it.

It is a **compiled-in** command plugin because its `Invoke(OpRun)` needs the
in-proc reverse channel to reach `verb:libvirt` for the display-endpoint resolve;
the out-of-process `CliMain` path has no reverse channel.

## What it provides

| Capability | Surface |
|---|---|
| `command:ssh` | the `charly ssh tunnel spice` / `charly ssh tunnel vnc` CLI |

## How to use it

```bash
charly ssh tunnel spice <vm> --host <remote-libvirt-host>
charly ssh tunnel vnc <vm> --host <remote-libvirt-host>
```

The command prints the local endpoint a SPICE/VNC client can then dial.

## Layout

- `candy/plugin-ssh/` — the plugin module: `command.go`/`provider.go` (the
  command + provider), `tunnel.go` (the SSH-forwarded endpoint), `plugin.go`,
  `schema/ssh.cue`, `cmd/serve/main.go`.
- `charly.yml` — the root project manifest (`discover: candy`).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.

## Related

- Owning skill: `/charly-core:ssh` — SSH tunnel access to remote SPICE and VNC
  endpoints. This candy carries no `skill:` entity of its own; the gap is tracked
  in
  [opencharly/opencharly#291](https://github.com/opencharly/opencharly/issues/291).
- `/charly-vm:vm` — the VM/libvirt surface the tunnel targets.
- `/charly-internals:plugin` — the plugin/provider model.
