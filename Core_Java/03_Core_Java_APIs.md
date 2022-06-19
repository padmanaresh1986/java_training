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


## Understanding Java Arrays

String and StringBuilder classes are implemented using an array of characters

An array is an area of memory on the heap with space for a designated number of elements

A String is implemented as an array with some methods that you might want to use when dealing with
characters specifically

A StringBuilder is implemented as an array where the array object is replaced with a new bigger array object when it runs out of space to store all the characters

### Creating an Array of Primitives

The most common way to create an array looks like this:

    int[] numbers1 = new int[3];

When using this form to instantiate an array, set all the elements to the default value for
that type

Another way to create an array is to specify all the elements it should start out with:

    int[] numbers2 = new int[] {42, 55, 99}

Java recognizes that this expression is redundant. Since you are specifying the type of
the array on the left side of the equal sign, Java already knows the type. And since you
are specifying the initial values, it already knows the size. As a shortcut, Java lets you
write this:

    int[] numbers2 = {42, 55, 99};

This approach is called an anonymous array. It is anonymous because you don’t specify
the type and size.

Finally, you can type the [] before or after the name, and adding a space is optional.
This means that all four of these statements do the exact same thing:

    int[] numAnimals;
    int [] numAnimals2;
    int numAnimals3[];
    int numAnimals4 [];

### Creating an Array with Reference Variables

You can choose any Java type to be the type of the array. This includes classes you create
yourself. Let’s take a look at a built-in type with String:

    public class ArrayType {
        public static void main(String args[]) {
            String [] bugs = { "cricket", "beetle", "ladybug" };
            String [] alias = bugs;
            System.out.println(bugs.equals(alias)); // true
            System.out.println(bugs.toString()); // [Ljava.lang.String;@160bc7c0
        } 
    }

We can call equals() because an array is an object

### Using an Array

    4: String[] mammals = {"monkey", "chimp", "donkey"};
    5: System.out.println(mammals.length); // 3
    6: System.out.println(mammals[0]); // monkey
    7: System.out.println(mammals[1]); // chimp
    8: System.out.println(mammals[2]); // donkey

What is the length of this array 

    String[] birds = new String[6];
    System.out.println(birds.length);

The answer is 6. Even though all 6 elements of the array are null, there are still 6 of
them. length does not consider what is in the array; it only considers how many slots have
been allocated.

It is very common to use a loop when reading from or writing to an array. This loop sets
each element of number to 5 higher than the current index:

    5: int[] numbers = new int[10];
    6: for (int i = 0; i < numbers.length; i++)
    7: numbers[i] = i + 5;

### Sorting

use Arrays class to sort the array 

    int[] numbers = { 6, 9, 1 };
    Arrays.sort(numbers);
    for (int i = 0; i < numbers.length; i++)
       System.out.print (numbers[i] + " ");

    String[] strings = { "10", "9", "100" };
    Arrays.sort(strings);
    for (String string : strings)
        System.out.print(string + " ");

### Searching

Java also provides a convenient way to search—but only if the array is already sorted

    3: int[] numbers = {2,4,6,8};
    4: System.out.println(Arrays.binarySearch(numbers, 2)); // 0
    5: System.out.println(Arrays.binarySearch(numbers, 4)); // 1
    6: System.out.println(Arrays.binarySearch(numbers, 1)); // -1
    7: System.out.println(Arrays.binarySearch(numbers, 3)); // -2
    8: System.out.println(Arrays.binarySearch(numbers, 9)); // -5

### Varargs

When creating an array yourself, it looks like what we’ve seen thus far. When one is passed
to your method, there is another way it can look. Here are three examples with a main()
method:

    public static void main(String[] args)
    public static void main(String args[])
    public static void main(String... args) // varargs

The third example uses a syntax called varargs (variable arguments)

### Multidimensional Arrays

Multiple array separators are all it takes to declare arrays with multiple dimensions

    int[][] vars1; // 2D array
    int vars2 [][]; // 2D array
    int[] vars3[]; // 2D array
    int[] vars4 [], space [][]; // a 2D AND a 3D array

You can specify the size of your multidimensional array in the declaration if you like

    String [][] rectangle = new String[3][2];

