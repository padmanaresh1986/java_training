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

