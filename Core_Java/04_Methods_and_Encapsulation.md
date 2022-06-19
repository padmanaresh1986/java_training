## Designing Methods

Every interesting Java program we’ve seen has had a main() method. 
We can write other methods, too. For example, we can write a basic method to take a nap.

![Alt text](https://github.com/padmanaresh1986/java_training/blob/main/Core_Java/images/2022-06-19_18-19-11.png)

This is called a method declaration, which specifies all the information needed to call
the method. There are a lot of parts, and we’ll cover each one in more detail

| Element | Value in nap() example   | Required? | 
|------| ----------- | ----------- | 
| Access modifier | public  | No |
| Optional specifier | final  | No   |
| Return type  |  void |  Yes  |
| Method name | nap  |  Yes  |
| Parameter list | (int minutes)  |  Yes, but can be empty parentheses  |
| Optional exception list | throws InterruptedException  | No   |
| Method body | { // take a nap }  | Yes, but can be empty braces   |

### Access Modifiers

Java offers four choices of access modifier:

**public** The method can be called from any class.

**private** The method can only be called from within the same class.

**protected** The method can only be called from classes in the same package or subclasses.

**Default (Package Private)** Access The method can only be called from classes in the same
package. This one is tricky because there is no keyword for default access. You simply omit
the access modifier.

    public void walk1() {}
    default void walk2() {} // DOES NOT COMPILE
    void public walk3() {} // DOES NOT COMPILE
    void walk4() {}

### Optional Specifiers

There are a number of optional specifier
you can have multiple specifiers in the same method (although not all combinations are legal). When this happens,
you can specify them in any order. And since it is optional, you can’t have any of them at
all. This means you can have zero or more specifiers in a method declaration.

**static** Covered later, Used for class methods.
**abstract** Covered later, Used when not providing a method body.
**final** Covered later, Used when a method is not allowed to be overridden by a subclass.
**synchronized**
**native** 
**strictfp**

    public void walk1() {}
    public final void walk2() {}
    public static final void walk3() {}
    public final static void walk4() {}
    public modifier void walk5() {} // DOES NOT COMPILE
    public void final walk6() {} // DOES NOT COMPILE
    final public void walk7() {}

### Return Type

The next item in a method declaration is the return type. The return type might be an
actual Java type such as String or int. If there is no return type, the void keyword is used.
This special return type comes from the English language: void means without contents. In
Java, we have no type there.

When checking return types, you also have to look inside the method body. Methods
with a return type other than void are required to have a return statement inside the
method body

This return statement must include the primitive or object to be returned

Methods that have a return type of void are permitted to have a return statement with no
value returned or omit the return statement entirely

    public void walk1() { }
    public void walk2() { return; }
    public String walk3() { return ""; }
    public String walk4() { } // DOES NOT COMPILE
    public walk5() { } // DOES NOT COMPILE
    String walk6(int a) { if (a == 4) return ""; } // DOES NOT COMPILE

    int integerExpanded() {
        int temp = 9;
        return temp;    
    }

    int longExpanded() {
        int temp = 9L; // DOES NOT COMPILE
        return temp;
    }

### Method Name

Method name may only contain letters, numbers, $, or _.
Also, the fi rst character is not allowed to be a number, and reserved words are not allowed.
By convention, methods begin with a lowercase letter but are not required to

    public void walk1() { }
    public void 2walk() { } // DOES NOT COMPILE
    public walk3 void() { } // DOES NOT COMPILE
    public void Walk_$() { }
    public void() { } // DOES NOT COMPILE

### Parameter List

Although the parameter list is required, it doesn’t have to contain any parameters. This
means you can just have an empty pair of parentheses after the method name

If you do have multiple parameters, you separate them with a comma

    public void walk1() { }
    public void walk2 { } // DOES NOT COMPILE
    public void walk3(int a) { }
    public void walk4(int a; int b) { } // DOES NOT COMPILE
    public void walk5(int a, int b) { }

### Optional Exception List

In Java, code can indicate that something went wrong by throwing an exception. We’ll cover
this later, “Exceptions.” For now, you just need to know that it is optional and
where in the method signature it goes if present.

In the example, InterruptedException is a type of Exception. You can list as many types of exceptions as you want in this clause sepa rated by commas. For example:

public void zeroExceptions() { }
public void oneException() throws IllegalArgumentException { }
public void twoExceptions() throws IllegalArgumentException, InterruptedException { }

The calling method can throw the same exceptions or handle them

### Method Body

The final part of a method declaration is the method body (except for abstract methods and interfaces). 
A method body is simply a code block. It has braces that contain zero or more Java statements

    public void walk1() { }
    public void walk2; // DOES NOT COMPILE
    public void walk3(int a) { int name = 5; }

### Working with Varargs

A vararg parameter must be the last element in a method’s parameter list. This implies you are only allowed to
have one vararg parameter per method.

    public void walk1(int... nums) { }
    public void walk2(int start, int... nums) { }
    public void walk3(int... nums, int start) { } // DOES NOT COMPILE
    public void walk4(int... start, int... nums) { } // DOES NOT COMPILE

    15: public static void walk(int start, int... nums) {
    16:   System.out.println(nums.length);
    17: }
    18: public static void main(String[] args) {
    19: walk(1); // 0
    20: walk(1, 2); // 1
    21: walk(1, 2, 3); // 2
    22: walk(1, new int[] {4, 5}); // 2
    23: }

Accessing a vararg parameter is also just like accessing an array. It uses array indexing.

For example:

    16: public static void run(int... nums) {
    17: System.out.println(nums[1]);
    18: }
    19: public static void main(String[] args) {
    20: run(11, 22); // 22
    21: }

