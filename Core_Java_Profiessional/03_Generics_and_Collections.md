## Generics and Collections

### Working with Generics

Why do we need generics? Well, remember when we said that we had to hope the caller
didn’t put something in the list that we didn’t expect? The following does just that:

    14: static void printNames(List list) {
    15: for (int i = 0; i < list.size(); i++) {
    16: String name = (String) list.get(i); // class cast exception here
    17: System.out.println(name);
    18: }
    19: }
    20: public static void main(String[] args) {
    21: List names = new ArrayList();
    22: names.add(new StringBuilder("Webby"));
    23: printNames(names);
    24: }

This code throws a ClassCastException. Line 22 adds a StringBuilder to list. This is
legal because a non-generic list can contain anything. However, line 16 is written to expect
a specific class to be in there. It casts to a String, reflecting this assumption. Since the
assumption is incorrect, the code throws a ClassCastException that java.lang
.StringBuilder cannot be cast to java.lang.String.


Generics fix this by allowing you to write and use parameterized types. You specify that
you want an ArrayList of String objects. Now the compiler has enough information to
prevent you from causing this problem in the first place:


    List<String> names = new ArrayList<String>();
    names.add(new StringBuilder("Webby")); // DOES NOT COMPILE


Getting a compiler error is good. You’ll know right away that something is wrong rather
than hoping to discover it later.

### Generic Classes

You can introduce generics into your own classes. The syntax for introducing a generic is to
declare a formal type parameter in angle brackets. For example, the following class named
Crate has a generic type variable declared after the name of the class:

    public class Crate<T> {
        private T contents;
        public T emptyCrate() {
            return contents;
        }
        public void packCrate(T contents) {
            this.contents = contents;
        }
    }

The generic type T is available anywhere within the Crate class. When you instantiate
the class, you tell the compiler what T should be for that particular instance.

    Elephant elephant = new Elephant();
    Crate<Elephant> crateForElephant = new Crate<>();
    crateForElephant.packCrate(elephant);
    Elephant inNewHome = crateForElephant.emptyCrate();

    Robot joeBot = new Robot();
    Crate<Robot> robotCrate = new Crate<>();
    robotCrate.packCrate(joeBot);
    Robot atDestination = robotCrate.emptyCrate();

### naming Conventions for Generics

A type parameter can be named anything you want. The convention is to use single
uppercase letters to make it obvious that they aren’t real class names. The following are
common letters to use:

■ E for an element

■ K for a map key

■ V for a map value

■ N for a number

■ T for a generic data type

■ S, U, V, and so forth for multiple generic types


Generic classes aren’t limited to having a single type parameter. This class shows two
generic parameters:

    public class SizeLimitedCrate<T, U> {
    private T contents;
    private U sizeLimit;
    public SizeLimitedCrate(T contents, U sizeLimit) {
    this.contents = contents;
    this.sizeLimit = sizeLimit;
    } }


    Elephant elephant = new Elephant();
    Integer numPounds = 15_000;
    SizeLimitedCrate<Elephant, Integer> c1 = new SizeLimitedCrate<>(elephant,numPounds);

Here we specify that the type is Elephant and the unit is Integer. We also throw in a
reminder that numeric literals have been able to contain underscores since Java 7.

### Type erasure

Specifying a generic type allows the compiler to enforce proper use of the generic type.
For example, specifying the generic type of Crate as Robot is like replacing the T in the
Crate class with Robot. However, this is just for compile time.

Behind the scenes, the compiler replaces all references to T in Crate with Object. In other
words, after the code compiles, your generics are actually just Object types. The Crate
class looks like the following:

    public class Crate {
    private Object contents;
    public Object emptyCrate() {
    return contents;
    }
    public void packCrate(Object contents) {
    this.contents = contents;
    }
    }

his means there is only one class file. There aren’t different copies for different
parameterized types. (Some other languages work that way.)
This process of removing the generics syntax from your code is referred to as type
erasure. Type erasure allows your code to be compatible with older versions of Java
that do not contain generics.

The compiler adds the relevant casts for your code to work with this type of erased
class. For example, you type

    Robot r = crate.emptyCrate();
    and the compiler turns it into
    Robot r = (Robot) crate.emptyCrate();

### Generic Interfaces

Just like a class, an interface can declare a formal type parameter. For example, the follow-
ing Shippable interface uses a generic type as the argument to its ship() method:

    public interface Shippable<T> {
    void ship(T t);
    }

There are three ways a class can approach implementing this interface. The first is to
specify the generic type in the class. The following concrete class says that it deals only
with robots. This lets it declare the ship() method with a Robot parameter:

    class ShippableRobotCrate implements Shippable<Robot> {
        public void ship(Robot t) { }
    }

The next way is to create a generic class. The following concrete class allows the caller
to specify the type of the generic:

    class ShippableAbstractCrate<U> implements Shippable<U> {
        public void ship(U t) { }
    }

The final way is to not use generics at all. This is the old way of writing code. It gener-
ates a compiler warning about Shippable being a raw type, but it does compile.

class ShippableCrate implements Shippable {
    public void ship(Object t) { }
}

### Generic Methods

Up until this point, you’ve seen formal type parameters declared on the class or interface
level. It is also possible to declare them on the method level. This is often useful for static
methods since they aren’t part of an instance that can declare the type. However, it is also
allowed on non-static methods as well

    public static <T> Crate<T> ship(T t) {
        System.out.println("Preparing " + t);
        return new Crate<T>();
    }

The method parameter is the generic type T. The return type is a Crate<T>. Before the
return type, we declare the formal type parameter of <T>.