The result of this statement is an array rectangle with three elements, each of which
refers to an array of two elements. 

You can think of the addressable range as [0][0] through [2][1], 
but don’t think of it as a structure of addresses like [0,0] or [2,1].

Now suppose we set one of these values:
rectangle[0][1] = "set";

While that array happens to be rectangular in shape, an array doesn’t need to be.

    int[][] differentSize = {{1, 4}, {3}, {9,8,7}};

Another way to create an asymmetric array is to initialize just an array’s fi rst dimension,
and defi ne the size of each array component in a separate statement

    int [][] args = new int[4][];
    args[0] = new int[5];
    args[1] = new int[3];

### Using a Multidimensional Array

The most common operation on a multidimensional array is to loop through it. This example
prints out a 2D array:

    int[][] twoD = new int[3][2];
        for (int i = 0; i < twoD.length; i++) {
            for (int j = 0; j < twoD[i].length; j++)
                System.out.print(twoD[i][j] + " "); // print element
        System.out.println(); // time for a new row
    }

We have two loops here. The first uses index i and goes through the first subarray for
twoD.

The second uses a different loop variable j. It is very important these be different
variable names so the loops don’t get mixed up. The inner loop looks at how many elements
are in the second-level array

This entire exercise would be easier to read with the enhanced for loop.

    for (int[] inner : twoD) {
        for (int num : inner)
            System.out.print(num + " ");
        System.out.println();
    }

### Understanding an ArrayList

An array has one glaring shortcoming: you have to know how many elements will be in the
array when you create it and then you are stuck with that choice.

Just like a StringBuilder, ArrayList can change size at runtime as needed. Like an array, an ArrayList is an ordered
sequence that allows duplicates.

### Creating an ArrayList

    ArrayList list1 = new ArrayList(); // default number of elements
    ArrayList list2 = new ArrayList(10); // specific number of slots
    ArrayList list3 = new ArrayList(list2); // make copy of another list

the new and improved way. Java 5 introduced generics, which allow you to specify the type
of class that the ArrayList will contain.

    ArrayList<String> list4 = new ArrayList<String>();
    ArrayList<String> list5 = new ArrayList<>()

Java 5 allows you to tell the compiler what the type would be by specifying it between <
and >. Starting in Java 7, you can even omit that type from the right side.

ArrayList implements an interface called List. In other words, an ArrayList is a List

    List<String> list6 = new ArrayList<>();
    ArrayList<String> list7 = new List<>(); // DOES NOT COMPILE

### Using an ArrayList

ArrayList has many methods, but you only need to know a handful of them—even fewer
than you did for String and StringBuilder.

add()

The add() methods insert a new value in the ArrayList. The method signatures are as
follows:

    boolean add(E element)
    void add(int index, E element)

Don’t worry about the boolean return value. It always returns true. It is there because
other classes in the collections family need a return value in the signature when adding an
element.

    ArrayList list = new ArrayList();
    list.add("hawk"); // [hawk]
    list.add(Boolean.TRUE); // [hawk, true]
    System.out.println(list); // [hawk, true]

add() does exactly what we expect: it stores the String in the no longer empty
ArrayList. It then does the same thing for the boolean. This is okay because we didn’t
specify a type for ArrayList; therefore, the type is Object, which includes everything
except primitives. It may not have been what we intended, but the compiler doesn’t know
that. Now, let’s use generics to tell the compiler we only want to allow String objects in
our ArrayList:

    ArrayList<String> safer = new ArrayList<>();
    safer.add("sparrow");
    safer.add(Boolean.TRUE); // DOES NOT COMPILE

This time the compiler knows that only String objects are allowed in and prevents the
attempt to add a boolean. Now let’s try adding multiple values to different positions.

    4: List<String> birds = new ArrayList<>();
    5: birds.add("hawk"); // [hawk]
    6: birds.add(1, "robin"); // [hawk, robin]
    7: birds.add(0, "blue jay"); // [blue jay, hawk, robin]
    8: birds.add(1, "cardinal"); // [blue jay, cardinal, hawk, robin]
    9: System.out.println(birds); // [blue jay, cardinal, hawk, robin]

remove()

