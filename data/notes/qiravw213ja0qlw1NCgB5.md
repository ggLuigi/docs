
# Program for changing pid for ts file
This is coded in C++.

## changePid flow
- Find PAT (pid 0 at 12-25 bit)
- find PMT packet in PAT
  - if multiple pmt,
    - find every PMT packets, find if it includes oldPid stream
      - if yes, set that PMT pid, and modified all that PMT packets with oldPid to newPid
## changePmtPid flow
- find PAT -> pid is 0, table id is 0, 
- find all PMT packets by oldPid
- modified all its pid to newPid
## Design of the program logic
PMT packets: (0-base index)
find PAT (pid 0 at 12-25 bit) -> 
    check if there is adaption field control (at 26-27 bit) 
    if (01) -> payload starts at (if payload_unit_start_indicator is 1)40 bit + (check pointer_field number of bytes at 36bit)
    or (if PUSI is 0, start at 36 bit)
    if (11) -> payload starts at 40bit + (check adaptation_field_length 8 bits number of bytes)
    if (10 or 00) skip (cuz no payload)
        PMT pid is at 91-103 bit (assuming only 1 program)
find packets with PMT pid (at 11-23 bit)
    check if there is adaption field control (at 26-27 bit) 
    if (01) -> payload starts at (if payload_unit_start_indicator is 1)40 bit + (check pointer_field number of bytes at 36bit)
    or (if PUSI is 0, start at 36 bit)
    if (11) -> payload starts at 40bit + (check adaptation_field_length 8 bits number of bytes)
    if (10 or 00) skip (cuz no payload)
        set this payload offset
        set pidInfo offset -> payloadOffset + (86+95 bit program_info_length) + program_info_length bytes * 8(if any)
        the pid (to be removed) is at  pidInfo_offset + (11-23bit)
        find if any ES_info_length at pidInfo_offset + (30-39bit number of bytes)
        if (found) remove from pidInfo_offset to ES_info bit
            calculate how many bits are remove -> translate to byte
        need to change section_length -> from payloadOffset + (12-23 bit) to subtract the bits to be removed

## Design of the usage
* Change pid
  * ./program changePid inputfilename --out <> --old oldPid --new newPid
    * (optional) --from packetno --to packetno
* Remove pid
  * ./program removePid inputfilename --out <> pid
    * (optional) --from packetno --to packetno
* Change pmt pid
  * ./program changePmtPid inputfilename --out <> --old oldPid --new newPid
    * (optional) --from packetno --to packetno