Unless a method is obtaining the generic formal type parameter from the class/interface,
it is specified immediately before the return type of the method. This can lead to some
interesting-looking code!

    3: public static <T> void sink(T t) { }
    4: public static <T> T identity(T t) { return t; }
    5: public static T noGood(T t) { return t; } // DOES NOT COMPILE

You can call a generic method normally, and the compiler will figure out which one you
want. Alternatively, you can specify the type explicitly to make it obvious what the type is:

    Box.<String>ship("package");
    Box.<String[]>ship(args);

### Bounds

A bounded parameter type is a generic type that specifies a bound for the generic. Be
warned that this is the hardest section in the chapter, so don’t feel bad if you have to read it
more than once.

A wildcard generic type is an unknown generic type represented with a question mark
(?). You can use generic wildcards in three ways,


| Type of bound | Syntax   | Example | 
|------| ----------- | ----------- | 
| Unbounded wildcard  | ?   | List<?> l =new ArrayList<String>(); |
| Wildcard with an upper bound  | ? extends type   | List<? extends Exception> l = new ArrayList<RuntimeException>(); |
| Wildcard with a lower bound  | ? super type   | List<? super Exception> l = new ArrayList<Object>(); |


### Unbounded Wildcards

An unbounded wildcard represents any data type. You use ? when you want to specify that
any type is OK with you. Let’s suppose that we want to write a method that looks through
a list of any type:

    public static void printList(List<Object> list) {
        for (Object x: list) System.out.println(x);
    }

    public static void main(String[] args) {
        List<String> keywords = new ArrayList<>();
        keywords.add("java");
        printList(keywords); // DOES NOT COMPILE
    }

    Wait. What’s wrong? A String is a subclass of an Object. This is true. However,
    List<String> cannot be assigned to List<Object>.

    4: List<Integer> numbers = new ArrayList<>();
    5: numbers.add(new Integer(42));
    6: List<Object> objects = numbers; // DOES NOT COMPILE
    7: objects.add("forty two");
    8: System.out.println(numbers.get(1));

On line 4, the compiler promises us that only Integer objects will appear in numbers. If
line 6 were to have compiled, line 7 would break that promise by putting a String in there
since numbers and objects are references to the same object. Good thing that the compiler
prevents this

we cannot assign a List\<String\> to a List\<Object\>.
That’s fine; we don’t really need a List\<Object\>. What we really need is a List of “what-
ever.” That’s what List<?> is. The following code does what we expect:

    public static void printList(List<?> list) {
        for (Object x: list) System.out.println(x);
    }

    public static void main(String[] args) {
        List<String> keywords = new ArrayList<>();
        keywords.add("java");
        printList(keywords);
    }


printList() takes any type of list as a parameter. keywords is of type List\<String\>.
We have a match! List\<String\> is a list of anything. “Anything” just happens to be a
String here.

### Upper-Bounded Wildcards

Let’s try to write a method that adds up the total of a list of numbers. We’ve established
that a generic type can’t just use a subclass:

ArrayList<Number> list = new ArrayList<Integer>(); // DOES NOT COMPILE

Instead, we need to use a wildcard:

List<? extends Number> list = new ArrayList<Integer>();

The upper-bounded wildcard says that any class that extends Number or Number itself
can be used as the formal parameter type

    public static long total(List<? extends Number> list) {
        long count = 0;
        for (Number number: list)
            count += number.longValue();
        return count;
    }

Remember how we kept saying that type erasure makes Java think that a generic type
is an Object? That is still happening here. Java converts the previous code to something
equivalent to the following

    public static long total(List list) {
        long count = 0;
        for (Object obj: list) {
            Number number = (Number) obj;
            count += number.longValue();
        }
        return count;
    }

Something interesting happens when we work with upper bounds or unbounded wild-
cards. The list becomes logically immutable

    2: static class Sparrow extends Bird { }
    3: static class Bird { }
    4:
    5: public static void main(String[] args) {
    6: List<? extends Bird> birds = new ArrayList<Bird>();
    7: birds.add(new Sparrow()); // DOES NOT COMPILE
    8: birds.add(new Bird()); // DOES NOT COMPILE
    9: }

The problem stems from the fact that Java doesn’t know what type List\<? extends
Bird\> really is. It could be List\<Bird\> or List\<Sparrow\> or some other generic type that
hasn’t even been written yet

Now let’s try an example with an interface. We have an interface and two classes that
implement it:

    interface Flyer { void fly(); }
    class HangGlider implements Flyer { public void fly() {} }
    class Goose implements Flyer { public void fly() {} }

We also have two methods that use it. One just lists the interface and the other uses an
upper bound:

private void anyFlyer(List<Flyer> flyer) {}
private void groupOfFlyers(List<? extends Flyer> flyer) {}

Note that we used the keyword extends rather than implements. Upper bounds are like
anonymous classes in that they use extends regardless of whether we are working with a
class or an interface.

### Lower-Bounded Wildcards

Let’s try to write a method that adds a string “quack” to two lists:

    List<String> strings = new ArrayList<String>();
    strings.add("tweet");

    List<Object> objects = new ArrayList<Object>(strings);

    addSound(strings);
    addSound(objects);

The problem is that we want to pass a List\<String\> and a List\<Object\> to the same
method

To solve this problem, we need to use a lower bound:

    public static void addSound(List<? super String> list) { // lower bound
        list.add("quack");
    }

With a lower bound, we are telling Java that the list will be a list of String objects or a
list of some objects that are a superclass of String. Either way, it is safe to add a String to
that list.