The remove() methods remove the first matching value in the ArrayList or remove the
element at a specified index. The method signatures are as follows:

    boolean remove(Object object)
    E remove(int index)

This time the boolean return value tells us whether a match was removed. The E return
type is the element that actually got removed. The following shows how to use these
methods:

    3: List<String> birds = new ArrayList<>();
    4: birds.add("hawk"); // [hawk]
    5: birds.add("hawk"); // [hawk, hawk]
    6: System.out.println(birds.remove("cardinal")); // prints false
    7: System.out.println(birds.remove("hawk")); // prints true
    8: System.out.println(birds.remove(0)); // prints hawk
    9: System.out.println(birds); // []

set()

The set() method changes one of the elements of the ArrayList without changing the size.
The method signature is as follows:

    E set(int index, E newElement)

The E return type is the element that got replaced. The following shows how to use this
method:

    15: List<String> birds = new ArrayList<>();
    16: birds.add("hawk"); // [hawk]
    17: System.out.println(birds.size()); // 1
    18: birds.set(0, "robin"); // [robin]
    19: System.out.println(birds.size()); // 1
    20: birds.set(1, "robin"); // IndexOutOfBoundsException

isEmpty() and size()

The isEmpty() and size() methods look at how many of the slots are in use. 
The method signatures are as follows:

    boolean isEmpty()
    int size()

The following shows how to use these methods:

    System.out.println(birds.isEmpty()); // true
    System.out.println(birds.size()); // 0
    birds.add("hawk"); // [hawk]
    birds.add("hawk"); // [hawk, hawk]
    System.out.println(birds.isEmpty()); // false
    System.out.println(birds.size()); // 2

clear()

The clear() method provides an easy way to discard all elements of the ArrayList.
The method signature is as follows:

    void clear()

The following shows how to use this method:

    List<String> birds = new ArrayList<>();
    birds.add("hawk"); // [hawk]
    birds.add("hawk"); // [hawk, hawk]
    System.out.println(birds.isEmpty()); // false
    System.out.println(birds.size()); // 2
    birds.clear(); // []
    System.out.println(birds.isEmpty()); // true
    System.out.println(birds.size()); // 0

contains()

The contains() method checks whether a certain value is in the ArrayList. 

The method signature is as follows:

    List<String> birds = new ArrayList<>();
    birds.add("hawk"); // [hawk]
    System.out.println(birds.contains("hawk")); // true
    System.out.println(birds.contains("robin")); // false

This method calls equals() on each element of the ArrayList to see whether there are any matches. Since String implements equals(), this works out wellains(Object object)

equals()

Finally, ArrayList has a custom implementation of equals() so you can compare two lists
to see if they contain the same elements in the same order.

boolean equals(Object object)

    31: List<String> one = new ArrayList<>();
    32: List<String> two = new ArrayList<>();
    33: System.out.println(one.equals(two)); // true
    34: one.add("a"); // [a]
    35: System.out.println(one.equals(two)); // false
    36: two.add("a"); // [a]
    37: System.out.println(one.equals(two)); // true
    38: one.add("b"); // [a,b]
    39: two.add(0, "b"); // [b,a]
    40: System.out.println(one.equals(two)); // false


## Wrapper Classes

Up to now, we’ve only put String objects in the ArrayList. What happens if we want to
put primitives in? Each primitive type has a wrapper class, which is an object type that
corresponds to the primitive.


| Primitive type     | Wrapper class        | Example of constructing |
| ----------- | ----------- | ------- |
| boolean      | Boolean       |    new Boolean(true)     |
| byte   |  Byte   |  new Byte((byte) 1)  |
| short   | Short    |  new Short((short) 1) |
|  int  |  Integer   | new Integer(1)  |
|  long  | Long    |  new Long(1) |
|  float  | Float    | new Float(1.0)  |
|  double  | Double    | new Double(1.0)  |
|   char |  Character   |  new Character('c') |

The wrapper classes also have a method that converts back to a primitive intValue() , autoboxing will take care of it 

There are also methods for converting a String to a primitive or wrapper class

There are also methods for converting a String to a primitive or wrapper class. You do
need to know these methods. The parse methods, such as parseInt(), return a primitive,
and the valueOf() method returns a wrapper class

    int primitive = Integer.parseInt("123");
    Integer wrapper = Integer.valueOf("123");

