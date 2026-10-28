# AGENTS.md

This repository is the shared MT6835 SoC layer of UniXNU. Board repositories depend on
it; do not add board-specific memory maps, reserved regions, enabled peripherals or
partition data here.

## Responsibilities

- `soc.toml`: format 2 SoC profile. Keep `[soc] name = "MT6835"` stable; boards select it
  by `soc_source`.
- `configs/mt6835.config`: only settings that hold for every MT6835 board.
- `dts/mt6835.dtsi`: CPU topology, PSCI, architected timer and GIC. Every value must
  cite its source in a comment and carry `REQUIRED` until hardware-verified.
- `patches/xnu/series`: apply order is line order. Leave the file present when empty.
- `kits/`: shared platform code; board-specific drivers belong to component or device
  repositories.

## Rules

1. Read the [SoC support](https://unixnu.com/zh/porting/soc-support) and
   [Device tree](https://unixnu.com/zh/porting/device-tree) guides before changing
   bindings. Exported nodes need a unique `unixnu,adt-name`; address translation needs
   `ranges` on every parent bus.
2. Never invent register ranges, interrupt numbers or timer frequencies. Extract them
   from the stock firmware DTB and record the source and extraction command in the commit.
3. A value marked `REQUIRED` must stay disabled or fail early; do not promote it to a
   default for other boards.
4. After changing the DTSI, compile the board DTS and regenerate the ADT through
   `./unixnu payload` before reporting success.