Just like generic classes, you probably won’t use this in your code unless you are writing
code for others to reuse. Even then it would be rare. But it’s on the exam, so now is the time
to learn it!

### understand Generic supertypes

When you have subclasses and superclasses, lower bounds can get tricky:

    3: List<? super IOException> exceptions = new ArrayList<Exception>();
    4: exceptions.add(new Exception()); // DOES NOT COMPILE
    5: exceptions.add(new IOException());
    6: exceptions.add(new FileNotFoundException());

Line 3 references a List that could be List\<IOException\> or List\<Exception\> or
List\<Object\>. Line 4 does not compile because we could have a List\<IOException\> and
an Exception object wouldn’t fit in there.

Line 4 is fine. IOException can be added to any of those types. Line 5 is also fine. File-
NotFoundException can also be added to any of those three types. This is tricky because
FileNotFoundException is a subclass of IOException and the keyword says super. What
happens is that Java says “Well, FileNotFoundException also happens to be an IOEx-
ception, so everything is fine.”

## Using Lists, Sets, Maps, and Queues

A collection is a group of objects contained in a single object. The Java Collections
Framework is a set of classes in java.util for storing collections. There are four main
interfaces in the Java Collections Framework:

■ List: A list is an ordered collection of elements that allows duplicate entries. Elements
in a list can be accessed by an int index.

■ Set: A set is a collection that does not allow duplicate entries.

■ Queue: A queue is a collection that orders its elements in a specific order for processing.
A typical queue processes its elements in a first-in, first-out order, but other orderings
are possible.

■ Map: A map is a collection that maps keys to values, with no duplicate keys allowed.
The elements in a map are key/value pairs.

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-27_14-34-00.png)

### Common Collections Methods

The Collection interface contains useful methods for working with lists, sets, and queues. We
will also cover maps

## add()

The add() method inserts a new element into the Collection and returns whether it was
successful. The method signature is

    boolean add(E element)

Remember that the Collections Framework uses generics. You will see E appear
frequently. It means the generic type that was used to create the collection. For some collec-
tion types, add() always returns true. For other types, there is logic as to whether the add
was successful. The following shows how to use this method:

    3: List<String> list = new ArrayList<>();
    4: System.out.println(list.add("Sparrow")); // true
    5: System.out.println(list.add("Sparrow")); // true
    6:
    7: Set<String> set = new HashSet<>();
    8: System.out.println(set.add("Sparrow")); // true
    9: System.out.println(set.add("Sparrow")); // false


A List allows duplicates, making the return value true each time. A Set does not allow
duplicates. On line 9, we tried to add a duplicate so that Java returns false from the add()
method.

### remove()

The remove() method removes a single matching value in the Collection and returns
whether it was successful. The method signature is

boolean remove(Object object)

This time, the boolean return value tells us whether a match was removed. The follow-
ing shows how to use this method:

    3: List<String> birds = new ArrayList<>();
    4: birds.add("hawk"); // [hawk]
    5: birds.add("hawk"); // [hawk, hawk]
    6: System.out.println(birds.remove("cardinal")); // prints false
    7: System.out.println(birds.remove("hawk")); // prints true
    8: System.out.println(birds); // [hawk]

## isEmpty() and size()

The isEmpty() and size() methods look at how many elements are in the Collection.
The method signatures are

    boolean isEmpty()
    int size()

The following shows how to use these methods:

    System.out.println(birds.isEmpty()); // true
    System.out.println(birds.size()); // 0

### clear()

The clear() method provides an easy way to discard all elements of the Collection. The
method signature is

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

### contains()

The contains() method checks if a certain value is in the Collection. The method
signature is

    boolean contains(Object object)

The following shows how to use this method:
    List<String> birds = new ArrayList<>();
    birds.add("hawk"); // [hawk]
    System.out.println(birds.contains("hawk")); // true
    System.out.println(birds.contains("robin")); // false

This method calls equals() on each element of the ArrayList to see if there are any matches.

### Using the List Interface

You use a list when you want an ordered collection that can contain duplicate entries. Items
can be retrieved and inserted at specific positions in the list based on an int index much
like an array

### Comparing List Implementations

An ArrayList is like a resizable array. When elements are added, the ArrayList auto-
matically grows. When you aren’t sure which collection to use, use an ArrayList.

The main benefit of an ArrayList is that you can look up any element in constant time.
Adding or removing an element is slower than accessing an element. This makes an
ArrayList a good choice when you are reading more often than (or the same amount
as) writing to the ArrayList.

A LinkedList is special because it implements both List and Queue. It has all of the
methods of a List. It also has additional methods to facilitate adding or removing from the
beginning and/or end of the list.

The main benefits of a LinkedList are that you can access, add, and remove from the
beginning and end of the list in constant time. The tradeoff is that dealing with an arbi-
trary index takes linear time. This makes a LinkedList a good choice when you’ll be using
it as Queue.

### Working with List Methods

The methods in the List interface are for working with indexes. In addition to the
inherited Collection methods



| Type of bound | Syntax   | 
|------| ----------- | 
| void add(E element) |  Adds element to end  | 
| void add(int index, E element) | Adds element at index and moves the rest toward the end   | 
| E get(int index)  |  Returns element at index | 
| int indexOf(Object o) | Returns first matching index or -1 if not found   | 
| int lastIndexOf(Object o) |  Returns last matching index or -1 if not found  | 
| void remove(int index)  |   Removes element at index and moves the rest toward the front | 
| E set(int index, E e) | Replaces element at index and returns original|

