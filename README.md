# ESA5000_Dashboard

## ESA 5000 Protocol

Here are a few messages from the Dashboard:

Message type | Header | Lenght | Destinaiton | Type | Command | Payload              | Checksum
 ----------- | ------ | ------ | ----------- | ---- | ------- | -------------------- | -------
Standby      | 55 AA  | 07     | 25          | 60   | 05      | 04 2B 2B 00 00       | 14 FF
Full Throttle| 55 AA  | 07     | 25          | 60   | 05      | 04 C5 2B 00 00       | 79 FE
Light On     | 55 AA  | 09     | 27          | 63   | 07      | 06 2B 2B 00 00 00 04 | 04 FF

- It appears that the second and third value of the Payload are throttle and brake, ranging from 2B to C5 respectively.
- Lenght, Destination, Type and the Command seem to change when we also pass the Light On message. Perhaps the different Destination, Type and Command point to a different buffer/decoder on the controller, which is able to handle the additional payload bytes
- As for the checksum: "we take the sum all of the previous bytes together except 0x55 & 0xAA. We then preform a XOR operation with 0xFFFF. The resulting value contains ck1 & ck0, respectively."

Here are the responses from the controller:

Message Type          | Header | Lenght | Destination | Type | Command | Payload                    | Checksum
--------------------- | ------ | ------ | ----------- | ---- | ------- | -------------------------- | --------
Standby               | 55 AA  | 0B     | 28          | 6D   | 09      | 00 07 00 00 00 00 00 00 64 | EB FE
Eco                   | 55 AA  | 0B     | 28          | 6D   | 09      | 02 07 00 00 00 00 00 00 64 | AB FE
Full Throttle & Light | 55 AA  | 0B     | 28          | 6D   | 09      | 00 07 01 00 E7 03 00 00 64 | 9A FE

- The controller is only answering every 2-3 times after the dashboard has sent some data
- Eco Mode is mentioned in the first byte of the payload
- Light is mentioned in the third byte
- The current speed is indicated in the fith and sixth byte, ranging from 00 00 (0km/h) to E7 03 (20km/h)
- The current battery is indicated in the last byte in percentage, 64 equals 100%

## Differences to the M365
- No communicative BMS, the ESA has a passive BMS
- Protocoll is more or less identical to the MiniRobot system

## Things to do
- [ ] Get a dump from the purple MiniRobot Dashboard, which has bluetooth connectivity
- [ ] Make a prototype test board to poke comms
