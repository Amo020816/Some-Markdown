# Sorting

## Preliminaries

Consider this question: What work have sorting done?

We know that 

###  Inversions

#### Definitions

An inversion is a pair of elements that are out of their natural order.

#### Example

Consider a sequence of numbers:
>2 4 1 3 5  

Assume that ascending is the natural order of this sequence.

Then there are 3 inversions in this sequence:
> (2, 1), (4, 1), (4, 3)



## Algorithms

### Heap sorting

#### Application: Top k-th

Although there are some shortcoming, it has its own application scenario.

When we only need top k-th elements of the data ignoring the rest of the data, heap sorting is a clever choice.

Heap sorting can just pick up the top k-th elements without    sorting through all of them.

### Merge sorting

#### Application: Work on disk