The first line converts a String to an int primitive. The second converts a String to an
Integer wrapper class. If the String passed in is not valid for the given type, Java throws
an exception. 

In these examples, letters and dots are not valid for an integer value:


    int bad1 = Integer.parseInt("a"); // throws NumberFormatException
    Integer bad2 = Integer.valueOf("123.45"); // throws NumberFormatException


| Wrapper class     | Converting String to primitive  | Converting String to wrapper class |
| ----------- | ----------- | ------- |
| Boolean      | Boolean.parseBoolean("true");       |    Boolean.valueOf("TRUE");     |
|  Byte     |  Byte.parseByte("1");    |   Byte.valueOf("2");   |
|   Short    |  Short.parseShort("1");    |  Short.valueOf("2");    |
|   Integer    | Integer.parseInt("1");      |  Integer.valueOf("2");    |
|    Long   |   Long.parseLong("1");    |   Long.valueOf("2");   |
|Float|Float.parseFloat("1"); |Float.valueOf("2.2");    |
|Double|Double.parseDouble("1");|Double.valueOf("2.2");|
|Character|None|None|


### Autoboxing

Since Java 5, you can just type the primitive value and Java will convert it to the
relevant wrapper class for you. This is called autoboxing. Let’s look at an example:

    4: List<Double> weights = new ArrayList<>();
    5: weights.add(50.5); // [50.5]
    6: weights.add(new Double(60)); // [50.5, 60.0]
    7: weights.remove(50.5); // [60.0]
    8: double first = weights.get(0); // 60.0

What do you think happens if you try to unbox a null?

    3: List<Integer> heights = new ArrayList<>();
    4: heights.add(null);
    5: int h = heights.get(0); // NullPointerException

Be careful when autoboxing into Integer. What do you think this code outputs?

    List<Integer> numbers = new ArrayList<>();
    numbers.add(1);
    numbers.add(2);
    numbers.remove(1);
    System.out.println(numbers);

It actually outputs 1. After adding the two values, the List contains [1, 2]. We then request
the element with index 1 be removed. That’s right: index 1. Because there’s already a remove()
method that takes an int parameter, Java calls that method rather than autoboxing. If you
want to remove the 2, you can write numbers.remove(new Integer(2)) to force wrapper
class use.

### Converting Between array and List

You should know how to convert between an array and an ArrayList. Let’s start with
turning an ArrayList into an array:

    3: List<String> list = new ArrayList<>();
    4: list.add("hawk");
    5: list.add("robin");
    6: Object[] objectArray = list.toArray();
    7: System.out.println(objectArray.length); // 2
    8: String[] stringArray = list.toArray(new String[0]);
    9: System.out.println(stringArray.length); // 2

Converting from an array to a List is more interesting. The original array and created
array backed List are linked

When a change is made to one, it is available in the other. It
is a fi xed-size list and is also known a backed List because the array changes with it. Pay
careful attention to the values here:

    20: String[] array = { "hawk", "robin" }; // [hawk, robin]
    21: List<String> list = Arrays.asList(array); // returns fixed size list
    22: System.out.println(list.size()); // 2
    23: list.set(1, "test"); // [hawk, test]
    24: array[0] = "new"; // [new, test]
    25: for (String b : array) System.out.print(b + " "); // new test
    26: list.remove(1); // throws UnsupportedOperation Exception

Line 21 converts the array to a List. Note that it isn’t the java.util.ArrayList we’ve
grown used to. It is a fi xed-size, backed version of a List. Line 23 is okay because set()
merely replaces an existing value. It updates both array and list because they point to the
same data store. Line 24 also changes both array and
list. Line 25 shows the array has changed to new test. Line 26 throws an exception because we are not allowed to change the size of the list.

### A Cool Trick with Varargs

asList() takes varargs, which let you pass in an array or just type out the String values.
This is handy when testing because you can easily create and populate a List on one line.

    List<String> list = Arrays.asList("one", "two");

### Sorting

Sorting an ArrayList is very similar to sorting an array. You just use a different helper class:

    List<Integer> numbers = new ArrayList<>();
    numbers.add(99);
    numbers.add(5);
    numbers.add(81);
    Collections.sort(numbers);
    System.out.println(numbers); [5, 81, 99]

