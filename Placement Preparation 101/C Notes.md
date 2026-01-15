#c #placement-preparation #one-shot-notes

##### Introduction to C Language
- **C** is a powerful and widely-used **procedural programming language**.
- Developed by **Dennis Ritchie** in the early 1970s.    
- Used in system programming, embedded systems, operating systems (e.g., Unix).

##### Structure of a C Program
This is the standard template of a C program, any C program contains this - 
```C
#include <stdio.h>     // stdio.h is a library that contains basic functions

int main() {         
	printf("Hello, World!");  //printf() is a function from stdio.h that is used to display the output (here, Hello World!)
	
    // Remaining code goes here...
    return 0;          
}
```

##### What is a Variable?
- A named space in memory to store a value.

*How to create a variable?*
```C
int age = 18;
float pi = 3.14;
char grade = 'A';
```

###### Constant
```C
const int myNum = 15;  // myNum will always be 15  
myNum = 10;  // error: assignment of read-only variable 'myNum'
```

*Format Specifiers*
Used for printing variables with `printf()` to tell what type of data are the variables storing
```C
int myNum = 15;             // Integer (whole number)
float myFloatNum = 5.99;    // Floating point number - suitable for 6-7 decimals
char myLetter = 'D';        // Character
double myDouble = 1.666666; // Double - suitable up to 15 decimals
char myStr[] = "Sarthak";
bool myBool = true;

printf("Integer: %d\n", myNum);       // %d or %i = integer
printf("Float: %.2f\n", myFloatNum); // %.2f limits the decimal places to 2
printf("Character: %c\n", myLetter); // %c = char
printf("Boolean: %d\n", myBool);     // bool are printed as integer = 0/1
printf("Double: %lf\n", myDouble);   // %lf = double
printf("String: %s", myStr);    //%s = string

printf("%c", myStr);    //prints only last char of string

//you can also use format specifier to print values without variables
printf("My favorite number is: %d", 15);

//%u = unsigned int/ %lu= unsigned long
```

> [!info]+ Simultaneous declaration
> You can do this in C :
> ```C
int x, y, z;  
x = y = z = 50;  
printf("%d", x + y + z);   //prints 150
> ```

---
*Input using `scanf()`:*
- `scanf()` is used to take the input from the user. 
- Generally, the input taken from user is stored inside a variable
```C
int age;
scanf("%d", &age);   //'&' symbol is added before the variable name to store the value in the variable from scanf
```


**Rules for naming variables**
- Names can contain letters, digits and underscores
- Names must begin with a letter or an underscore (`_`)
- Names are case-sensitive (`myVar` and `myvar` are different variables)
- Names cannot contain whitespaces or special characters like !, #, %, etc.
- Reserved words (such as `int`) cannot be used as names

*Scientific Numbers*
```C
float f1 = 35e3;  //'e' represents the number of 0s to append
double d1 = 12E4;

printf("%f\n", f1);   //35,000.00
printf("%lf", d1);    //120,000.00
```
---
#### Operators
| **Type**                | **Operator** | **Description**                                    | **Example**                    |
| ----------------------- | ------------ | -------------------------------------------------- | ------------------------------ |
| **Arithmetic**          | `+,-,*,/`    | Add, subtract, multiply, divide                    |                                |
|                         | `%`          | Modulo operator - calculates the remainder         | `62%12` = 2                    |
| **Relational**          | `==`         | Equal to                                           | `a == b` → true if equal       |
|                         | `!=`         | Not equal to                                       | `a != b` → true if not         |
|                         | `<`          | Less than                                          | `a < b`                        |
|                         | `>`          | Greater than                                       | `a > b`                        |
|                         | `<=`         | Less than or equal to                              | `a <= b`                       |
|                         | `>=`         | Greater than or equal to                           | `a >= b`                       |
| **Logical**             | `&&`         | Logical AND – true if both conditions are true     | `a > 0 && b > 0`               |
|                         | \|\|         | Logical OR - true if either condition is true      | `a > 0` \|\|` b > 0`           |
|                         | `!`          | Logical NOT – inverts the condition                | `!a>0`                         |
| **Assignment**          | `=`          | Assigns right value to left variable               | `a = 10`                       |
|                         | `+=`         | Adds and assigns                                   | `a += 5` (same as `a = a + 5`) |
|                         | `-=`         | Subtracts and assigns                              | `a -= 2`                       |
|                         | `*=`         | Multiplies and assigns                             | `a *= 3`                       |
|                         | `/=`         | Divides and assigns                                | `a /= 2`                       |
|                         | `%=`         | Modulo and assigns                                 | `a %= 3`                       |
| **Bitwise**             | `&`          | AND – bit-level operation                          | `a & b`                        |
|                         | \|           | OR – bit-level operation                           | `a` \| `b`                     |
|                         | `^`          | XOR – bit-level difference                         | `a ^ b`                        |
|                         | `~`          | NOT – flips bits                                   | `~a`                           |
|                         | `<<`         | Left shift – shifts bits to the left               | `a << 1`                       |
|                         | `>>`         | Right shift – shifts bits to the right             | `a >> 2`                       |
| **Increment/Decrement** | `++`         | Increments value by 1                              | `i++` or `++i`                 |
|                         | `--`         | Decrements value by 1                              | `i--` or `--i`                 |
| **Special**             | `sizeof`     | Returns size (in bytes) of a data type or variable | `sizeof(int)`                  |
|                         | `&`          | Address of variable                                | `&a`                           |
|                         | `*`          | Pointer dereferencing                              | `*ptr`                         |
|                         | `? :`        | Ternary operator – short if-else                   | `x = (a > b) ? a : b`          |