The following statements demonstrate these basic methods for adding and removing
items from a list:

    4: List<String> list = new ArrayList<>();
    5: list.add("SD"); // [SD]
    6: list.add(0, "NY"); // [NY,SD]
    7: list.set(1, "FL"); // [NY,FL]
    8: list.remove("NY"); // [FL]
    9: list.remove(0); // []

    5: list.add("OH"); // [OH]
    6: list.add("CO"); // [OH,CO]
    7: list.add("NJ"); // [OH,CO,NJ]
    8: String state = list.get(0); // OH
    9: int index = list.indexOf("NJ"); // 2

### looping through a list

    for (String string: list) {
        System.out.println(string);
    }

    Iterator<String> iter = list.iterator();
    while(iter.hasNext()) {
        String string = iter.next();
        System.out.println(string);
    }

### Using the Set Interface

You use a set when you don’t want to allow duplicate entries

### Comparing Set Implementations

A HashSet stores its elements in a hash table. This means that it uses the hashCode()
method of the objects to retrieve them more efficiently

The main benefit is that adding elements and checking if an element is in the set both
have constant time. The tradeoff is that you lose the order in which you inserted the
elements

A TreeSet stores its elements in a sorted tree structure
The main benefit is that the set is always in sorted order
TreeSet implements a special interface called NavigableSet

### Working with Set Methods

The Set interface doesn’t add any extra methods. You just have to know how sets behave with respect to the traditional Collection methods. You also have to know the differences between the types of sets. Let’s start with HashSet:

    3: Set<Integer> set = new HashSet<>();
    4: boolean b1 = set.add(66); // true
    5: boolean b2 = set.add(10); // true
    6: boolean b3 = set.add(66); // false
    7: boolean b4 = set.add(8); // true
    8: for (Integer integer: set) System.out.print(integer + ","); // 66,8,10,

Remember that the equals() method is used to determine equality. The hashCode() method
is used to know which bucket to look in so that Java doesn’t have to look through the whole
set to find out if an object is there. The best case is that hash codes are unique, and Java has to
call equals() on only one object. The worst case is that all implementations return the same
hashCode(), and Java has to call equals() on every element of the set anyway.

Now let’s look at the same example with TreeSet:

    3: Set<Integer> set = new TreeSet<>();
    4: boolean b1 = set.add(66); // true
    5: boolean b2 = set.add(10); // true
    6: boolean b3 = set.add(66); // false
    7: boolean b4 = set.add(8); // true
    8: for (Integer integer: set) System.out.print(integer + ","); // 8,10,66
    
This time, the elements are printed out in their natural sorted order. Numbers imple-
ment the Comparable interface in Java, which is used for sorting.

### The NavigableSet interface

TreeSet implements the NavigableSet interface. This interface provides some interest-
ing methods. Their method signatures are as follows:

| Method | Description   | 
|------| ----------- | 
| E lower(E e) |  Returns greatest element that is < e, or null if no such element  | 
| E floor(E e) |  Returns greatest element that is <= e, or null if no such element  | 
| E ceiling(E e) | Returns smallest element that is >= e, or null if no such element  | 
| E higher(E e) | Returns smallest element that is > e, or null if no such element  | 

    36: NavigableSet<Integer> set = new TreeSet<>();
    37: for (int i = 1; i <= 20; i++) set.add(i);
    38: System.out.println(set.lower(10)); // 9
    39: System.out.println(set.floor(10)); // 10
    40: System.out.println(set.ceiling(20)); // 20
    41: System.out.println(set.higher(20)); // null

### Using the Queue Interface

You use a queue when elements are added and removed in a specific order. Queues are typi-
cally used for sorting elements prior to processing them

Unless stated otherwise, a queue is assumed to be FIFO (first-in, first-out)

### Comparing Queue Implementations

You saw LinkedList earlier in the List section. In addition to being a list, it is a double-
ended queue. A double-ended queue is different from a regular queue in that you can insert
and remove elements from both the front and back of the queue

An ArrayDeque is a “pure” double-ended queue. It was introduced in Java 6, and it
stores its elements in a resizable array. The main benefit of an ArrayDeque is that it is more
efficient than a LinkedList.

### Working with Queue Methods

| Method | Description   | For Queue | For Stack |
|------| ----------- | ------| ----------- | 
| boolean add(E e) | Adds an element to the back of the queue   and returns true or throws an exception  | Yes | No | 
| E element() | Returns next element or throws an exception if empty queue| Yes | No |
| boolean offer(E e) | Adds an element to the back of the queue and returns whether successful| Yes | No|
| E remove()| Removes and returns next element or throws an exception if empty queue| Yes | No|
| void push(E e)|Adds an element to the front of the queue | Yes | Yes |
| E poll() | Removes and returns next element or returns null if empty queue| Yes | No |
| E peek() | Returns next element or returns null if empty queue | Yes | Yes |
| E pop() |Removes and returns next element or throws an exception if empty queue| No | Yes|


As you can see, there are basically two sets of methods. One set throws an exception
when something goes wrong. The other uses a different return value when something goes
wrong. The offer/poll/peek methods are more common. This is the standard language
people use when working with queues.

    12: Queue<Integer> queue = new ArrayDeque<>();
    13: System.out.println(queue.offer(10)); // true
    14: System.out.println(queue.offer(4)); // true
    15: System.out.println(queue.peek()); // 10
    16: System.out.println(queue.poll()); // 10
    17: System.out.println(queue.poll()); // 4
    18: System.out.println(queue.peek()); // null

### Map

You use a map when you want to identify values by a key. For example, when you use the
contact list in your phone, you look up “George” rather than looking through each phone
number in turn.

    Mary -> 777-777-7777
    George -> 555-555-5555

