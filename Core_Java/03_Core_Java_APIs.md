## Creating and Manipulating Strings

The String class is such a fundamental class that you’d be hard-pressed to write code with-
out it. After all, you can’t even write a main() method without using the String class. A
string is basically a sequence of characters; here’s an example:

    String name = "Fluffy";
    String name = new String("Fluffy");

Both give you a reference variable of type name pointing to the String object "Fluffy". 
They are subtly different

String class is special and doesn’t need to be instantiated with new

### Concatenation

Placing one String before the other String and combining them together is called string
concatenation

If both operands are numeric, + means numeric addition.
If either operand is a String, + means concatenation
The expression is evaluated left to right

    System.out.println(1 + 2); // 3
    System.out.println("a" + "b"); // ab
    System.out.println("a" + "b" + 3); // ab3
    System.out.println(1 + 2 + "c"); // 3c

     String s = "1"; // s currently holds "1"
     s += "2"; // s currently holds "12"
     s += 3; // s currently holds "123"
     System.out.println(s); // 123

You learned that + is used to do String concatenation in Java. There’s another way,
which isn’t used much on real projects

    String s1 = "1";
    String s2 = s1.concat("2");
    s2.concat("3");
    System.out.println(s2);



### Immutability

Once a String object is created, it is not allowed to change. It cannot be made larger or
smaller, and you cannot change one of the characters inside it

Mutable is another word for changeable. Immutable is the opposite—an object that
can’t be changed once it’s create

### The String Pool

Since strings are everywhere in Java, they use up a lot of memory. In some production appli-
cations, they can use up 25–40 percent of the memory in the entire program. Java realizes
that many strings repeat in the program and solves this issue by reusing common ones. The
string pool, also known as the intern pool, is a location in the Java virtual machine (JVM)
that collects all these strings.

The string pool contains literal values that appear in your program. For example,
“name” is a literal and therefore goes into the string pool. myObject.toString() is a string
but not a literal, so it does not go into the string pool. Strings not in the string pool are gar-
bage collected just like any other object.

Remember back when we said these two lines are subtly different?

    String name = "Fluffy";  // created in string pool
    String name = new String("Fluffy"); // created in JVM heap memory

### Important String Methods

length()

The method length() returns the number of characters in the String. 
The method signature is as follows:

int length()

The following code shows how to use length():

    String string = "animals";
    System.out.println(string.length()); // 7

charAt()

The method charAt() lets you query the string to fi nd out what character is at a specific
index. 

The method signature is as follows:

char charAt(int index)

The following code shows how to use charAt():

    String string = "animals";
    System.out.println(string.charAt(0)); // a
    System.out.println(string.charAt(6)); // s
    System.out.println(string.charAt(7)); // throws exception

Since indexes start counting with 0, charAt(0) returns the “fi rst” character in the
sequence. Similarly, charAt(6) returns the “seventh” character in the sequence. charAt(7)
is a problem. It asks for the “eighth” character in the sequence, but there are only seven
characters present

indexOf()

The method indexOf() looks at the characters in the string and fi nds the first index that
matches the desired value. 

indexOf can work with an individual character or a whole String as input. 

It can also start from a requested position. The method signatures are as follows:

    int indexOf(char ch)
    int indexOf(char ch, index fromIndex)
    int indexOf(String str)
    int indexOf(String str, index fromIndex)

The following code shows how to use indexOf():

    String string = "animals";
    System.out.println(string.indexOf('a')); // 0
    System.out.println(string.indexOf("al")); // 4
    System.out.println(string.indexOf('a', 4)); // 4
    System.out.println(string.indexOf("al", 5)); // -1


substring()

The method substring() also looks for characters in a string. It returns parts of the string.
The first parameter is the index to start with for the returned string. As usual, this is a
zero-based index. There is an optional second parameter, which is the end index you want
to stop at.

    int substring(int beginIndex)
    int substring(int beginIndex, int endIndex)

The following code shows how to use substring():

    String string = "animals";
    System.out.println(string.substring(3)); // mals
    System.out.println(string.substring(string.indexOf('m'))); // mals
    System.out.println(string.substring(3, 4)); // m
    System.out.println(string.substring(3, 7)); // mals

    System.out.println(string.substring(3, 3)); // empty string
    System.out.println(string.substring(3, 2)); // throws exception
    System.out.println(string.substring(3, 8)); // throws exception

toLowerCase() and toUpperCase()

    String toLowerCase(String str)
    String toUpperCase(String str)

The following code shows how to use these methods:

    String string = "animals";
    System.out.println(string.toUpperCase()); // ANIMALS
    System.out.println("Abc123".toLowerCase()); // abc123

equals() and equalsIgnoreCase()

    boolean equals(String str)
    boolean equalsIgnoreCase(String str)

The following code shows how to use these methods:

    System.out.println("abc".equals("ABC")); // false
    System.out.println("ABC".equals("ABC")); // true
    System.out.println("abc".equalsIgnoreCase("ABC")); // true

startsWith() and endsWith()

    boolean startsWith(String prefix)
    boolean endsWith(String suffix)

The following code shows how to use these methods:

    System.out.println("abc".startsWith("a")); // true
    System.out.println("abc".startsWith("A")); // false
    System.out.println("abc".endsWith("c")); // true
    System.out.println("abc".endsWith("a")); // false

contains()

    boolean contains(String str)

The following code shows how to use these methods:

    System.out.println("abc".contains("b")); // true
    System.out.println("abc".contains("B")); // false

replace()

The replace() method does a simple search and replace on the string

    String replace(char oldChar, char newChar)
    String replace(CharSequence oldChar, CharSequence newChar)

The following code shows how to use these methods:

    System.out.println("abcabc".replace('a', 'A')); // AbcAbc
    System.out.println("abcabc".replace("a", "A")); // AbcAbc

