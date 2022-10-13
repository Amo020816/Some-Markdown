# Overview
This artical is mainly about the difference between C++ and Java or some new characteristics apeared in Java.

# Data Types
There are some data types in both C++ and Java.

Most of them have little differences, instead of arrays and strings.

## String

### Escape character
There are several common ESC in Java.
- \\"  = "
- \\'   = "
- \\\\  = \
- \\n = new line
- \\r  = enter
- \\t  = table

Difference:
- <mark>\\u = a character of unicode</mark>


### Text blocks (Multi-line String)

From Java 13, String can use ``` """....""" ``` to present text blocks.

Example
``` java
public class Main {
    public static void main(String[] args) {
        String s = """
                   SELECT * FROM
                     users
                   WHERE id > 100
                   ORDER BY name DESC
                   """;
        System.out.println(s);
    }
}
```
Output
``` txt
SELECT * FROM
	users 
WHERE id > 100 
ORDER BY name DESC

```

The text blocks above actually have five lines. There is a '\\n' behind the last word 'DECS'.

Four lines:
```Java
String s = """
            SELECT * FROM
                 users
            WHERE id > 100
            ORDER BY name DESC""";
```

Notice that the common space in front of each line will be removed.
i.e.
```Java
String s = """
...........SELECT * FROM
...........  users
...........WHERE id > 100
...........ORDER BY name DESC
...........""";
```
The marked space will be removed.


### String Literal

A main difference of String appeared in Java is the concept of String Literal.

>Each time you create a string literal, the JVM checks the "string constant pool" first. If the string already exists in the pool, a reference to the pooled instance is returned. If the string doesn't exist in the pool, a new string instance is created and placed in the pool. 

For example:
``` Java
  String s1="Welcome";  
  String s2="Welcome";     //It doesn't create a new instance
```
![[java-string.png]]

>In the above example, only one object will be created. Firstly, JVM will not find any string object with the value "Welcome" in string constant pool that is why it will create a new object. After that it will find the string with the value "Welcome" in the pool, it will not create a new object but will return the reference to the same instance.

### Immutability
In Java, <mark><b> String objects are immutable</b></mark> which means the String is unchangable or unmodifiable.

Once String object is created its data or state can't be changed but a new String object is created.

Example:
```Java
class Testimmutablestring{  
public static void main(String args[]){  
  String s="Sachin";  
  s.concat(" Tendulkar");
  //concat() method appends the string at the end
  System.out.println(s);
  //will print Sachin because strings are immutable objects  
   }  
}
```

Output:
``` txt
Sachin
```

>Now it can be understood by the diagram given below. Here Sachin is not changed but a new object is created with Sachin Tendulkar. That is why String is known as immutable.
![[immutable-string-in-java.png]]

As you can see in the above figure that two objects are created but **_s_** reference variable still refers to "Sachin" not to "Sachin Tendulkar".

But if we explicitly assign it to the reference variable, it will refer to "Sachin Tendulkar" object.

For example:
```Java
  class Testimmutablestring1
  {  
   public static void main(String args[])
   {  
     String s="Sachin";  
     s=s.concat(" Tendulkar");  
     System.out.println(s);  
   }
   }
```

Output
``` txt
Sachin Tendulkar
```

### Comparison
We can compare String in Java based on content and reference.

It is used in authentication ( by equals( ) method ), sorting ( by compareTo() method ),  reference matching ( by == operator) etc.

Three ways to compare String in java

- equals() method
- compareTo() method
- == operator

#### 1. equals() method
It is used to compare the original content of the Strings. It compares values of strings for equality.

There are two following methods

- <b> public boolean equals (Object another) </b> compares this string to the specified object.
- <b> public boolean equalsIgnoreCase (String another)</b> compares this string to another string, ignoring case.

Example 1:
``` Java
public class TestStringComparison  
{  
    public static void main( String[] args)  
    {  
        String s1 = "Sachin";  
        String s2 = "Sachin";  
        String s3 = new String("Sachin");  
        String s4 = "Saurav";  
        System.out.println(s1.equals(s2));  
        System.out.println(s1.equals(s3));  
        System.out.println(s1.equals(s4));  
    }  
}
```

Output 1:
``` txt
true
true
false
```

Example 2:
```Java
public class TestStringComparison  
{  
    public static void main( String[] args)  
    {  
        String s1 = "Sachin";  
        String s2 = "SACHIN";  
  
        System.out.println(s1.equals(s2));  
        System.out.println(s1.equalsIgnoreCase(s2));  
    }  
}
```

Output 2:
``` txt
false
true
```

#### 2. == Operator
The == operator compares reference not values.

Example:
``` Java
public class TestStringComparison2  
{  
    public static void main(String[] args)  
    {  
        String s1 = "Sachin";  
        String s2 = "Sachin";  
        String s3 = new String("Sachin");  
  
        System.out.println( s1 == s2);  
        System.out.println( s1 == s3);  
    }  
}
```

Output:
``` txt
true
false
```

#### 3. compareTo() method
Similar to C++, the String class compareTo() method compares values <b><mark>lexicographically</mark></b> and returns an integer value that describes if first string is less than, equal to or greater than second string.

Suppose s1 and s2 are two Strings objects. if, 
- s1 == s2, returns 0
- s1 > s2, return a positive value
- s1 < s2, return a negative value

### Concatenation

#### 1. + String concatenation operator
We can use `+` operator to combine multiple strings into one string. 

Example:

```java
public class Main 
{
    public static void main(String[] args) 
    {
        String s1 = "Hello";
        String s2 = "world";
        String s = s1 + " " + s2 + "!";
        System.out.println(s);
    }
}
```

Output:
``` txt
Hello world!
```

Additionally, + connector can link strings with other data types. During this process, 
==other data tpyes first transform into String , then connect together.==

```Java
public class Main 
{
    public static void main(String[] args) 
    {
        int age = 25;
        String s = "age is " + age;
        System.out.println(s);
    }
}
```

Output:
``` txt
age is 25
```

#### 2. concat() method
The String concat() method concatenates the specified string to the end of current string. Syntax:
``` Java
public String concat(String another)
```

Example:
``` Java
class TestStringConcatenation3
{  
 public static void main(String args[])
 {  
   String s1="Sachin ";  
   String s2="Tendulkar";  
   String s3=s1.concat(s2);  
   System.out.println(s3);     //Sachin Tendulkar  
 }  
}
```

Output:
``` txt
Sachin Tendulkar
```

