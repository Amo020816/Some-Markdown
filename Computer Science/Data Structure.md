# Overview
There are two main question of this topic : 

1. What is data structure?
2. What is algorithm?

## What is data structure?
Data is a bunch of effective information that is collected from the real world or some inputs we received from another functions. 

Then data structure tells us how to organize the information.

 Different problems (or needs) lead to different methods of organizing, that is different data structures.

Some classical data structures : lists, trees, graphs .....

## What is algorithm?
Then it leads to the second question : What is algorithm?

As we discussed above, different needs (or problems) lead to different data structures. ==The final goal is to solve the typical problems.==

> An algorithm is a clearly specified set of simple instructions to be followed to solve a problem.

More intuitively, an algorithm is a effective method of operating the data in order to get the correct(or satisifying) outputs.

## Learn more about algorithm.
Consider an example.

There are serveral methods for humans to calculate multiplication.

- Chinese method
基于九九乘法表，运用竖式运算。

- Russian method
 It is more like the multiplication in computers.  [Russian multiplication](https://baike.baidu.com/item/%E4%BF%84%E7%BD%97%E6%96%AF%E4%B9%98%E6%B3%95/4051312?fr=aladdin)

- Indian method
[Indian multiplication](https://baijiahao.baidu.com/s?id=1624184299783266665&wfr=spider&for=pc)

So there are three main methods of multiplication for humans. Each method can be regarded as an algorithm.

Naturally, there exists a problem : Which algorithm is the best?

Consider that what does "best" mean? 

In daily life, the faster we calculate the better.

Similarly, in the world of computer, speed is also a significant criterion. Another important criterion is space, because computers' memory is limited. 

These are resources the algorithm required.

As a result, if an algorithm costs less time and space,  we could conclude it is a better algorithm.

# Algorithm Analysis
There are two criteria to judge whether an algorithm is good enough.

Naturally, we can arise several questions:
- How to estimate the time required for a program
- How to reduce the running thime of a program from days or years to fractions of a second.

## Asymptotic Algorithm Analysis
In this method, we focus on
- Critical resources (Time and Space)
- Factors affecting running time
- For most algorithms, running time depends on "<b>size</b>" of the input
Runing time is expressed as $T(n)$ for some function $T$ on input size $n$.

Given two functions, there are always points where one function is smaller than the other. So it does not make sense to claim , for instance, $f(N) < g(N)$.

Thus, only the <mark><b>ralative rates of growth</b></mark> is meaningful.

We hope to find that when the input size $N\ \rightarrow\ \infty$, how $T(N)$ grows.


##  Mathematical Background
It is a theoretical issue to analyse the resource use of an algorithm and therefore a formal framework is required.

### Definitions

There are four important mathematical definitions:

#### Definition 1

$$ T(N) = O(f(N))$$

 if there are positive *constants* $c$ and $n_0$ such that $T(N) \leq cf(N)$ when $N \geq n_0$  

#### Definition 2
$$
T(N) = \Omega(g(N))
$$
if there are positive *constants* $c$ and $n_0$ such that $T(N) \geq cg(N)$ when $N \geq n_0$

#### Definition 3
$$
T(n) = \Theta(h(N))
$$
if and only if $T(N) = O(h(N))$ and $T(N) = \Omega(h(N))$.

#### Definition 4
$$
T(N) = o(p(N))
$$
if, for all position constants $c$, there exists an $n_0$ such that $T(N) < cp(N)$ when $N > n_0$. Less formally, $T(N) = o(p(N))$ if $T(N) = O(p(N))$ and $T(N) \neq \Theta(p(N))$.

<mark>The idea of these definition is to establish a relative order among functions</mark>

#### 

When we say that $T(N) = O(f(N))$, we are guaranteeing that the function $T(N)$ grows at a rate no faster than $f(N)$; thus $f(N)$ is an <b>upper bound</b> on $T(N)$. Since this implies that $f(N) = \Omega(T(N))$, we say that $T(N)$ is a <b>lower bound</b> on $f(N)$. 

<font size = 5>Example: </font>

- $N^3$ grows faster than $N^2$, so we can say that $N^2 = O(N^3)$ or $N^3 = \Omega(N^2)$.

- $f(N) = N^2$ and $g(N) = 2N^2$ grow at the same rate, so both $f(N) = O(g(N))$ and $f(N) = \Omega(g(N))$ 

- Intuitively, if $g(N) = 2N^2$, then $g(N) = O(N^4)$, $g(N) = O(N^3)$, and $g(N) = O(N^2)$ are all technically correct, but the last option is the best answer.

- Writing $g(N)\ =\ \Theta(N^2)$ says not only that $g(N) = O(N^2)$ but also the result is as good (tight) as possible.


### Methods to calculate the relative rate of growth

#### Use Rules
##### Rule 1
if $T_1(N) = O(f(N))$ and $T_2(N) = O(g(N))$, then
1. $$T_1(N) + T_2(N)\ =\ O(f(N) + g(N))$$ 
 ( intuitively and less formally it is $O(max(f(N), g(N)))$). 
 2. $$T_1(N) * T_2(N)\ =\ O(f(N)*g(N))$$
##### Rule 2
if $f(N) = O(g(N))$ and $g(N) = O(h(N))$ , then$f(n)\ =\ O(h(N))$

##### Rule 3
If $T(N)$ is a polynomial of degree $k$, then $T(N) = \Theta(N^k)$

##### Rule 4
$log^kN = O(N)$ for any constant $k$. <mark>This tells us that logarithms grow very slowly.</mark>

### By calculating limit
We can always determine the ralative growth rates of two functions $f(N)$ and $g(N)$ by computing
$$
\lim_{N\to \infty} \frac{f(N)}{g(N)}
$$
, using L\`Hopital's law if necessary.

The limit can have four possible values:
- The limit is 0  : $f(N)\ = \ o(g(N))$
- The limit is $c\ \neq\ 0$ : $f(N)\ =\ \Theta(g(N))$
- The limit is $\infty$ : $g(N)\ =\ o(f(N))$
- The limit does not exist : There is no relation

## Model
To run an algorithm, we need a computer. However, different computers have different sizes of memory and different abilities of running a program.

So, it is high time that we should develop a ideal model of computers so that we can analyze an algorithm generally.

Consider a ideal computer that:
- <b>has standarad repertoire of simple instructions</b>, such as addition, multiplication, comparision and assignment.
- <b>takes excatly one time unit to do anything simple.</b>
- <b>has fix-sized integers</b>
- <b>has infinite memory</b>

Notice that this ideal computer has some weakness.
- In real life, <b>not all operations take exactly the same time</b>.
In particular, in our model, one disk reads counts the same as an addition, even though the addition is typically several orders of magnitude faster.
- By assuming infinite memory, we ignore <b>the fact that cost of a memory access can increase when slower memory is used due to larger memory requirements</b>.

## What to analyze
There are three $T(N)$s that we are interested in :
- The best case $T_{best}(N)$
- The average case $T_{avg}(N)$
- The worst case $T_{worst}(N)$

Average-case performance often reflects typical behavior, while worst-case performance represents a guarantee for performance on any possible input.

##
As an example, we shall consider this problem:

### Maxium Subsequence Sum problem
> Given (possibly negative) integers  $A_1,\ A_2,\ \dots\  ,A_N$, find the maxium value of $\sum_{k=i}^j A_k$.



# Lists
## (Java version)
>(reference to https://joshhug.gitbooks.io/hug61b/content/chap2)
### IntList

First we build up the most basic list class.
``` Java
public class IntList {
    public int first;
    public IntList rest;        

    public IntList(int f, IntList r) {
        first = f;
        rest = r;
    }
}
```

Though it is ugly to use. If we want to make a list of the numbers 5, 10, 15, we can either do

``` Java
IntList L = new IntList(5, null);
L.rest = new IntList(10, null);
L.rest.rest = new IntList(15, null);
```

Alternativly, we can build the list backwards.
``` java
IntList L = new IntList(15, null);
L = new IntList(10, L);
L = new IntList(5, L);
```

It seems that IntList lacks of the ability to do some tasks.

Next, we'll adopt the usual object oriented programming strategy of adding helper methods to our class to perform some basic tasks.

#### size() and iterativeSize()

We'd like to add a method `size` to the `IntList` class so that if you call `L.size()`, you can know the numbers of items in `L`.

Consider writing a `size` and `iterativeSize` method. `size` should use recursion, and `iterativeSize` should not.

``` Java
/** Return the size of the list using... recursion! **/
public int size() {
    if (rest == null) {
        return 1;
    }
    return 1 + this.rest.size();
}
```

The key thing  to remember about recursive code is that you neeed a base case.

In the case, to get the base case, we first consider what do we know about the size of the simplest list.

It's natural we know that when `rest` is `null`, the list just contains a single element, where the size is 1.

<mark>Here is a question</mark>: why don't we do something like `if (this == null) return 0;` instead.

<mark>Answer</mark>: Think about what happens when we call `size`. We are calling it on an objest, for example, `L.size()`. If L were `null`, then NullPointer error results.

Next is `iterativeSize` method:
``` Java
/** Return the size of the list using no recursion! */
public int iterativeSize() {
    IntList p = this;
    int totalSize = 0;
    while (p != null) {
        totalSize += 1;
        p = p.rest;
    }
    return totalSize;
}
```

We needs a pointer because we can't reassign `this` in Java.

Note that both `size` and `iterativeSize` take linear time which is $T(N)\  =\  \Theta(N)$.  It's too slow for a huge list.

It's high time that we should delve deeper into the list and improve it.

### SLList

In the last section, we built the `IntList` class, a list data structure that can technically do all the things a list can do.

However, in practice, it is too awkward for users to use, resulting in code that is hard to read and maintain.

Alternatively, it also leaves the data unsafe, because all the `Node`s of the list are public. 

Third, in order to use `IntList` correctly, the programmer must understand and utilize recursion even for simple list related tasks.

Next we'll build a new class `SLList` (Singly-linked List), which much more closely resembles the implementations.

#### Review IntList

We can easily notice that our class `IntList` acutally is not a real list. It is more like a `Node` in a list, where `first` is data or objects and `rest` is the next node indeed.

So, we can rename the `IntList` and its member variables and throw away all the helper methods:

``` Java
public class IntNode {
    public int item;
    public IntNode next;

    public IntNode(int i, IntNode n) {
        item = i;
        next = n;
    }
}
```

#### Black-Box it

Knowing that `IntNode` is hard to use, we are going to put `IntNode` into a black box. That is, we leave `IntNode` behind the scene and users can not interact with it. 

The only thing `IntNode` needs to deal with is to store some necessary information for us.

Thus, we need another interface for users to interact with this black box. We're going to create a sperate class named `SLList` for users.

The basic class is shown below:
``` Java
public class SLList {
    public IntNode first;

    public SLList(int x) {
        first = new IntNode(x, null);
    }
}
```

Compare the creation of an `IntList` of one item and the creation of a `SLList` :
``` Java
IntList L1 = new IntList(5, null);
SLList L2  = new SLList(5);
```

The `SLList` hides the detail that there exists a null link from the user. We sucessfully put the complex implementation of create a `List` into a black box.

Yet our `SLList` lacks of some helper methods.

#### addFirst and getFirst

`addFirst` is straightforward:
``` JAVA
    /** Adds an item to the front of the list. **/
    public void addFirst(int x) {
        first = new IntNode(x, first);
    }
```

Just like we did  `L = new IntList((something), L);` in the last section.

`getFirst` is much easier:
``` Java
/** Retrieves the front item from the list. */
public int getFirst() {
    return first.item;
}
```

The resulting `SLList` is much easier to use. Compare:
``` Java
SLList L = new SLList(15);
L.addFirst(10);
L.addFirst(5);
int x = L.getFirst();
```

to the  `IntList` :
``` Java
IntList L = new IntList(15, null);
L = new IntList(10, L);
L = new IntList(5, L);
int x = L.first;
```

> reference to https://joshhug.gitbooks.io/hug61b/content/chap2/chap22.html
![[IntList_vs_SLList.png]]

In `SLList`, we put the `Node`s behind the `SLList` class.

>Essentially, the `SLList` class acts as a middleman between the list user and the naked recursive data structure.

The resulting class not only is much easier to grasp and use, but also provide a possibility to make the data safe.

#### Private data member

Till this moment, our data is still exposed to outer world. A programmer can easily modify the `Node`s directly, where some unexpected errors may occur.

Check this:
``` Java
SLList L = new SLList(15);
L.addFirst(10);
L.first.next.next = L.first.next;
```

> reference to https://joshhug.gitbooks.io/hug61b/content/chap2/chap22.html
> ![[bad_SLList.png]]
> This results in a malformed list with an infinite loop.

How to solve this problem?

The most resonable way is to make our `first` data member private, i.e.

``` Java
public class SLList {
    private IntNode first;
...
```

>Private variables and methods can only be accessed by codes inside the same .java file.

Private keyword makes our data safer, and

>In large software engineering projects, the `private` keyword is an invaluable signal that certain pieces of code should be ignored (and thus need not be understood) by the end user.

#### Nested class
At the moment, we have two `.java` files: `IntNode` and `SLList`. However, the `IntNode` is just a supporting characteristic in `SLList`.

In this situation, we can embed `IntNode` class into the `SLList` class.

``` Java
public class SLList {
       public class IntNode {
            public int item;
            public IntNode next;
            public IntNode(int i, IntNode n) {
                item = i;
                next = n;
            }
       }

       private IntNode first; 

       public SLList(int x) {
           first = new IntNode(x, null);
       } 
...
```


