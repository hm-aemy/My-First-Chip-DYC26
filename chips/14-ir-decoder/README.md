# 14-ir-decoder

**Author:** Lukas Mittermeier, Alexander Sauerwein, Maik Unglert, Katalin Szentmiklosy

## Description

A fully hardware-based Infrared Remote Decoder, Recorder, and Replay system implemented in SystemVerilog.

The chip receives IR signals from a standard IR receiver module, decodes them in real time according to the **NEC**, **Samsung36/32**, and **N8X2** (custom Christmas-lights) protocols, and outputs decoded frames as human-readable text over **UART** (9600 baud, 8N1). Up to **40 decoded IR codes** can be stored in on-chip BRAM slots and replayed at any time via a 38 kHz modulated IR transmitter output. Recording and replay can be triggered either by physical buttons or wirelessly through an **ESP32-C3 WiFi web-UI** connected over a software SPI interface.

## Pin List

| Pin | Direction | Description |
| --- | --- | --- |
| `io_clk_pad` | Input | Main clock input |
| `io_rst_n_pad` | Input | Active-low asynchronous reset |
| `io_ir_in_pad` | Input | Raw IR signal from IR receiver module (active low, demodulated) |
| `io_spi_clk_pad` | Input | Software-SPI clock from ESP32-C3 WiFi module |
| `io_spi_data_pad` | Input | Software-SPI data from ESP32-C3 WiFi module |
| `io_ir_tx_npn_drive_pad` | Output | IR LED drive output |
| `io_uart_tx_pad` | Output | UART TX — decoded frame as ASCII text |
| `io_receiving_pad` | Output | Status: IR reception active  |
| `io_valid_pad` | Output | Status: pulse when a frame has been successfully decoded and validated |
| `io_recording_pad` | Output | Status: recording active (waiting for a valid frame to store into BRAM) |

## Design Data

The full RTL source and build scripts are provided as a Git submodule in [`design_data/`](design_data/).

```bash
git submodule update --init chips/14-ir-decoder/design_data
```