You don’t need to know the names of the
specific interfaces that the different maps implement, but you do need to know that TreeMap
is sorted and navigable.

### Comparing Map Implementations

A HashMap stores the keys in a hash table. This means that it uses the hashCode() method
of the keys to retrieve their values more efficiently.

The main benefit is that adding elements and retrieving the element by key both have
constant time. 

The tradeoff is that you lose the order in which you inserted the elements.
Most of the time, you aren’t concerned with this in a map anyway. If you were, you could
use LinkedHashMap.

A TreeMap stores the keys in a sorted tree structure. The main benefit is that the keys are
always in sorted order. The tradeoff is that adding and checking if a key is present are both
are slow compared to HashMap

### Working with Map Methods

Given that Map doesn’t extend Collection, there are more methods specified on the Map
interface. Since there are both keys and values, we need generic type parameters for both.
The class uses K for key and V for value.

| Method | Description   | 
|------| ----------- |
| void clear()  | Removes all keys and values from the map.  |
| boolean isEmpty() | Returns whether the map is empty. |
| int size()  | Returns the number of entries (key/value pairs) in the map. |
| V get(Object key) | Returns the value mapped by key or null if none is mapped.|
| V put(K key, V value) | Adds or replaces key/value pair. Returns previous value or null.|
| V remove(Object key) | Removes and returns value mapped to key. Returns null if none. |
| boolean containsKey(Object key)| Returns whether key is in map. |
| boolean containsValue(Object) | Returns value is in map. |
| Set<K> keySet() | Returns set of all keys. |
| Collection<V> values() | Returns Collection of all values |

As usual, let’s compare running the same code with two Map types. First up is HashMap:

    Map<String, String> map = new HashMap<>();
    map.put("koala", "bamboo");
    map.put("lion", "meat");
    map.put("giraffe", "leaf");
    String food = map.get("koala"); // bamboo
    for (String key: map.keySet())
    System.
    out.print(key + ","); // koala,giraffe,lion,

Java uses the hashCode() of the key to determine the order. The order here happens to
not be sorted order, or the order in which we typed the values. Now let’s look at TreeMap:

    Map<String, String> map = new TreeMap<>();
    map.put("koala", "bamboo");
    map.put("lion", "meat");
    map.put("giraffe", "leaf");
    String food = map.get("koala"); // bamboo
    for (String key: map.keySet())
    System.out.print(key + ","); // giraffe,koala,lion,

TreeMap sorts the keys as we would expect. If we were to have called values() instead
of keySet(), the order of the values would correspond to the order of the keys.
With our same map, we can try some boolean checks:

    System.out.println(map.contains("lion")); // DOES NOT COMPILE non existing method
    System.out.println(map.containsKey("lion")); // true
    System.out.println(map.containsValue("lion")); // false
    System.out.println(map.size()); // 3


### Comparing Collection Types

| Type | Can contain duplicate elements?   | Elements ordered?   | Has keys and values?   | Must add/remove in specific order?   | 
|------| ------   | ------   | ------   | ------   | 
| List | Yes |  Yes (by index)   |  No |  No | 
| Map | Yes (for values)  | No   | Yes  |  No | 
| Queue | Yes | Yes (retrieved in defined order)   |  No | Yes  | 
| Set | No |   No |  No | No  | 


| Type | Java Collections Framework interface   | Sorted?   | Calls hashCode?   | Calls compareTo?   | 
|------| ------   | ------   | ------   | ------   | 
|ArrayList|List|No|No|No|
|ArrayDeque|Queue|No|No|No|
|HashMap|Map|No|Yes|No|
|HashSet|Set|No|Yes|No|
|Hashtable|Map|No|Yes|No|
|LinkedList|List, Queue|No|No|No|
|Stack|List|No|No|No|
|TreeMap|Map|Yes|No|Yes|
|TreeSet|Set|Yes|No|Yes|
|Vector|List|No|No|No|


The data structures that involve sorting do not allow nulls. This makes sense. We can’t
compare a null and a String

### Comparator vs. Comparable

We discussed “order” for the TreeSet and TreeMap classes. For numbers, order is
obvious—it is numerical order. For String objects, order is defi ned according to the
Unicode character mapping.

Numbers sort before letters and uppercase letters sort before lowercase letters.

You can also sort objects that you create. Java provides an interface called Comparable.
If your class implements Comparable, it can be used in these data structures that require
comparison. There is also a class called Comparator, which is used to specify that you want
to use a different order than the object itself provides.

### Comparable

The Comparable interface has only one method. In fact, this is the entire interface:

    public interface Comparable<T> {
        public int compareTo(T o);
    }

See the use of generics in there? This lets you avoid the cast when implementing compa-
reTo(). Any object can be Comparable. For example, we have a bunch of ducks and want to
sort them by name:

    import java.util.*;
    public class Duck implements Comparable<Duck> {
        private String name;
        public Duck(String name) {
            this.name = name;
        }

        public String toString() { // use readable output
            return name;
        }

        public int compareTo(Duck d) {
            return name.compareTo(d.name); // call String's compareTo
        }

        public static void main(String[] args) {
            List<Duck> ducks = new ArrayList<>();
            ducks.add(new Duck("Quack"));
            ducks.add(new Duck("Puddles"));
            Collections.sort(ducks); // sort by name
            System.out.println(ducks); // [Puddles, Quack]
        } 
    }

The Duck class implements the Comparable interface. Without implementing that interface, all we have is a method named compareTo(), but it wouldn’t be a Comparable object.

We still need to know what the compareTo() method returns so that we can write our
own. There are three rules to know:

