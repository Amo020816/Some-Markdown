## Representing and Manipulating Information

在电子计算机中，所有的信息都是靠二进制编码来进行储存。但是一个bit的01是没有办法代表如此多样的信息，所以在访问 `memory` 时，绝大多数的 computer 不会一个一个bit的去获取，而是去访问一个 `8 bits` 的 `byte`。

一个 machine-level 的 `program` 会把 `memory` 看成一段十分长的 `bytes` 数组，称其为 `virtual memory`。 `memory` 中的每一个 `byte` 都由一个唯一的数字表示，称为它的 `address`。 所有可能的 `address` 的集合被称为 `virtual address space`。

> 这里的 `virtual` 指的是这 `address space` 只是一个给 `machine-level program` 参考的概念意象.
> 
> 实际的实现是靠 `dynamic random access memory` , `flase memory`, `disck storage`, 特殊的硬件和 `operating system` 软件的组合去为 `programs` 提供看起来是整体 `bytes` 数组的内容。

`program objects` : `program data`, `instructions` and `control information`

Various mechanisms are used to allocate and manage the storage for different parts of the programs.

This management is all performed within the `virtual address space`.

#### Hexadecimal Notation

In practice, using binary notation to represent `bit pattern` is quiet verbose, while with decimal notation it is too tedious to convert to and from bit pattern.

Instead, we write `bit patterns` as base-16, or hexadecimal numbers. 

>In C, numeric constants starting with `0x` or `0X` are interpreted as being in hexadecimal

##### Reasons:
Hexadecimal notation is concise and easily understood. And conversions between binary and hexadecimal is straightforward. 

##### Rule:
A single hexadecimal number represents four binary digit.

#### Data Size
Every computer has a `word` size, ==indicating the nominal size of pointer data==.

A virtual address is encoded by such a word.

Therefore, the most important system parameter determined by the word size is the ==maximum size of the virtual address space==.

##### Example:
Assuming a machine is with a $w$-bit word size, there is at most $2^w$ bytes can be accessed by the program, while virtual addresses can range from 0 to $2^w - 1$.

That is,
			From  000...000 (w bits)     =       0
			To       111...111 (w bits)   =    $2^w -1$




