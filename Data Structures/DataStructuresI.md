## Data Structures I (Dan)

* Boole - T/F rules
* Shannon - Mapping Boolean values onto circuits
* Babbage - Theoretical drawing of a machine that can store data and make computations
* Lovelace - Wrote the "code" for creating Babbage machines
* Vacuum tubes could be turned on/off by electricity (by heating up or not heating up), allowing us to use electricity to control logic
* Transistors also use electricity to allow current to pass or not pass (open and close gates) - but there are a lot smaller and easier to build than vacuum tubes
* Turing - Created the idea for a machine that read ticker tape, which had a boolean value on each square, and had a set of rules based on the square's value, the direction its travelling, and what the next square is - allowed him to model any logical possibility. This was just an idea - he didn't build the machine.
* Von Neumann - Built a machine based on Turing's idea - created the first ever CPU

#### Memory
* Memory was originally stored on punch cards (both data and programs)
* Admiral Grace Hopper realized that magnetic tape was better for storing data than punch cards
* A hard drive is a disc of magnetic medal that spins very fast, and the needle jumps to the location that it will read. The needle can jump to any point on the disc - you can read information non-sequentially
* CD - metallic blips - 0/1 is determined by continuation or discontinuation
* SSDs are arrays of transistors
* The memory caches closer to the processor are smaller - the closer the memory, the faster ... but you can't store much there. Macs have more RAM but smaller disks - which makes them feel faster, but you don't have so much space to store things.


#### Storing Info:
* Numbers - JS stores numbers in 64Float (not really great for big or small numbers ...)
* Letters (&words) - Started with ASCII, then UTF-8, and now we use UTF-16
* Colors - bitmaps of red, green, and blue values
* Audio - 16-bits of amplitude

#### Languages
* Compiler - transforms the language we write into the language that can be executed
* Some languages (like C) tell the computer how to alter memory
* Other languages (like JS) provide high-level data types, and the compiler decides what to do

#### Abstract Data Types & Data Structures 
(data structure is implementation of an ADT within a language)
* Array (non-JS) - ordered series of buckets of information that you can insert and remove from
* Stack - Ordered collection of elements - first in, last out
* Queue - First in, last out
* Linked List - nodes that have values and pointer (singly-linked have next value, doubly-linked have previous and next value); main entity has references to head (and possibly tail)