trim()

The trim() method removes whitespace from the beginning and end
of a String.

    public String trim()

The following code shows how to use this method:

    System.out.println("abc".trim()); // abc
    System.out.println("\t a b c\n".trim()); // a b c

### Method Chaining

It is common to call multiple methods on the same String, as shown here:

    String start = "AniMaL ";
    String trimmed = start.trim(); // "AniMaL"
    String lowercase = trimmed.toLowerCase(); // "animal"
    String result = lowercase.replace('a', 'A'); // "Animal"
    System.out.println(result);

    String result = "AniMaL ".trim().toLowerCase().replace('a', 'A');
    System.out.println(result);

### Using the StringBuilder Class

A small program can create a lot of String objects very quickly. For example, how many
do you think this piece of code creates?

    10: String alpha = "";
    11: for(char current = 'a'; current <= 'z'; current++)
    12:     alpha += current;
    13: System.out.println(alpha);

The empty String on line 10 is instantiated, and then line 12 appends an "a". However,
because the String object is immutable, a new String object is assigned to alpha and the
“” object becomes eligible for garbage collection. The next time through the loop,
alpha is assigned a new String object, "ab", and the "a" object becomes eligible for garbage
collection. The next iteration assigns alpha to "abc" and the "ab" object becomes eligible
for garbage collection, and so on.

This sequence of events continues, and after 26 iterations through the loop, a total of 27
objects are instantiated, most of which are immediately eligible for garbage collection.

This is very inefficient. Luckily, Java has a solution. The StringBuilder class
creates a String without storing all those interim String values. Unlike the String class,
StringBuilder is not immutable.

    15: StringBuilder alpha = new StringBuilder();
    16: for(char current = 'a'; current <= 'z'; current++)
    17:     alpha.append(current);
    18: System.out.println(alpha);

There are three ways to construct a StringBuilder:

    StringBuilder sb1 = new StringBuilder();
    StringBuilder sb2 = new StringBuilder("animal");
    StringBuilder sb3 = new StringBuilder(10); // reserves slots for characters 

### Important StringBuilder Methods

charAt(), indexOf(), length(), and substring()

These four methods work exactly the same as in the String class:

    StringBuilder sb = new StringBuilder("animals");
    String sub = sb.substring(sb.indexOf("a"), sb.indexOf("al"));
    int len = sb.length();
    char ch = sb.charAt(6);
    System.out.println(sub + " " + len + " " + ch);

append()

The append() method is by far the most frequently used method in StringBuilder

    StringBuilder append(String str)

    StringBuilder sb = new StringBuilder().append(1).append('c');
    sb.append("-").append(true);
    System.out.println(sb); // 1c-true

The insert() method adds characters to the StringBuilder at the requested index and
returns a reference to the current StringBuilder

StringBuilder insert(int offset, String str)

    3: StringBuilder sb = new StringBuilder("animals");
    4: sb.insert(7, "-"); // sb = animals-
    5: sb.insert(0, "-"); // sb = -animals-
    6: sb.insert(4, "-"); // sb = -ani-mals
    7: System.out.println(sb);

delete() and deleteCharAt()

The delete() method is the opposite of the insert() method

    StringBuilder delete(int start, int end)
    StringBuilder deleteCharAt(int index)

The following code shows how to use these methods:

    StringBuilder sb = new StringBuilder("abcdef");
    sb.delete(1, 3); // sb = adef
    sb.deleteCharAt(5); // throws an exception

reverse()

    StringBuilder sb = new StringBuilder("ABC");
    sb.reverse();
    System.out.println(sb);

oString()

The last method converts a StringBuilder into a String. The method signature is as
follows:

    String toString()

The following code shows how to use this method:

    String s = sb.toString()

### StringBuilder vs. StringBuffer

When writing new code that concatenates a lot of String objects together, you should
use StringBuilder. 

StringBuilder was added to Java in Java 5. If you come across older
code, you will see StringBuffer used for this purpose. 

StringBuffer does the same thing but more slowly because it is thread safe

### Understanding Equality

    StringBuilder one = new StringBuilder();
    StringBuilder two = new StringBuilder();
    StringBuilder three = one.append("a");
    System.out.println(one == two); // false
    System.out.println(one == three); // true

Since this example isn’t dealing with primitives, we know to look for whether the
references are referring to the same object.
one and two are both completely separate

StringBuilders, giving us two objects. Therefore, the fi rst print statement gives us false.
three is more interesting. Remember how StringBuilder methods like to return the current reference for chaining? This means one and three both point to the same object and the second print statement gives us true.

    String x = "Hello World";
    String y = "Hello World";
    System.out.println(x == y); // true

Remember that Strings are immutable and literals are pooled. The JVM created only
one literal in memory.x and y both point to the same location in memory; therefore, the
statement outputs true. It gets even trickier. Consider this code.

    String x = "Hello World";
    String z = " Hello World".trim();
    System.out.println(x == z); // false

In this example, we don’t have two of the same String literal. Although
x andz happen to evaluate to the same string, one is computed at runtime. Since it isn’t the same at
compile-time, a new String object is created.

    String x = new String("Hello World");
    String y = "Hello World";
    System.out.println(x == y); // false

Since you have specifically requested a different String object, the pooled value isn’t
shared

    String x = "Hello World";
    String z = " Hello World".trim();
    System.out.println(x.equals(z)); // true

This works because the authors of the String class implemented a standard method
called equals to check the values inside the String rather than the String itself. If a
class doesn’t have an equals method, Java determines whether the references point to the
same object—which is exactly what == does. In case you are wondering, the authors of
StringBuilder did not implement equals(). If you call equals() on two StringBuilder
instances, it will check reference equality


### Understanding Java Arrays












