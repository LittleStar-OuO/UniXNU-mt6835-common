# UniXNU-mt6835-common

Shared MediaTek MT6835 (Dimensity 6300 family) support for UniXNU, used by board
repositories such as [UniXNU-tb365fc](../UniXNU-tb365fc).

## Status

The CPU topology, GIC ranges, timer frequency and UART registers in
`dts/mt6835.dtsi` were measured from the TB365FC stock firmware DTB (ZUXOS
1.1.10.122). The PSCI conduit (`smc`) matches the stock `/psci` node
(`arm,psci-1.0`). No MT6835 device has booted XNU yet.

## Layout

| Path | Purpose |
| --- | --- |
| `soc.toml` | Format 2 SoC layer merged between the ARM64 and OS profiles |
| `configs/mt6835.config` | Kernel settings shared by every MT6835 board |
| `dts/mt6835.dtsi` | Shared CPU, timer, GIC and PSCI description |
| `patches/xnu/series` | SoC XNU patch order, currently empty |
| `kits/` | Shared platform operations, empty until a component needs them |

## Use

A board repository pins this repository in `feeds.toml`:

```toml
[[source]]
name = "mt6835"
kind = "soc"
git = "https://github.com/OWNER/UniXNU-mt6835-common"
commit = "<40-character commit>"
```

and selects it with `[board] soc = "MT6835"` and `soc_source = "mt6835"`. The board
DTS includes `mt6835.dtsi` through `[device_tree] include_dirs = ["soc:dts"]`.

The reference for the shared layout is the
[SoC support guide](https://unixnu.com/zh/porting/soc-support); hardware rules are in
[Device tree](https://unixnu.com/zh/porting/device-tree).