## Working with Dates and Times

You need an import statement to work with the date and time classes.
Most of them are in the java.time package. 

To use it, add this import to your program:
    import java.time.*; // import time classes

### Creating Dates and Times

LocalDate Contains just a date—no time and no time zone. 

LocalTime Contains just a time—no date and no time zone.

LocalDateTime Contains both a date and time but no time zone. 

If you do need to communicate across time zones, ZonedDateTime handles them.

    System.out.println(LocalDate.now());
    System.out.println(LocalTime.now());
    System.out.println(LocalDateTime.now());

    2015-01-20
    12:45:18.401
    2015-01-20T12:45:18.401

The key is to notice the type of information in the output. The first one contains only a
date and no time. The second contains only a time and no date. This time displays hours,
minutes, seconds, and nanoseconds. The third contains both date and time. Java uses T to
separate the date and time when converting LocalDateTime to a String.

Now that you know how to create the current date and time, let’s look at other specific
dates and times. To begin, let’s create just a date with no time. Both of these examples
create the same date:

    LocalDate date1 = LocalDate.of(2015, Month.JANUARY, 20);
    LocalDate date2 = LocalDate.of(2015, 1, 20);

The method signatures are as follows:

    public static LocalDate of(int year, int month, int dayOfMonth)
    public static LocalDate of(int year, Month month, int dayOfMonth)

When creating a time, you can choose how detailed you want to be. You can specify just
the hour and minute, or you can add the number of seconds. You can even add nanosec-
onds if you want to be very precise

    LocalTime time1 = LocalTime.of(6, 15); // hour and minute
    LocalTime time2 = LocalTime.of(6, 15, 30); // + seconds
    LocalTime time3 = LocalTime.of(6, 15, 30, 200); // + nanoseconds

These three times are all different but within a minute of each other. The method signa-
tures are as follows

    public static LocalTime of(int hour, int minute)
    public static LocalTime of(int hour, int minute, int second)
    public static LocalTime of(int hour, int minute, int second, int nanos)

Finally, we can combine dates and times

    LocalDateTime dateTime1 = LocalDateTime.of(2015, Month.JANUARY, 20, 6, 15, 30);
    LocalDateTime dateTime2 = LocalDateTime.of(date1, time1);

This time there are a lot of method signatures since there are more combinations. The
method signatures are as follows:

    public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute)
    public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second)
    public static LocalDateTime of(int year, int month, int dayOfMonth, int hour, int minute, int second, int nanos)
    public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute)
    public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second)
    public static LocalDateTime of(int year, Month month, int dayOfMonth, int hour, int minute, int second, int nanos)
    public static LocalDateTime of(LocalDate date, LocalTime)

Did you notice that we did not use a constructor in any of the examples? The date and
time classes have private constructors to force you to use the static methods

    LocalDate d = new LocalDate(); // DOES NOT COMPILE

what happens when you pass invalid numbers to of()

    LocalDate.of(2015, Month.JANUARY, 32) // throws DateTimeException

### Creating Dates in Java 7 and Earlier

| | Old way     | New way (Java 8 and later)  | 
|------| ----------- | ----------- | 
| Importing | import java.util.*;   | import java.time.*; | 
| Creating an object with the current date | Date d = new Date();| LocalDate d = LocalDate .now(); | 
| Creating an object with the current date and time | Date d = new Date();   | LocalDateTime dt = LocalDateTime.now(); | 
| Creating an object representing January 1, 2015 | Calendar c = Calendar.getInstance(); c.set(2015, Calendar.JANUARY, 1); Date jan = c.getTime();   | LocalDate jan = LocalDate.of(2015, Month.JANUARY, 1); | 
| Creating January 1, 2015 without the constant| Calendar c = Calendar.getInstance(); c.set(2015, 0, 1); Date jan = c.getTime();   | LocalDate jan = LocalDate.of(2015, 1, 1) | 

### Manipulating Dates and Times