■ The number zero is returned when the current object is equal to the argument to com-
pareTo().

■ A number less than zero is returned when the current object is smaller than the argu-
ment to compareTo().

■ A number greater than zero is returned when the current object is larger than the argu-
ment to compareTo().

    1: public class Animal implements java.util.Comparable<Animal> {
    2: private int id;
    3: public int compareTo(Animal a) {
    4: return id – a.id;
    5: }
    6: public static void main(String[] args) {
    7: Animal a1 = new Animal();
    8: Animal a2 = new Animal();
    9: a1.id = 5;
    10: a2.id = 7;
    11: System.out.println(a1.compareTo(a2)); // -2
    12: System.out.println(a1.compareTo(a1)); // 0
    13: System.out.println(a2.compareTo(a1)); // 2
    14: } }

### compareTo() and equals() Consistency

If you write a class that implements Comparable, you introduce new business logic for
determining equality. The compareTo() method returns 0 if two objects are equal, while
your equals() method returns true if two objects are equal. A natural ordering that
uses compareTo() is said to be consistent with equals if, and only if, x.equals(y) is true
whenever x.compareTo(y) equals 0. You are strongly encouraged to make your Compara-
ble classes consistent with equals because not all collection classes behave predictably if
the compareTo() and equals() methods are not consistent.

For example, the following Product class defines a compareTo() method that is not con-
sistent with equals:

    public class Product implements Comparable<Product> {
        int id;
        String name;
        public boolean equals(Object obj) {
        if(!(obj instanceof Product)) {
            return false;
        }
        Product other = (Product) obj;
            return this.id == other.id;
        }
        public int compareTo(Product obj) {
            return this.name.compareTo(obj.name);
        } 
    }

You might be sorting Product objects by name, but names are not unique. Therefore, the
return value of compareTo() might not be 0 when comparing two equal Product objects,
so this compareTo() method is not consistent with equals. One way to fix that is to use a
Comparator to define the sort elsewhere.

### Comparator

Sometimes you want to sort an object that did not implement Comparable, or you want to
sort objects in different ways at different times.

Suppose that we add weight to our Duck class. We now have the following:

    public class Duck implements Comparable<Duck> {
        private String name;
        private int weight;
        public Duck(String name, int weight) {
            this.name = name;
            this.weight = weight;
        }
        public String getName() { return name; }
        public int getWeight() { return weight; }
        public String toString() { return name; }
        public int compareTo(Duck d) {
            return name.compareTo(d.name);
        }
    }

The Duck class itself can define compareTo() in only one way. In this case, name was
chosen. If we want to sort by something else, we have to define that sort order outside the
compareTo() method:


    public static void main(String[] args) {
        Comparator<Duck> byWeight = new Comparator<Duck>() {
            public int compare(Duck d1, Duck d2) {
            return d1.getWeight()—d2.getWeight();
            }
        };        
        List<Duck> ducks = new ArrayList<>();
        ducks.add(new Duck("Quack", 7));
        ducks.add(new Duck("Puddles", 10));
        Collections.sort(ducks);
        System.out.println(ducks); // [Puddles, Quack]
        Collections.sort(ducks, byWeight);
        System.out.println(ducks); // [Quack, Puddles]
    }


First, we defined an inner class with the comparator. Then we sorted without the com-
parator and with the comparator to see the difference in output.

Comparator is a functional interface since there is only one abstract method to imple-
ment. This means that we can rewrite the comparator in the previous example as any of the
following:

    Comparator<Duck> byWeight = (d1, d2) -> d1.getWeight()—d2.getWeight();
    Comparator<Duck> byWeight = (Duck d1, Duck d2) -> d1.getWeight()—d2.getWeight();
    Comparator<Duck> byWeight = (d1, d2) -> { return d1.getWeight()—d2.getWeight(); };
    Comparator<Duck> byWeight = (Duck d1, Duck d2) -> {return d1.getWeight()—d2.getWeight(); };

| Difference | Comparable   | Comparator   | 
|------| ----------- |----------- |
| Package name  | java.lang  | java.util |
| Interface must be implemented by class comparing?  | Yes | No |
| Method name in interface  | compareTo | compare |
| Number of parameters  | 1 | 2 |
| Common to declare using a lambda  | No | Yes |

### Comparing multiple fields

    public class Squirrel {
        private int weight;
        private String species;
            public Squirrel(String theSpecies) {
            if (theSpecies == null) throw new IllegalArgumentException();
            species = theSpecies;
            }
        public int getWeight() { return weight; }
        public void setWeight(int weight) { this.weight = weight; }
        public String getSpecies() { return species; }
    }

We want to write a Comparator to sort by species name. If two squirrels are from the spe-
cies, we want to sort the one that weighs the least first. We could do this with code that
looks like this:

    public class MultiFieldComparator implements Comparator<Squirrel> {
        public int compare(Squirrel s1, Squirrel s2) {
        int result = s1.getSpecies().compareTo(s2.getSpecies());
        if (result != 0) return result;
        return s1.getWeight()—s2.getWeight();
    }}

    public class ChainingComparator implements Comparator<Squirrel> {
        public int compare(Squirrel s1, Squirrel s2) {
        Comparator<Squirrel> c = Comparator.comparing(s -> s.getSpecies());
        c = c.thenComparingInt(s -> s.getWeight());
        return c.compare(s1, s2);
    }}

### Searching and Sorting

You already know the basics of searching and sorting. You now know a little more about
Comparable and Comparator.

