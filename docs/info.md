## How it works

This project implements a 4-bit × 4-bit unsigned binary multiplier. Two 4-bit operands (A and B) are provided through the 8-bit dedicated input bus (`ui_in`), and the 8-bit product (P) is output on the dedicated output bus (`uo_out`).

### Architecture

The design consists of two modules:

- **`tt_um_mult_top`** — The top-level wrapper that interfaces with the Tiny Tapeout infrastructure. It registers the inputs and control signals (enable, reset) on the rising edge of the clock to ensure clean, synchronized operation. When the design is not enabled or is held in reset, the inputs to the multiplier are forced to zero.

- **`mult`** — A purely combinational multiplier core that computes `P = A * B` using the Verilog `*` operator; tools will infer the appropriate gate-level logic.

### Flow

1. `ui_in[7:4]` (operand A) and `ui_in[3:0]` (operand B) are registered on the clock edge.
2. The registered enable (`ena`) and a two-stage registered reset (`rst_n`) gate the inputs.
3. The combinational multiplier computes the product.
4. The 8-bit result is driven onto `uo_out[7:0]`.

The output is available one clock cycle after the inputs are applied.

## How to test

1. Assert reset (`rst_n = 0`) for at least one clock cycle, then deassert (`rst_n = 1`).
2. Set `ena = 1` to enable the multiplier.
3. Apply operand A on `ui_in[7:4]` and operand B on `ui_in[3:0]`.
4. After one clock cycle, read the 8-bit product from `uo_out[7:0]`.

### Example

A testbench (`mult_tb.v`) is included that runs 1000 randomized test vectors and reports the pass count.