#### Type Conversion
```C
//Implicit Conversion
float myFloat = 9;  
printf("%f", myFloat); // 9.000000

//Explicit Conversion
int num1 = 5;  
int num2 = 2;  
float sum = (float) num1 / num2;  
printf("%f", sum); // 2.500000
```

---
#### Creating Strings
```C
//Declare string
char greetings[] = "Hello World!";  
printf("%s", greetings);

//Another way to declare
//Here you have to include '\0' - null character explicitly
char greetings1[] = {'H', 'e', 'l', 'l', 'o', ' ', 'W', 'o', 'r', 'l', 'd', '!', '\0'};  
printf("%s", greetings1);

printf("%d", sizeof(greetings)); //prints 13 (includes \0 in the size)

printf("%d", strlen(greetings)); //prints 12 (correct) (is in string.h)
```

#### String Functions
```C
#include <stdio.h>
#include <string.h>

int main() {
    char str1[50] = "Hello";
    char str2[50] = "World";
    char str3[50];

    // String length
    printf("Length of str1: %u\n", strlen(str1)); 
    //5

    // String concatenation
    strcat(str1, " ");
    strcat(str1, str2);
    printf("Concatenated string: %s\n", str1);
    //Hello World

    // String copy
    strcpy(str3, str1);
    printf("Copied string (str3): %s\n", str3);
	//Hello World
	
    // String comparison
    int cmp = strcmp(str1, str3);
    printf("Comparison result (str1 vs str3): %d\n", cmp); //0 = equal, !=0 means not equal
	//0

    // String reverse
    strrev(str3);  // Note: `strrev` is not standard in all compilers; GCC doesn't have it.
    printf("Reversed string (str3): %s\n", str3);
	//

    // Finding a substring
    char *substr = strstr(str1, "World");
    if (substr) {
        printf("Substring found: %s\n", substr);
    } else {
        printf("Substring not found\n");
    }

    // Character search
    char *chr = strchr(str1, 'o');
    if (chr) {
        printf("First occurrence of 'o': %s\n", chr);
    } else {
        printf("'o' not found\n");
    }

    return 0;
}
```

#### Getting User Input
```C
printf("Type a number AND a character and press enter: \n");  
scanf("%d %c", &myNum, &myChar);

//Getting string input
printf("Enter your string: \n");   
scanf("%s", firstName);        
//don't have to use '&' when using scanf to take-in strings
//but fails when a space or newline comes

fgets(firstName, sizeof(fullName), stdin);
//string variable, size of variable, stdin
```

---
#### Memory Address
```C
int myAge = 43;  
printf("%p", **&myAge**); // Outputs 0x7ffe5367e044
// %p is used to print pointer's address
```

### Pointers
A pointer is a variable that <mark style="background: #FFB8EBA6;">stores the memory address</mark> of another variable as its value.
A pointer variable points to a data type (like int) of the same type, and is created with the * operator.

