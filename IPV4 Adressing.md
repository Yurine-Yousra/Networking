

| **Class** | **Range**     | **Mask**              | **Network Bits** | **Host Bits** | **Possible Networks** | **Possible Hosts**       | **Example Address**     | **Network Address**   | **Broadcast Address**       | **First Host Address** | **Last Host Address**    |
|-----------|---------------|----------------------|------------------|----------------|-----------------------|--------------------------|-------------------------|-----------------------|-----------------------------|-------------------------|--------------------------|
| A         | 0 - 127      | 255.0.0.0            | 8                | 24             | 2^7 - 2 = 126         | 2^24 - 2 = 16,777,214    | 112.2.3.8 /8            | 112.0.0.0            | 112.255.255.255            | 112.0.0.1              | 112.255.255.254         |
| B         | 128 - 191    | 255.255.0.0          | 16               | 16             | 2^14 = 16,384         | 2^16 - 2 = 65,534        | 136.5.65.7 /16          | 136.5.0.0            | 136.5.255.255             | 136.5.0.1              | 136.5.255.254           |
| C         | 192 - 223    | 255.255.255.0        | 24               | 8              | 2^21 = 2,097,152      | 2^8 - 2 = 254            | 192.186.5.3 /24         | 192.186.5.0          | 192.186.5.255             | 192.186.5.1            | 192.186.5.254           |
| D         | 224 - 239    | 255.255.255.255      | 32               | 0              | N/A                   | N/A                      | N/A                     | N/A                   | N/A                         | N/A                     | N/A                      |
| E         | 240 - 255    | 255.255.255.255      | 32               | 0              | N/A                   | N/A                      | N/A                     | N/A                   | N/A                         | N/A                     | N/A                      |

### Additional Notes:
- Class D is used for multicast addressing and does not have host addresses in the same way as classes A, B, and C.
- Class E is reserved for experimental purposes and is not typically used in public networks.

