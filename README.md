# AXI_Lite_Slave_Peripheral
lightweight AXI4-Lite slave peripheral in Verilog that connects a small register bank to a master

# **Spec Sheet:**
| Address Offset | Name         | Access | Description                       |
| -------------- | ------------ | ------ | --------------------------------- |
| `0x00`         | `REG_DATA`   | R/W    | General purpose data register     |
| `0x04`         | `REG_STATUS` | R      | Read-only status register (fixed) |

#**Interface:**
##Write Address Ch.
| Signal    | Dir | Description           |
| --------- | --- | --------------------- |
| `AWVALID` | IN  | Master wants to write |
| `AWREADY` | OUT | Slave ready           |
| `AWADDR`  | IN  | Write address         |

##Write Data Ch.
| Signal   | Dir | Description                    |
| -------- | --- | ------------------------------ |
| `WVALID` | IN  | Master sends data              |
| `WREADY` | OUT | Slave ready                    |
| `WDATA`  | IN  | Write data                     |
| `WSTRB`  | IN  | Byte enable (we’ll use all 1s) |

##Write Response Ch.
| Signal   | Dir | Description          |
| -------- | --- | -------------------- |
| `BVALID` | OUT | Response valid       |
| `BREADY` | IN  | Master accepted resp |


##Read Address Ch.
| Signal    | Dir | Description          |
| --------- | --- | -------------------- |
| `ARVALID` | IN  | Master wants to read |
| `ARREADY` | OUT | Slave ready          |
| `ARADDR`  | IN  | Read address         |


##Read Data Ch.
| Signal   | Dir | Description                 |
| -------- | --- | --------------------------- |
| `RVALID` | OUT | Read data valid             |
| `RREADY` | IN  | Master ready                |
| `RDATA`  | OUT | Read data                   |
| `RRESP`  | OUT | Response (we'll use `OKAY`) |


#Draw.io Architecture Block Diagram:
<img width="956" height="551" alt="axi_lite_arch_block drawio" src="https://github.com/user-attachments/assets/25876089-048a-4d3e-88e0-7ea9f99a4e6f" />

#**Whats happening?**
##Testbench
-axi_lite_slave_tb
-Writes to reg_data (slave) with 0xCAFEBABE
-Reads back from reg_data
-Reads from reg_status
-Exercises AXI handshakes from interfaces (out)

##Slave
-FSM handles handshakes
-reg_data stores (W/R) data
-reg_status constant (R)only status
-DECODER selects which register to access

##Waveform
-Should expect (W)reg_data updates
-Should expext (R)rdata values 0xCAFEBABE, 0xDEADBEEF