Adding to a date is easy. The date and time classes are immutable, just like String was.
This means that we need to remember to assign the results of these methods to a reference
variable so they are not lost.

    12: LocalDate date = LocalDate.of(2014, Month.JANUARY, 20);
    13: System.out.println(date); // 2014-01-20
    14: date = date.plusDays(2);
    15: System.out.println(date); // 2014-01-22
    16: date = date.plusWeeks(1);
    17: System.out.println(date); // 2014-01-29
    18: date = date.plusMonths(1);
    19: System.out.println(date); // 2014-02-28
    20: date = date.plusYears(5);
    21: System.out.println(date); // 2019-02-28

There are also nice easy methods to go backward in time. This time, let’s work with
LocalDateTime.

    22: LocalDate date = LocalDate.of(2020, Month.JANUARY, 20);
    23: LocalTime time = LocalTime.of(5, 15);
    24: LocalDateTime dateTime = LocalDateTime.of(date, time);
    25: System.out.println(dateTime); // 2020-01-20T05:15
    26: dateTime = dateTime.minusDays(1);
    27: System.out.println(dateTime); // 2020-01-19T05:15
    28: dateTime = dateTime.minusHours(10);
    29: System.out.println(dateTime); // 2020-01-18T19:15
    30: dateTime = dateTime.minusSeconds(30);
    31: System.out.println(dateTime); // 2020-01-18T19:14:30

It is common for date and time methods to be chained. For example, without the print
statements, the previous example could be rewritten as follows:

    LocalDate date = LocalDate.of(2020, Month.JANUARY, 20);
    LocalTime time = LocalTime.of(5, 15);
    LocalDateTime dateTime = LocalDateTime.of(date2, time).minusDays(1).minusHours(10).minusSeconds(30);

### Working with Periods

Now we know enough to do something fun with dates! Our zoo performs animal enrich-
ment activities to give the animals something fun to do. The head zookeeper has decided
to switch the toys every month. This system will continue for three months to see how it
works out

    public static void main(String[] args) {
        LocalDate start = LocalDate.of(2015, Month.JANUARY, 1);
        LocalDate end = LocalDate.of(2015, Month.MARCH, 30);
        performAnimalEnrichment(start, end);
    }
    private static void performAnimalEnrichment(LocalDate start, LocalDate end) {
        LocalDate upTo = start;
        while (upTo.isBefore(end)) { // check if still before end
            System.out.println("give new toy: " + upTo);
            upTo = upTo.plusMonths(1); // add a month
        }
    }

Java has a Period class that we can pass in. This code does the same thing as
the previous example:

    public static void main(String[] args) {
        LocalDate start = LocalDate.of(2015, Month.JANUARY, 1);
        LocalDate end = LocalDate.of(2015, Month.MARCH, 30);
        Period period = Period.ofMonths(1); // create a period
        performAnimalEnrichment(start, end, period);
    }
    private static void performAnimalEnrichment(LocalDate start, LocalDate end,
    Period period) { // uses the generic period
        LocalDate upTo = start;
        while (upTo.isBefore(end)) {
        System.out.println("give new toy: " + upTo);
        upTo = upTo.plus(period); // adds the period
    }}

The method can add an arbitrary period of time that gets passed in. This allows us to
reuse the same method for different periods of time as our zookeeper changes her mind.
There are five ways to create a Period class

    Period annually = Period.ofYears(1); // every 1 year
    Period quarterly = Period.ofMonths(3); // every 3 months
    Period everyThreeWeeks = Period.ofWeeks(3); // every 3 weeks
    Period everyOtherDay = Period.ofDays(2); // every 2 days
    Period everyYearAndAWeek = Period.of(1, 0, 7); // every year and 7 days

You’ve probably noticed by now that a Period is a day or more of time. There is also
Duration, which is intended for smaller units of time. For Duration, you can specify the
number of days, hours, minutes, seconds, or nanoseconds.


The last thing to know about Period is what objects it can be used with.
Let’s look at some code:

    3: LocalDate date = LocalDate.of(2015, 1, 20);
    4: LocalTime time = LocalTime.of(6, 15);
    5: LocalDateTime dateTime = LocalDateTime.of(date, time);
    6: Period period = Period.ofMonths(1);
    7: System.out.println(date.plus(period)); // 2015-02-20
    8: System.out.println(dateTime.plus(period)); // 2015-02-20T06:15
    9: System.out.println(time.plus(period)); // UnsupportedTemporalTypeException


