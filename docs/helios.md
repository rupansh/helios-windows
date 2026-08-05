# Helios GPU acceleration

This fork publishes an amd64 image with the Helios QEMU and Venus host stack to
`ghcr.io/winboat-org/helios-windows:latest`.

Pass the host render node into the container and enable Helios with these
Compose fields:

```yaml
environment:
  HELIOS: "Y"
  HELIOS_BOOTSTRAP: "N"
  HELIOS_HOSTMEM: "4G"
  HELIOS_BLOB_LIMIT: "4G"
  RENDERNODE: /dev/dri/renderD128
  VKR_DEVICE_MEMORY_LIMIT_BYTES: "4294967296"
  override_vram_size: "4096"
devices:
  - /dev/kvm
  - /dev/dri/renderD128
```

Set `HELIOS_BOOTSTRAP=Y` while installing the Windows driver to keep a standard
VGA device available. Set it back to `N` after the driver-restart checkpoint.

The image is based on the current upstream Dockur image. QEMU and
virglrenderer are pinned in `Dockerfile.helios` so a rebuild uses the tested
host graphics stack.