The sort method uses the compareTo() method to sort. It expects the objects to be sorted
to be Comparable.

    1: import java.util.*;
    2: public class SortRabbits {
    3: static class Rabbit{ int id; }
    4: public static void main(String[] args) {
    5: List<Rabbit> rabbits = new ArrayList<>();
    6: rabbits.add(new Rabbit());
    7: Collections.sort(rabbits); // DOES NOT COMPILE
    8: } }

Java knows that the Rabbit class is not Comparable. It knows sorting will fail, so it
doesn’t even let the code compile. You can fix this by passing a Comparator to sort().
Remember that a Comparator is useful when you want to specify sort order without using a
compareTo() method:

    import java.util.*;
        public class SortRabbits {
        static class Rabbit{ int id; }
        public static void main(String[] args) {
        List<Rabbit> rabbits = new ArrayList<>();
        rabbits.add(new Rabbit());
        Comparator<Rabbit> c = (r1, r2) -> r1.id - r2.id;
        Collections.sort(rabbits, c);
    } }

sort() and binarySearch() allow you to pass in a Comparator object when you don’t want
to use the natural order. There is a trick in this space. What do you think the following outputs?

    3: List<String> names = Arrays.asList("Fluffy", "Hoppy");
    4: Comparator<String> c = Comparator.reverseOrder();
    5: int index = Collections.binarySearch(names, "Hoppy", c);
    6: System.out.println(index);

The correct answer is -1
This list happens to be sorted in ascending order. Line 4 creates a Comparator that
reverses the natural order. Line 5 requests a binary search in descending order. Since the list
is in ascending order, we don’t meet the precondition for doing a search

### Additions in Java 8

### Using Method References

Method references are a way to make the code shorter by reducing some of the code that
can be inferred and simply mentioning the name of the method. Like lambdas, it takes time
to get used to the new syntax.

Suppose that we have a Duck class with name and weight attributes along with this
helper class:

    public class DuckHelper {
        public static int compareByWeight(Duck d1, Duck d2) {
            return d1.getWeight()—d2.getWeight();
        }
        public static int compareByName(Duck d1, Duck d2) {
            return d1.getName().compareTo(d2.getName());
        }
    }

Now think about how we would write a Comparator for it if we wanted to sort by
weight. Using lambdas, we’d have the following:

    Comparator<Duck> byWeight = (d1, d2) -> DuckHelper.compareByWeight(d1, d2);

Not bad. There’s a bit of redundancy, though. The lambda takes two parameters and
does nothing but pass those parameters to another method. Java 8 lets us remove that
redundancy and simply write this:

    Comparator<Duck> byWeight = DuckHelper::compareByWeight;

The :: operator tells Java to pass the parameters automatically into compareByWeight.

There are four formats for method references:

■ Static methods

■ Instance methods on a particular instance

■ Instance methods on an instance to be determined at runtime

■ Constructors


Let’s look at some examples from the Java API. In each set, we show the lambda equiva-
lent. Remember that none of these method references are actually called in the code that
follows. They are simply available to be called in the future. Let’s start with a static method:

    14: Consumer<List<Integer>> methodRef1 = Collections::sort;
    15: Consumer<List<Integer>> lambda1 = l -> Collections.sort(l);

    16: String str = "abc";
    17: Predicate<String> methodRef2 = str::startsWith;
    18: Predicate<String> lambda2 = s -> str.startsWith(s)

    19: Predicate<String> methodRef3 = String::isEmpty;
    20: Predicate<String> lambda3 = s -> s.isEmpty();

    21: Supplier<ArrayList> methodRef4 = ArrayList::new;
    22: Supplier<ArrayList> lambda4 = () -> new ArrayList();

### Removing Conditionally

Java 8 introduces a new method called removeIf. Before this, we had the ability to remove
a specified object from a collection or a specified index from a list. Now we can specify
what should be deleted using a block of code.

The method signature looks like this:

    boolean removeIf(Predicate<? super E> filter)

It uses a Predicate, which is a lambda that takes one parameter and returns a boolean.
Since lambdas use deferred execution, this allows specifying logic to run when that point in
the code is reached. Let’s take a look at an example.

    4: List<String> list = new ArrayList<>();
    5: list.add("Magician");
    6: list.add("Assistant");
    7: System.out.println(list); // [Magician, Assistant]
    8: list.removeIf(s -> s.startsWith("A"));
    9: System.out.println(list); // [Magician]

### Updating All Elements

Another new method introduced on Lists is replaceAll. Java 8 lets you pass a lambda
expression and have it applied to each element in the list. The result replaces the current
value of that element.

    void replaceAll(UnaryOperator<E> o)

It uses a UnaryOperator, which takes one parameter and returns a value of the same
type. 

Let’s take a look at an example:

    List<Integer> list = Arrays.asList(1, 2, 3);
    list.replaceAll(x -> x*2);
    System.out.println(list); // [2, 4, 6]

The lambda uses deferred execution to increase the value of each element in the list.

### Looping through a Collection

Looping though a Collection is very common. For example, we often want to print out the
values one per line. There are a few ways to do this. We could use an iterator, the enhanced
for loop, or a number of other approaches—or we could use a Java 8 lambda.

Cats like to explore, so let’s join two of them as we learn a shorter way to loop through

a Collection. We start with the traditional way:

    List<String> cats = Arrays.asList("Annie", "Ripley");
    for(String cat: cats)
        System.out.println(cat);

This works. We can do the same thing with lambdas in one line:
    cats.forEach(c -> System.out.println(c));

This time, we’ve used a Consumer, which takes a single parameter and doesn’t return
anything. You won’t see this approach used too often because it is common to use a method

    reference instead:
    cats.forEach(System.out::println);