### Converting to a long

LocalDate and LocalDateTime have a method to convert them into long equivalents in rela-
tion to 1970. What’s special about 1970? That’s what UNIX started using for date standards,
so Java reused it.

LocalDate has toEpochDay(), which is the number of days since January 1, 1970.
LocalDateTime has toEpochTime(), which is the number of seconds since January 1, 1970.


### Formatting Dates and Times

The date and time classes support many methods to get data out of them:

LocalDate date = LocalDate.of(2020, Month.JANUARY, 20);
System.out.println(date.getDayOfWeek()); // MONDAY
System.out.println(date.getMonth()); // JANUARY
System.out.println(date.getYear()); // 2020
System.out.println(date.getDayOfYear()); // 20

We could use this information to display information about the date. However, it would
be more work than necessary. Java provides a class called DateTimeFormatter to help us
out

    LocalDate date = LocalDate.of(2020, Month.JANUARY, 20);
    LocalTime time = LocalTime.of(11, 12, 34);
    LocalDateTime dateTime = LocalDateTime.of(date, time);

    System.out.println(date.format(DateTimeFormatter.ISO_LOCAL_DATE));
    System.out.println(time.format(DateTimeFormatter.ISO_LOCAL_TIME));
    System.out.println(dateTime.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME));

    2020-01-20
    11:12:34
    2020-01-20T11:12:34

This is a reasonable way for computers to communicate, but probably not how you want
to output the date and time in your program. Luckily there are some predefined formats
that are more useful:

    DateTimeFormatter shortDateTime = DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT);
    System.out.println(shortDateTime.format(dateTime)); // 1/20/20
    System.out.println(shortDateTime.format(date)); // 1/20/20
    System.out.println(shortDateTime.format(time)); // UnsupportedTemporalTypeException

The format() method is declared on both the formatter objects and the date/time objects, allowing you to reference
the objects in either order. The following statements print exactly the same thing as the previous code:

    DateTimeFormatter shortDateTime = DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT);
    System.out.println(dateTime.format(shortDateTime));
    System.out.println(date.format(shortDateTime));
    System.out.println(time.format(shortDateTime));


If you don’t want to use one of the predefined formats, you can create your own. For
example, this code spells out the month:

    DateTimeFormatter f = DateTimeFormatter.ofPattern("MMMM dd, yyyy, hh:mm");
    System.out.println(dateTime.format(f)); // January 20, 2020, 11:12

Before we look at the syntax, know you are not expected to memorize what different
numbers of each symbol mean. The most you will need to do is recognize the date and time
pieces.

MMMM M represents the month. The more Ms you have, the more verbose the Java output.
For example, M outputs 1, MM outputs 01, MMM outputs Jan, and MMMM outputs January.

dd d represents the date in the month. As with month, the more ds you have, the more
verbose the Java output. dd means to include the leading zero for a single-digit month.

, Use , if you want to output a comma (this also appears after the year).

yyyy y represents the year. yy outputs a two-digit year and yyyy outputs a four-digit year.

hh h represents the hour. Use hh to include the leading zero if you’re outputting a single-
digit hour.

: Use : if you want to output a colon.

mm m represents the minute.

### Formatting Dates in Java 7 and Earlier

| | Old way     | New way (Java 8 and later)  | 
|------| ----------- | ----------- | 
| Formatting the times | SimpleDateFormat sf = new SimpleDateFormat("hh:mm"); sf.format(jan3);  | DateTimeFormatter f = DateTimeFormatter.ofPattern(“hh:mm”); dt.format(f); | 

### Parsing Dates and Times

Now that you know how to convert a date or time to a formatted String, you’ll fi nd it easy
to convert a String to a date or time. Just like the format() method, the parse() method
takes a formatter as well. If you don’t specify one, it uses the default for that type.

    DateTimeFormatter f = DateTimeFormatter.ofPattern("MM dd yyyy");
    LocalDate date = LocalDate.parse("01 02 2015", f);
    LocalTime time = LocalTime.parse("11:22");
    System.out.println(date); // 2015-01-02
    System.out.println(time); // 11:22



















