```C
int myAge = 43;          
int* ptr = &myAge;       

// Output the value of myAge (43)
printf("%d\n", myAge);  

// Output the memory address of myAge (e.g., 0x7ffe5367e044)
printf("%p\n", &myAge);  

// Output the memory address of myAge with the pointer (e.g., 0x7ffe5367e044)
printf("%p\n", ptr);

// Dereference: Output the value of myAge with the pointer (43)
printf("%d\n", *ptr);
```

**Pointers and Array**
The memory address of the first element is the same as the name of the array
```C
int myNumbers[4] = {25, 50, 75, 100};

printf("%p\n", myNumbers);             //25
printf("%p\n", &myNumbers[0]);         //25
printf("%d\n", *(myNumbers + 1));      //50
```

#### Struct
Structures (also called structs) are a way to group several related variables into one place. Each variable in the structure is known as a member of the structure.
Equivalent to "class" in Java/Python
```C
struct myStructure {
  int myNum;
  char myLetter;
};

int main() {
  struct myStructure s1;

  s1.myNum = 13;
  s1.myLetter = 'B';

  printf("My number: %d\n", s1.myNum);
  printf("My letter: %c\n", s1.myLetter);

  return 0;
}
```

> [!Problem with the Strings!!]
> If you have declared a string inside the struct, you can't simply assign the string to a struct - `s1.myString = "Some text";`
> This is because in C, string is a array and you can only initialize array during declaration, or you would have to set each element one-by-one
> Eg., 
> `char arr[] = "Some text";`
> `char arr[];`
> `arr[0] = 'S'; arr[1] = 'o'; .....`
> 
> Now you can do 1 thing - use the strcpy()
> `strcpy(s1.myString, "Some text");`
> 
> However this is completely valid:
> `struct myStructure s1 = {13, 'B', "Some text"};`

>[!imp]
> Use -> whenever you're working with a pointer to a structure.
> Use . when you're working with a direct instance of a structure.
> ```C
> struct Point p = malloc(sizeof(struct Point));
> p->x = 5;  // Assign values
> p->y = 10;
> printf("x = %d, y = %d\n", p->x, p->y);     //p->x is equivalent to "(*p).x"
> free(p);   // Don't forget to free memory
> ```

#### Enums
Enums are used to give names to constants, which makes the code easier to read and maintain.
Use enums when you have values that you know aren't going to change, like month days, days, colors, deck of cards, etc.

```C
enum Level {
  LOW,
  MEDIUM,
  HIGH
};
int main() {
    enum Level myVar = MEDIUM;
    printf("%d", myVar);   //prints 1
    
    myVar = LOW;
    printf("%d", myVar);   //prints 0
    return 0;
}
```

You can also assign values to each enum value 
```C
enum Level {  
  LOW = 25,  
  MEDIUM = 50,  
  HIGH = 75  
};

printf("%d", myVar); // Now outputs 25
```


#### C Memory Allocation
C doesn't have its own memory management like Java. So one has to individually take care of it

**Static Memory**
Static memory is memory that is reserved for variables before the program runs. Allocation of static memory is also known as compile time memory allocation.
```C
int students[20];  
printf("%lu", sizeof(students)); // 80 bytes are allocated at compile time
```

*Stack memory* - Variables declared inside a function use stack memory rather than static memory. When a function is called, stack memory is allocated for the variables in the function. When the function returns the stack memory is freed.

**Dynamic Memory**
Dynamic memory is memory that is allocated after the program starts running. Allocation of dynamic memory can also be referred to as runtime memory allocation.
`malloc()` and`calloc()` of `stdlib.h` can be used to perform dynamic memory allocation

```C
int *ptr1 = malloc(sizeof(*ptr1));  //4 bytes
int *ptr2 = calloc(1, sizeof(*ptr2));  //4 bytes
```

`realloc()` can be used to reallocate memory
It returns a NULL pointer if it is not able to allocate more memory. This is very unlikely, but it is worth keeping in mind 
```C
  int *ptr1, *ptr2, size;

  // Allocate memory for four integers
  size = 4 * sizeof(*ptr1);
  ptr1 = malloc(size);

  printf("%d bytes allocated at address %p \n", size, ptr1);
  //16 bytes allocated at address 0x5640317512a0

  // Resize the memory to hold six integers
  size = 6 * sizeof(*ptr1);
  ptr2 = realloc(ptr1, size);

  printf("%d bytes reallocated at address %p \n", size, ptr2);
  //24 bytes reallocated at address 0x5640317512a0
```

`free()` is used to free/deallocate the memory
```C
free(ptr1);
```
