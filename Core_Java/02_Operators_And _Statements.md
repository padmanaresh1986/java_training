# Understanding Java Operators

A Java operator is a special symbol that can be applied to a set of variables, values, or literals referred to as operands and that returns a result. 

Three flavors of operators are available in Java: unary, binary, and ternary. 

These types of operators can be applied to one, two, or three operands, respectively.

| Operator     | Symbols and examples        | 
| ----------- | ----------- |
| Post-unary operators  |expression++, expression--|   
|  Pre-unary operators | ++expression, --expression |    
|  Other unary operators | +, -, ! |  
|  Multiplication/Division/Modulus | *, /, %  |  
|  Addition/Subtraction | +, -  |  
|  Shift operators | <<, >>, >>> |  
|  Relational operators  | <, >, <=, >=, instanceof |  
|  Equal to/not equal to | ==, != |  
|  Logical operators | &, ^, | |  
| Short-circuit logical operators  | &&, || |  
|  Ternary operators | boolean expression ? expression1 : expression2 |  
| Assignment operators  | =, +=, -=, *=, /=, %=, &=, ^=, !=, <<=, >>=, >>>= |  
--------------------------------------------


### Working with Unary Operators

| Unary operator     | Description       | 
| ----------- | ----------- |
| + | Indicates a number is positive, although numbers are assumed
to be positive in Java unless accompanied by a negative unary
operator |
| - | Indicates a literal number is negative or negates an expression |
| ++ | Increments a value by 1 |
| -- | Decrements a value by 1 |
| ! | Inverts a Boolean’s logical value |


    int counter = 0;
    System.out.println(counter); // Outputs 0
    System.out.println(++counter); // Outputs 1
    System.out.println(counter); // Outputs 1
    System.out.println(counter--); // Outputs 1
    System.out.println(counter); // Outputs 0


## Logical Operators

The logical operators, (&), (|), and (^), may be applied to both numeric and boolean data
types

### AND 

AND is only true if both operands are true. (x & y)

### INCLUSIVE OR
Inclusive OR is only false if both operands are false.(x | y)

### EXCLUSIVE OR 

Exclusive OR is only true if the operands are different.(x ^ y)

### short-circuit operators 

The short-circuit operators are nearly identical to the logical opera-
tors, & and |, respectively, except that the right-hand side of the expression may never be
evaluated if the fi nal result can be determined by the left-hand side of the expression

&& and ||, which are often referred to asshort-circuit operators

    boolean x = true || (y < 4);

Referring to the truth tables, the value x can only be false if both sides of the expression
are false. Since we know the left-hand side is true, there’s no need to evaluate the right-hand
side, since no value of y will ever make the value of x anything other than true.

A more common example of where short-circuit operators are used is checking for null
objects before performing an operation, such as this:

    if(x != null && x.getValue() < 5) {
        // Do something
    }

    if(x != null & x.getValue() < 5) { // Throws an exception if x is null
        // Do something
    }

## Equality Operators

Determining equality in Java can be a nontrivial endeavor as there’s a semantic difference
between “two objects are the same” and “two objects are equivalent.”

equals operator == 

not equals operator !=

Comparing two numeric primitive types. If the numeric values are of different data types, the values are automatically promoted as previously described. 

For example,

5 == 5.00 returns true since the left side is promoted to a double

For object comparison, the equality operator is applied to the references to the objects, not the objects they point to. 

Two references are equal if and only if they point to the same object, or both point to null.

 Let’s take a look at some examples:

    File x = new File("myFile.txt");
    File y = new File("myFile.txt");
    File z = x;
    System.out.println(x == y); // Outputs false
    System.out.println(x == z); // Outputs true


## Understanding Java Statements

### if-then Statement

Often, we only want to execute a block of code under certain circumstances. The if-then
statement

    if(hourOfDay < 11)
        System.out.println("Good Morning");

    if(hourOfDay < 11) {
        System.out.println("Good Morning");
        morningGreetingCount++;
    }


### if-then-else Statement

