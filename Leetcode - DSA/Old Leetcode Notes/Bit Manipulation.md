#bitwise #leetcode #dsa #active
#### Common Techniques
---
```Java
//Get the binary num representation
String binaryStr = Integer.toBinaryString(x)

//Check if num is even/odd
if( (num&1) == 1 ) -> Odd num
if( (num&1) == 0 ) -> Even num

//Check if num is a power of 2
if(num>0 && (num&(num-1)) == 0) -> Power of 2

//Check if kth LSB is set
if( (num & (1<<k)) != 0 ) -> Bit set
if( (num & (1<<k)) == 0 ) -> Bit unset

//Toggle the kth bit
num = num ^ (1<<k);

//Set the kth bit
num = num | (1<<k);

//Unset the kth bit
num = num & ~(1<<k);

//Generate 2^k
num = (1<<k);

//Multiplication by 2^k
num = num << k;

//Division by 2^k
num = num >> k;

//Unset the rightmost set bit
int x = 48; // Example: 48 = 101 000 (32+16)
int res = x & (x - 1); // res = 32

//Get the rightmost set bit
int x = 40; // Example: 40 = 100 100
int res = x & -x; // res = 8

//Integer.highestOneBit() - 
//finds the most significant bit that is set and returns an int where only that bit is set.
int n = 58; // Binary: 111010
int highest = Integer.highestOneBit(n);  //  Output: 32

//Binary Representation of -ve num - In Java, -ve num are stored using 2’s complement.
x = ~x;     //Invert all bits
x = x+1;   //Add 1
```


##### Useful Wrapper Class Methods

| **Method**                                 | **Description**                                  |
| ------------------------------------------ | ------------------------------------------------ |
| `Integer.highestOneBit(int n)`             | Returns the highest (leftmost) one-bit.          |
| `Integer.lowestOneBit(int n)`              | Returns the lowest (rightmost) one-bit.          |
| `Integer.bitCount(int n)`                  | Counts the number of set bits (Hamming weight).  |
| `Integer.numberOfLeadingZeros(int n)`      | Counts 0s before the first 1 bit (MSB).          |
| `Integer.numberOfTrailingZeros(int n)`     | Counts 0s after the last 1 bit (LSB).            |
| `Integer.reverse(int n)`                   | Reverses all 32 bits of the integer.             |
| `Integer.reverseBytes(int n)`              | Reverses byte order (used in endian conversion). |
| `Integer.rotateLeft(int n, int distance)`  | Left rotates the bits.                           |
| `Integer.rotateRight(int n, int distance)` | Right rotates the bits.                          |
| `Integer.signum(int n)`                    | Returns -1, 0, or 1 based on sign of `n`.        |

---
###### Converting a decimal number to binary
![[Converting decimal number to binary.png|450]]

Steps - 
- When converting 13 from decimal to binary, we keep dividing it with 2 and obtain the remainder
- So, when 13 is divided by 2, we have quotient=6 (written below 13) and remainder = 1 (written next to 13)
- Then when 6 is divided by 2, we have quotient=3 (written below) and remainder = 0 (written next to 6)
- Then again 3 gives 1 as quotient and 1 as remainder
Finally, we write the last number first (the quotient left as 1) and then write all the remainders

Code: (Time Complexity and Space Complexity - `logn`)
```Java
String convert2Binary(int n){
	return Integer.toBinaryString(n);
}
```
```Java
String convert2Binary(int n){
	if (n == 0) return "0";

    StringBuilder binary = new StringBuilder();
	while(n!=1){
		int rem = n%2;
		binary.append(0, rem);
		n = n/2;
	}

	return binary.toString();
}
```
---
###### Converting a binary to decimal number
![[Converting binary to decimal number.png|550]]

Steps - 
-  When converting 1101 to decimal, start from the rightmost bit/least significant bit with 0-based indexing
- Create powers of 2 by using the index number - so we have  - `2^0, 2^1, 2^2, 2^3 `
- Multiply each bit by its corresponding power of 2  - `1 * 2^0  + 0 * 2^1 + 1 * 2^2 + 1 * 2^3 = 1+0+4+8 = 13 `

Code: (Time Complexity - `O(n)` and Space Complexity - `O(1)`)
```Java
int convert2Decimal(String n) {
	return Integer.parseInt(n, 2);
}
```
```Java
int convert2Decimal(String n){
	int decimal = 0;
    int power = 0;

    for (int i = n.length() - 1; i >= 0; i--) {
        char bit = n.charAt(i);
        if (bit == '1') {
            decimal += Math.pow(2, power);
        }
        power++;
    }
    
    return decimal;
}
```
---
#### Bitwise Operators
###### 1. & (Bitwise AND)
**Definition** - Sets each bit to 1 if both bits are 1.
Example - 
```Java
int a = 5;  // 0101
int b = 3;  // 0011
System.out.println(a & b);  // Output: 1 (0001)
```

###### 2. | (Bitwise OR)
**Definition** - Sets each bit to 1 if at least one bit is 1
Example -
```Java
int a = 5;  // 0101
int b = 3;  // 0011
System.out.println(a | b);  // Output: 7 (0111)
```

###### 3. ^ (Bitwise XOR)
**Definition** - Sets each bit to 1 if the bits are different
Example - 
```Java
int a = 5;  // 0101
int b = 3;  // 0011
System.out.println(a ^ b);  // Output: 6 (0110)
```

###### 4. ~ (Bitwise NOT)
**Definition** - Inverts all the bits (1's complement)
Example -
```Java
int a = 5;  // 00000000 00000101
System.out.println(~a);    // Output: -6 (in 2's complement)
```


###### 5. << (Left Shift)
**Definition** - Shifts bits to the left, fills with 0s. Multiplies by 2ⁿ.
Example - 
```Java
int a = 5;  // 0101
System.out.println(a << 1);  // Output: 10 (1010)
```

###### 6. >> (Signed Right Shift)
**Definition** - Shifts bits to the right, preserves sign bit
Example - 
```Java
int a = 5;  // 0101
System.out.println(a >> 1);  // Output: 2 (0010)
```

###### 7. >>> (Unsigned Right Shift)
**Definition** - Shifts bits right, fills with 0 regardless of sign
Example - 
```Java
int a = -5;
System.out.println(a >>> 1);  // Large positive number due to zero-fill
```

---
#### Common Bit Manipulation Techniques

4. Check if a bit is set at a given position
5. Set, clear, toggle a bit
6. Count set bits (`Hamming weight`)
7. Check if a number is a power of 2
8. Swapping numbers without a temporary variable
9. Turn off the rightmost set bit
10. Get the rightmost set bit
---
#### 🔹 Intermediate Techniques

11. Bit masking
12. Subset generation using bits
13. XOR tricks (e.g., find the unique element in array)
14. Binary representation of negative numbers (Two's complement)
15. Using bit manipulation to optimize space (bitset/bitmap)
---
#### 🔹 Advanced Concepts

16. Brian Kernighan's Algorithm (efficient set-bit count)
17. Fast Exponentiation using bitwise operations
18. Bitwise operations for permissions/access control
19. Gray code generation
20. Using Trie + Bits for problems like max XOR pair
---
#### 🔹 Optional (More Advanced/Math-heavy)

21. Bit Manipulation with Floating Points (IEEE 754)
22. Bit Hacks (e.g., next power of 2, reverse bits, etc.)