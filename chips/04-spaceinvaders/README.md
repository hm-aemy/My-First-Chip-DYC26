# 04-spaceinvaders

> **Note:** This chip is not final and will be improved in the next few days to create a fully playable game.

**Authors:** Emil Gschloessl, Benjamin Otto, Stefanie Schmid

## Description
This project implements a classic Space Invaders arcade game in hardware. It features direct VGA video output and utilizes dedicated buttons to move the cannon and shoot or start the game.

Similar to typical FPGA boards, the VGA interface uses 14 signals: 4 bits per color (RGB) and two sync signals (HS/VS). The color signals rely on external resistor-divider circuits to provide 16 intensity levels per color.

For instructions on producing the chips gds, please refer to the [design_data README](design_data/README.md).

## Pin List

| Pin | Direction | Description |
|---|---|---|
| `clk_PAD` | Input | Main clock |
| `rst_n_PAD` | Input | Asynchronous reset (active low) |
| `button_left_PAD` | Input | Move left button |
| `button_right_PAD` | Input | Move right button |
| `button_shoot_PAD` | Input | Shoot button / Start game |
| `vga_r_PADs[0]` | Output | VGA red signal, bit 0 |
| `vga_r_PADs[1]` | Output | VGA red signal, bit 1 |
| `vga_r_PADs[2]` | Output | VGA red signal, bit 2 |
| `vga_r_PADs[3]` | Output | VGA red signal, bit 3 |
| `vga_g_PADs[0]` | Output | VGA green signal, bit 0 |
| `vga_g_PADs[1]` | Output | VGA green signal, bit 1 |
| `vga_g_PADs[2]` | Output | VGA green signal, bit 2 |
| `vga_g_PADs[3]` | Output | VGA green signal, bit 3 |
| `vga_b_PADs[0]` | Output | VGA blue signal, bit 0 |
| `vga_b_PADs[1]` | Output | VGA blue signal, bit 1 |
| `vga_b_PADs[2]` | Output | VGA blue signal, bit 2 |
| `vga_b_PADs[3]` | Output | VGA blue signal, bit 3 |
| `vga_hs_PAD` | Output | VGA horizontal sync |
| `vga_vs_PAD` | Output | VGA vertical sync |