Let’s expand our example a little. What if we want to display a different message if it is 11 a.m. or later?

    if(hourOfDay < 11) {
        System.out.println("Good Morning");
    }
    if(hourOfDay >= 11) {
        System.out.println("Good Afternoon");
    }


This seems a bit redundant, though, since we’re performing an evaluation on hourOfDay
twice. It’s also wasteful because in some circumstances the cost of the boolean expression
we’re evaluating could be computationally expensive

    if(hourOfDay < 11) {
        System.out.println("Good Morning");
    } else {
        System.out.println("Good Afternoon");
    }


    if(hourOfDay < 11) {
        System.out.println("Good Morning");
    } else if(hourOfDay < 15) {
        System.out.println("Good Afternoon");
    } else {
        System.out.println("Good Evening");
    }

### Ternary Operator

the ternary operator, is the only operator that takes three operands and is of the form

    booleanExpression ? expression1 : expression2

The ternary operation is really a condensed form of an if- then-else statement that returns a value

Example : both code snippets are same 

    int y = 10;
    final int x;
    if(y > 5) {
    x = 2 * y;
    } else {
    x = 3 * y;
    }

    // same as above code 
    int y = 10;
    int x = (y > 5) ? (2 * y) : (3 * y);

Note that it is often helpful for readability to add parentheses around the expressions in
ternary operations, although it is certainly not required

### The switch Statement

A switch statement, is a complex decision-making struc- ture in which a single value is evaluated and flow is redirected to the first matching branch, known as a case statement.

If no such case statement is found that matches the value, an optional default statement will be called. If no such default option is available, the entire
switch statement will be skipped.

Data types supported by switch statements include the following

    int and Integer
    byte and Byte
    short and Short
    char and Character
    int and Integer
    String
    enum values

Note that boolean and long, and their associated wrapper classes, are not supported by switch statements

    int dayOfWeek = 5;
        switch(dayOfWeek) {
        default:
            System.out.println("Weekday");
            break;
        case 0:
            System.out.println("Sunday");
            break;
        case 6:
            System.out.println("Saturday");
            break;
    }

    With a value of dayOfWeek of 5, this code will output: Weekday

### while Statement/Loop

