 - Ports 65 bits wide or greater are represented as arrays of **uint_32** NOT uint_64! Notice that the next level down **IS** represented as uint_64, and after that, back to uint_32.
 - Ports between 33 and 64 bits wide inclusive are represented as uint_64.
 - Ports between 16 and 32 bits wide inclusive are represented as uint_32.
 - Ports between 2 and 16 bits wide inclusive are represented as uint_16.
 - I believe 1 bit ports are represent as bool.