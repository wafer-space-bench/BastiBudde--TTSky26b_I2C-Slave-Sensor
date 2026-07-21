# Silicon Testing Procedure (I2C Slave Sensor, TTGF26b)

## 1. Purpose and scope
This document defines how the fabricated chip is tested once it returns from the TTGF26b shuttle. The goal is to confirm that the physical device behaves on the bus exactly as established in RTL simulation ([verification_report.md](verification_report.md)), gate-level simulation, and FPGA bring-up ([fpga_report.md](fpga_report.md)).

As with any fabricated chip, the design offers no internal signal visibility, so every check is a black-box observation over the I2C bus. This is the same scope as the gate-level simulation and the FPGA test.

## 2. Identical procedure to the FPGA test
The chip presents the same I2C interface as the FPGA and is tested by the same method. An Espressif ESP32-C6 acts as the I2C master and runs the same firmware test suite used for the FPGA, exercising the slave over the physical bus at 100 kHz, 400 kHz, and 1 MHz. The only difference from the FPGA test is the device under test itself.

For this reason the setup and the individual test cases are not repeated here. The master firmware, the wiring, the instrumentation, and the per-test requirement mapping are all described in [fpga_report.md](fpga_report.md). The firmware source is in `test/fpga/ESP32_C6_fpga_test`.

## 3. Chip bring-up
Only the steps specific to the fabricated chip differ from the FPGA setup.

- Mount the chip on its Tiny Tapeout carrier board and select this design so that it receives the system clock and reset.
- The design clock frequency is the one declared in `info.yaml`. The update thythm of the LFSR scales with the actual clock, as it does on the FPGA.
- Bring the two bus pins `uio[0]` (SCL) and `uio[3]` (SDA) and a common ground out to the ESP32-C6, and add external pull-ups as in the FPGA wiring.

The bus pins were deliberately placed to match the on-carrier RP2350 controller of the Tiny Tapeout demo board, so that controller could alternatively serve as the master. The procedure here keeps the ESP32-C6 for parity with the FPGA test.

## 4. Test execution
1. Power the carrier board and select this design.
2. Wire the ESP32-C6 to the chip and add the bus pull-ups and a common ground.
3. Build and flash the firmware with ESP-IDF, then open the serial monitor.
4. Observe the per-run pass/fail summary and, as needed, capture the bus waveforms with an oscilloscope at each speed.

## 5. Acceptance criteria
The chip is considered to pass when every black-box check of the firmware regression passes at all three bus speeds and the observed behaviour matches the simulation and FPGA results. The serial log of a full run and a set of representative oscilloscope captures are recorded as evidence.

## 6. Results
The fabricated chip has not yet been received from the shuttle, so this section is a placeholder. Once the chip is available and the procedure above has been run, it will hold the serial pass/fail summary and representative oscilloscope captures, mirroring Section 6 of [fpga_report.md](fpga_report.md).