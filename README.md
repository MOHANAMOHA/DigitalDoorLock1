🏠 Home Security System – Verilog | Basys 3 FPGA

This project implements a simple Home Security System using the Basys 3 FPGA.
It uses 4 switches as a password, LED indicators for unlock/alarm, and a seven-segment display to show the lock status.

It also includes a full simulation testbench and a Vivado waveform example.

🚀 Features

✔ 4-bit password input using switches

✔ Unlock LED (green)

✔ Alarm LED (red)

✔ Seven Segment Display shows:

U → Unlocked

L → Locked

✔ Vivado behavioral simulation included

✔ Ready-to-run on Basys 3 FPGA

📌 Top Module – home_security_top.v
module home_security_top (
    input  [3:0] sw,             // Password input
    output reg led_unlock,       // Unlock LED
    output reg led_alarm,        // Alarm LED
    output [6:0] seg             // Seven segment display
);

    parameter [3:0] PASSWORD = 4'b1010;

    reg [6:0] seg_data;

    always @(*) begin
        if (sw == PASSWORD) begin
            led_unlock = 1;
            led_alarm  = 0;
            seg_data   = 7'b0001000; // U
        end else begin
            led_unlock = 0;
            led_alarm  = 1;
            seg_data   = 7'b1000110; // L
        end
    end

    assign seg = seg_data;

endmodule

🧪 Testbench – tb_home_security.v
`timescale 1ns/1ps

module tb_home_security;

    reg [3:0] sw;
    wire led_unlock, led_alarm;
    wire [6:0] seg;

    home_security_top uut (
        .sw(sw),
        .led_unlock(led_unlock),
        .led_alarm(led_alarm),
        .seg(seg)
    );

    initial begin
        // Test 1 – wrong password
        sw = 4'b0000;  
        #10;

        // Test 2 – correct password (1010)
        sw = 4'b1010;  
        #10;

        // Test 3 – wrong input again
        sw = 4'b1111;
        #10;

        // Test 4 – correct password again
        sw = 4'b1010;
        #10;

        $finish;
    end

endmodule

📊 Simulation Waveform (Vivado)

Below is the waveform generated from your simulation:

🔍 Uploaded Simulation Screenshot

![WhatsApp Image 2025-11-20 at 00 34 57](https://github.com/user-attachments/assets/8932c6a3-a7d2-43f0-9b40-6c0e0df58e45)


✔ Expected Behavior in Waveform
Input (SW)	Result	LED Unlock	LED Alarm	7-Seg
0000	Wrong	0	1	L
1010	Correct	1	0	U
1111	Wrong	0	1	L
1010	Correct	1	0	U

Your screenshot confirms this exact behavior.

🔠 Seven Segment Encoding Table
Character	Encoding
L	7'b1000110
U	7'b0001000
🎛 Basys 3 XDC Constraints
## Switches
set_property PACKAGE_PIN V17 [get_ports {sw[0]}]
set_property PACKAGE_PIN V16 [get_ports {sw[1]}]
set_property PACKAGE_PIN W16 [get_ports {sw[2]}]
set_property PACKAGE_PIN W17 [get_ports {sw[3]}]
set_property IOSTANDARD LVCMOS33 [get_ports {sw[*]}]

## LEDs
set_property PACKAGE_PIN U16 [get_ports led_unlock]
set_property PACKAGE_PIN V15 [get_ports led_alarm]
set_property IOSTANDARD LVCMOS33 [get_ports led_unlock]
set_property IOSTANDARD LVCMOS33 [get_ports led_alarm]

## Seven Segment Display
set_property PACKAGE_PIN U8  [get_ports {seg[0]}]
set_property PACKAGE_PIN V8  [get_ports {seg[1]}]
set_property PACKAGE_PIN U7  [get_ports {seg[2]}]
set_property PACKAGE_PIN V7  [get_ports {seg[3]}]
set_property PACKAGE_PIN U6  [get_ports {seg[4]}]
set_property PACKAGE_PIN V6  [get_ports {seg[5]}]
set_property PACKAGE_PIN U5  [get_ports {seg[6]}]
set_property IOSTANDARD LVCMOS33 [get_ports {seg[*]}]

📁 Recommended GitHub Project Structure
HomeSecurity_FPGA/
│
├── src/
│   ├── home_security_top.v
│   └── tb_home_security.v
│
├── constraints/
│   └── Basys3.xdc
│
├── simulation/
│   ├── sim_waveform.png
│   └── sim_results.vcd (optional)
│
└── README.md