### Using New Java 8 Map APIs

Java 8 added a number of new methods on the Map interface. Only merge() , computeIfPresent() and computeIfAbsent()

Sometimes you need to update the value for a specific key in the map. There are a few
ways that you can do this. The first is to replace the existing value unconditionally:

    Map<String, String> favorites = new HashMap<>();
    favorites.put("Jenny", "Bus Tour");
    favorites.put("Jenny", "Tram");
    System.out.println(favorites); // {Jenny=Tram}

There’s another method, called putIfAbsent(), that you can call if you want to set a
value in the map, but this method skips it if the value is already set to a non-null value:

    Map<String, String> favorites = new HashMap<>();
    favorites.put("Jenny", "Bus Tour");
    favorites.put("Tom", null);
    favorites.putIfAbsent("Jenny", "Tram");
    favorites.putIfAbsent("Sam", "Tram");
    favorites.putIfAbsent("Tom", "Tram");
    System.out.println(favorites); // {Tom=Tram, Jenny=Bus Tour, Sam=Tram}

As you can see, Jenny’s value is not updated because one was already present. Sam
wasn’t there at all, so he was added. Tom was present as a key but had a null value.
Therefore, he was added as well. These two methods handle simple replacements.


### merge

The merge() method allows adding logic to the problem of what to choose. Suppose that
our guests are indecisive and can’t pick a favorite. They agree that the ride that has the lon-
gest name must be the most fun. We can write code to express this by passing a mapping
function to the merge() method:

    11: BiFunction<String, String, String> mapper = (v1, v2)
    12: -> v1.length() > v2.length() ? v1: v2;
    13:
    14: Map<String, String> favorites = new HashMap<>();
    15: favorites.put("Jenny", "Bus Tour");
    16: favorites.put("Tom", "Tram");
    17:
    18: String jenny = favorites.merge("Jenny", "Skyride", mapper);
    19: String tom = favorites.merge("Tom", "Skyride", mapper);
    20:
    21: System.out.println(favorites); // {Tom=Skyride, Jenny=Bus Tour}
    22: System.out.println(jenny); // Bus Tour
    23: System.out.println(tom); // Skyride

Line 11 uses a functional interface called a BiFunction. In this case, it takes two param-
eters of the same type and returns a value of that type. Our implementation returns the one
with the longest name. Line 18 calls this mapping function, and it sees that “Bus Tour”
is longer than “Skyride,” so it leaves the value as “Bus Tour.” Line 19 calls this mapping
function again. This time “Tram” is not longer than “Skyride,” so the map is updated. Line
21 prints out the new map contents. Lines 22 and 23 show that the result gets returned
from merge().

The merge() method also has logic for what happens if nulls or missing keys are
involved. In this case, it doesn’t call the BiFunction at all, and it simply uses the new
value.

    BiFunction<String, String, String> mapper = (v1, v2) -> v1.length() >
    v2.length() ? v1 : v2;
    Map<String, String> favorites = new HashMap<>();
    favorites.put("Sam", null);
    favorites.merge("Tom", "Skyride", mapper);
    favorites.merge("Sam", "Skyride", mapper);
    System.out.println(favorites); // {Tom=Skyride, Sam=Skyride}

Notice that the mapping function isn’t called. If it were, we’d have a
NullPointerException. The mapping function is used only when there are two actual val-
ues to decide between.

The final thing to know about merge() is what happens when the mapping function is
called and returns null. The key is removed from the map when this happens:

    BiFunction<String, String, String> mapper = (v1, v2) -> null;
    Map<String, String> favorites = new HashMap<>();
    favorites.put("Jenny", "Bus Tour");
    favorites.put("Tom", "Bus Tour");
    favorites.merge("Jenny", "Skyride", mapper);
    favorites.merge("Sam", "Skyride", mapper);
    System.out.println(favorites); // {Tom=Bus Tour, Sam=Skyride

Tom was left alone since there was no merge() call for that key. Sam was added since
that key was not in the original list. Jenny was removed because the mapping function
returned null. You’ll see merge again in the next chapter

### computeIfPresent and computeIfAbsent

computeIfPresent() calls the BiFunction if the requested key is found.

    Map<String, Integer> counts = new HashMap<>();
    counts.put("Jenny", 1);
    BiFunction<String, Integer, Integer> mapper = (k, v) -> v + 1;
    Integer jenny = counts.computeIfPresent("Jenny", mapper);
    Integer sam = counts.computeIfPresent("Sam", mapper);
    System.out.println(counts); // {Jenny=2}
    System.out.println(jenny); // 2
    System.out.println(sam); // null

The function interface is a BiFunction again. However, this time the key and value are
passed rather than two values. Just like with merge(), the return value is the result of what
changed in the map or null if that doesn’t apply.

For computeIfAbsent(), the functional interface runs only when the key isn’t present or
is null:

    Map<String, Integer> counts = new HashMap<>();
    counts.put("Jenny", 15);
    counts.put("Tom", null);
    Function<String, Integer> mapper = (k) -> 1;
    Integer jenny = counts.computeIfAbsent("Jenny", mapper); // 15
    Integer sam = counts.computeIfAbsent("Sam", mapper); // 1
    Integer tom = counts.computeIfAbsent("Tom", mapper); // 1
    System.out.println(counts); // {Tom=1, Jenny=15, Sam=1}

Since there is no value already in the map, a Function is used instead of a
BiFunction. Only the key is passed as input. As you can see, Jenny isn’t changed
because that key is already in the map. Tom is updated because null is treated like not
being there.



















