A repetition control structure, which we refer to as a loop, executes a statement of code multiple times in succession

    int roomInBelly = 5;
    public void eatCheese(int bitesOfCheese) {
        while (bitesOfCheese > 0 && roomInBelly > 0) {
            bitesOfCheese--;
            roomInBelly--;
        }
    System.out.println(bitesOfCheese+" pieces of cheese left");

This method takes an amount of food, in this case cheese, and continues until the mouse
has no room in its belly or there is no food left to eat. With each iteration of the loop, the
mouse “eats” one bite of food and loses one spot in its belly. By using a compound boolean
statement, you ensure that the while loop can end for either of the conditions.


Infinite Loops

    int x = 2;
    int y = 5;
    while(x < 10)
         y++;

###  do-while Statement

Java also allows for the creation of a do-while loop, which like a while loop, is a repetition control structure with a termination condition and statement

Unlike a while loop, though, a do-while loop guaran-
tees that the statement or block will be executed at least once.

    int x = 0;
    do {
         x++;
    } while(false);
    System.out.println(x); // Outputs 1

Java will execute the statement block fi rst, and then check the loop condition. Even
though the loop exits right away, the statement block was still executed once and the pro-
gram outputs a 1.

### The for Statement

The Basic for Statement

Variables declared in the initialization block of a for loop have limited scope and
are only accessible within the for loop

Let’s take a look at an example that prints the numbers 0 to 9:

    for(int i = 0; i < 10; i++) {
        System.out.print(i + " ");
    }

    //Creating an Infinite Loop

    for( ; ; ) {
        System.out.println("Hello World");
    }

    //Adding Multiple Terms to the for Statement

    int x = 0;
    for(long y = 0, z = 4; x < 5 && y < 10; x++, y++) {
        System.out.print(y + " ");
    }
    System.out.print(x)


    //Redeclaring a Variable in the Initialization Block

    int x = 0;
    for(long y = 0, x = 4; x < 5 && y < 10; x++, y++) { // DOES NOT COMPILE
        System.out.print(x + " ");
    }

    //Using Incompatible Data Types in the Initialization Block

    for(long y = 0, int x = 4; x < 5 && y<10; x++, y++) { // DOES NOT COMPILE
        System.out.print(x + " ");
    }

    //Using Loop Variables Outside the Loop

    for(long y = 0, x = 4; x < 5 && y < 10; x++, y++) {
        System.out.print(y + " ");
    }
    System.out.print(x); // DOES NOT COMPILE

### The for-each Statement

Starting with Java 5.0, Java developers have had a new type of enhanced for loop at their disposal, one specifically designed for iterating over arrays and Collection objects.

    final String[] names = new String[3];
    names[0] = "Lisa";
    names[1] = "Kevin";
    names[2] = "Roger";
    for(String name : names) {
        System.out.print(name + ", ");
    }

    This code will compile and print:
    Lisa, Kevin, Roger,

    java.util.List<String> values = new java.util.ArrayList<String>();
    values.add("Lisa");
    values.add("Kevin");
    values.add("Roger");
    for(String value : values) {
         System.out.print(value + ", ");
    }

    This code will compile and print the same values:
    Lisa, Kevin, Roger,


The right-hand side is an array or Iterable object and the left-hand side has a matching type

    String names = "Lisa";
    for(String name : names) { // DOES NOT COMPILE
         System.out.print(name + " ");
    }

    String[] names = new String[3];
    for(int name : names) { // DOES NOT COMPILE
         System.out.print(name + " ");
    }


### Comparing for and for-each Loops

Below codes will perform same operation

    for(String name : names) {
        System.out.print(name + ", ");
    }

    for(int i=0; i < names.length; i++) {
        String name = names[i];
        System.out.print(name + ", ");
    }


For objects that inherit java.lang.Iterable, there is a different, but similar, conversion.
For example, assuming values is an instance of List\<Integer>, as we saw in the second
example, the following two loops are equivalent:

    for(int value : values) {
         System.out.print(value + ", ");
    }

    for(java.util.Iterator<Integer> i = values.iterator(); i.hasNext(); ) {
        int value = i.next();
        System.out.print(value + ", ");
    }

While the for-each statement is convenient for working with lists in many cases, it does
hide access to the loop iterator variable. If we wanted to print only the comma between
names, we could convert the example into a standard for loop, as in the following example:

While the for-each statement is convenient for working with lists in many cases, it does
hide access to the loop iterator variable. If we wanted to print only the comma between
names, we could convert the example into a standard for loop, as in the following example:

    java.util.List<String> names = new java.util.ArrayList<String>();
    names.add("Lisa");
    names.add("Kevin");
    names.add("Roger");
    for(int i=0; i<names.size(); i++) {
        String name = names.get(i);
        if(i>0) {
             System.out.print(", ");
        }
        System.out.print(name);
    }

    This sample code would output the following:
    Lisa, Kevin, Roger
    
It is also common to use a standard for loop over a for-each loop if comparing mul-tiple elements in a loop within a single iteration, as in the following example. 

    int[] values = new int[3];
    values[0] = 10;
    values[1] = new Integer(5);
    values[2] = 15;
    for(int i=1; i<values.length; i++) {
    System.out.print(values[i]-values[i-1]);
    }
    This sample code would output the following: -5, 10,

Notice that we skip the first loop’s execution, since value[-1] is not defi ned and would throw an
IndexOutOfBoundsException error.

### Understanding Advanced Flow Control

### Nested Loops

First off, loops can contain other loops. For example, consider the following code that iter-
ates over a two-dimensional array, an array that contains other arrays as its members

    int[][] myComplexArray = {{5,2,1,3},{3,9,8,9},{5,7,12,7}};
    for(int[] mySimpleArray : myComplexArray) {
        for(int i=0; i<mySimpleArray.length; i++) {
            System.out.print(mySimpleArray[i]+"\t");
        }
        System.out.println();
    }

    5 2 1 3
    3 9 8 9
    5 7 12 7

Nested loops can include while and do-while, as shown in this example. 

    int x = 20;
    while(x>0) {
        do {
            x -= 2
        } while (x>5);
        x--;
        System.out.print(x+"\t");
    }

    3   0

The first time this loop executes, the inner loop repeats until the value of x is 4. The
value will then be decremented to 3 and that will be the output at the end of the fi rst itera-
tion of the outer loop. On the second iteration of the outer loop, the inner do-while will
be executed once, even though x is already not greater than 5. As you may recall, do-while
statements always execute the body at least once. This will reduce the value to 1, which will
be further lowered by the decrement operator in the outer loop to 0. Once the value reaches
0, the outer loop will terminate. The result is that the code will output the following:     3 0


### Adding Optional Labels

A label is an optional pointer to the head of a statement that allows the application flow to jump to it or break from it. 

It is a single word that is proceeded by a colon (:). For example, we can add optional labels to one of the pre-
vious examples:    

    int[][] myComplexArray = {{5,2,1,3},{3,9,8,9},{5,7,12,7}};
    OUTER_LOOP: for(int[] mySimpleArray : myComplexArray) {
        INNER_LOOP: for(int i=0; i<mySimpleArray.length; i++) {
            System.out.print(mySimpleArray[i]+"\t");
        }
    System.out.println();
    }

Optional labels are often only used
in loop structures

### The break Statement

As you saw when working with switch statements, a break statement transfers the flow
of control out to the enclosing statement. The same holds true for break statements that
appear inside of while, do-while, and for loops, as it will end the loop early

break statement can take an optional label parameter.
Without a label parameter, the break statement will terminate the nearest inner loop it is currently in the process of executing. 

The optional label parameter allows us to break out
of a higher level outer loop

    public class SearchSample {
        public static void main(String[] args) {
            int[][] list = {{1,13,5},{1,2,5},{2,7,2}};
            int searchValue = 2;
            int positionX = -1;
            int positionY = -1;
            PARENT_LOOP: for(int i=0; i<list.length; i++) {
                for(int j=0; j<list[i].length; j++) {
                    if(list[i][j]==searchValue) {
                    positionX = i;
                    positionY = j;
                    break PARENT_LOOP;
                }
            }
            }
            if(positionX==-1 || positionY==-1) {
            System.out.println("Value "+searchValue+" not found");
            } else {
            System.out.println("Value "+searchValue+" found at: " +
            "("+positionX+","+positionY+")");
            }
        }
    }

### The continue Statement

continue statement, a statement that causes flow to finish the execution of the current loop

You may notice the syntax of the continue statement mirrors that of the break state-ment. In fact, the statements are similar in how they are used, but with different results.

While the break statement transfers control to the enclosing statement, the continue
statement transfers control to the boolean expression that determines if the loop should continue

    public class SwitchSample {
        public static void main(String[] args) {
            FIRST_CHAR_LOOP: for (int a = 1; a <= 4; a++) {
                for (char x = 'a'; x <= 'c'; x++) {
                    if (a == 2 || x == 'b')
                    continue FIRST_CHAR_LOOP;
                    System.out.print(" " + a + x);
                }
            }
        }
    }

With the structure as defi ned, the loop will return control to the parent loop any time
the fi rst value is 2 or the second value is b. This results in one execution of the inner loop
for each of three outer loop calls. The output looks like this: 1a 3a 4a

Now, imagine we removed the FIRST_CHAR_LOOP label in the continue statement so that
control is returned to the inner loop instead of the outer. See if you can understand how the
output will be changed to:
1a 1c 3a 3c 4a 4c

Finally, if we remove the continue statement and associated if-then statement alto-
gether, we arrive at a structure that outputs all the values, such as:
1a 1b 1c 2a 2b 2c 3a 3b 3c 4a 4b 4c























