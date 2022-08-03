Reader Protocol 0
=================

This chapter describes the protocol of the reader interface if the given protocol is ``0``.

The interface provides 2 bulk endpoints, one out endpoints for commands and one in endpoint
for command results. Both endpoints support transfers with up to 512 bytes.
Sent to the device is a datastream consisting of the command listed below.

All multi-byte data is transmitted in little-endian format (LSB first)

Limitations
-----------

Because of limitations in the Xilinx USB driver we must prevent issues by filling up all transfer
descriptors in on the device side. To this end every command in the data stream has a
response and the host must await responses before continuing. A command must not be split across 
writes to the device, which effectively limits the size of a command to 512 bytes.

Write Command
-------------

Writes a register::

    struct {
      uint8_t source;
      uint8_t opcode; /* = 0x01 */
      uint16_t address;
      uint32_t value;
    }

Response::

    struct {
      uint8_t original_source;
      uint8_t opcode; /* = 0x01 */
    }

+-------+---------+-------------------------+
| Bits  | Name    | Description             |
+=======+=========+=========================+
| 0:8   | source  | source ID, set to ``0`` |
+-------+---------+-------------------------+
| 8:16  | opcode  | ``0x01``                |
+-------+---------+-------------------------+
| 16:32 | address | address to write        |
+-------+---------+-------------------------+
| 32:64 | value   | value to write          |
+=======+=========+=========================+

Read Command
------------

Reads a single register::

    struct {
      uint8_t source;
      uint8_t opcode; /* = 0x02 */
      uint16_t address;
    }

Response::

    struct {
      uint8_t original_source;
      uint8_t opcode; /* = 0x02 */
      uint32_t value;
    }

