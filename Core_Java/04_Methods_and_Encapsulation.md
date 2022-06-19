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