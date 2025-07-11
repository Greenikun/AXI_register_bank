# AXI_register_bank
lightweight AXI4-Lite slave peripheral in Verilog that exposes a small register bank to a master

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


#Architecture Block Diagram:
          +-------------------------+
AXI AWADDR|                         |AXI ARADDR
AXI WDATA |    AXI-LITE SLAVE      |AXI RDATA
----------> Register Bank Interface <----------
          |                         |
          | 0x00 → REG_DATA (R/W)   |
          | 0x04 → REG_STATUS (R)   |
          +-------------------------+
