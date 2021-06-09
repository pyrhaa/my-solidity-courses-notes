## Operateurs

Les operateurs qui sont utilisés dans Solidity sont similaires au Javascript. Nous les groupons en 6 catégories.

---

#### Arithmetic Operators

Ces opérateurs sont utilisés pour effectuer des opérations arithmétiques.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract ArithmeticOperators {
    // initializing variables
    uint public x = 20;
    uint public y = 10;

    // addition, subtraction, multiplication, division
    uint public addition = x + y; // 30
    uint public subtraction = x - y; // 10
    uint public multiplication = x * y; // 200
    uint public division = x / y; // 2

   /**
    * @dev x++ executes the statement and then increments the value,
    * however ++x increments the value and then executes the statement
    *
    * see examples!
    */
    uint public inc = x++; // equivalent to x = x + 1
    uint public dec = x--; // equivalent to x = x - 1

    // post - pre increment, decrement
    uint public postInc = y++; // postInc = 10, y = 11
    uint public preInc = ++y; // preInc = 12, y = 12

   /**
    * @dev the modulo (%) opererator produces the remainder
    * of an integer division
    *
    * i.e. 17 % 5 = 2
    */
    uint public mod = x % y; // 20 % 10 = 0
}
```

---

#### Logical Operators

Combinaison de deux (ou plus) conditions qui requière l'usage d'opérateurs logiques.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract LogicalOperators {
   /**
    * @dev In programming, AND(&&) returns true if both operands are truthy
    * and false otherwise
    *
    * ( true && true ); // true
    * ( true && false ); // false
    * ( false && true ); // false
    * ( false && false ); // false
    */
    function andOperator(uint hour) public pure returns (bool) {
        if (hour >= 10 && hour <= 18) {
            return true;
        }

        return false;
    }

   /**
    * @dev In programming, OR(||) returns true if any of given arguments is true,
    * otherwise it returns false.
    *
    * ( true || true ); // true
    * ( true || false ); // true
    * ( false || true ); // true
    * ( false || false ); // false
    */
    bool public isWeekend = true;
    function orOperator(uint hour) public view returns (bool) {
        if (hour < 10 || hour > 18 || isWeekend) {
            return true;
        }

        return false;
    }

   /**
    * @dev NOT(!) operator converts the operand to its inverse value
    *
    * (!true); // false
    * (!false); // true
    */
    function setWeekendValue() public {
        isWeekend = false;
    }
}
```

---

#### Comparison Operators

Ces opérateurs sont utilisés pour comparer deux valeurs.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract ComparisonOperators {
    // initializing variables
    uint16 public x = 20;
    uint16 public y = 10;

    // equal (==) operator checks if two values are equal or not
    bool public isEqual = x == y; // false

    // not equal (!=)
    bool public notequal = x != y; // x is not equal to y, true

    // greater (>) than
    bool public greater = x > y; // true

    // greater than or equal to (>=)
    bool public greaterOrEqual = x >= y; // true

    // less (<) than
    bool public less = x < y; // false

    // less than or equal to (<=)
    bool public lessOrEqual = x <= y; // false
}
```

---

#### Assignment Operators

Pour assigner une valeur à une variable, Solidity prend en charge les Assignment operators.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract AssignmentOperators {
   /**
    * @dev number1 += number2 is equivalent to number1 = number1 + number2
    *
    * The operators -=, *=, /=, %= are defined accordingly.
    */
    uint public number = 2;

    uint public add = 5;
    uint public sub = 5;
    uint public mul = 5;
    uint public div = 6;
    uint public mod = 5;

    // Defining function to
    // demonstrate Assignment Operator
    function getResult() public returns(string memory) {
        add += number; // 5 + 2
        sub -= number; // 5 - 2
        mul *= number; // 5 * 2
        div /= number; // 6 / 2
        mod %= number; // 5 % 2

        return "Calculation finished!";
    }
}
```

---

#### Bitwise Operators

Ils sont usités pour effectuer des bit-level operations comme `AND`, `OR`, `XOR`, `NOT` etc.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract BitwiseOperators {
   /**
    * @dev bitwise AND(&), performs boolean AND operation on each bit
    *
    * bytes1 a = 0x15; // [00010101]
    * bytes1 b = 0xf6; // [11110110]
    *
    * i.e. (00010101) & (11110110) = (00010100)
    */
    function and(bytes1 b1, bytes1 b2) public pure returns(bytes1) {
        return b1 & b2;
    }

   /**
    * @dev bitwise OR(|), performs boolean OR operation on each bit
    *
    * bytes1 a = 0x15; // [00010101]
    * bytes1 b = 0xf6; // [11110110]
    *
    * i.e. (00010101) | (11110110) = (11110111)
    */
    function or(bytes1 b1, bytes1 b2) public pure returns(bytes1) {
        return b1 | b2;
    }

   /**
    * @dev bitwise XOR(^), performs boolean XOR operation on each bit
    *
    * XOR Table
    * 1 ^ 1 = 0
    * 0 ^ 0 = 0
    * 1 ^ 0 = 1
    * 0 ^ 1 = 1
    *
    * bytes1 a = 0x15; // [00010101]
    * bytes1 b = 0xf6; // [11110110]
    *
    * i.e. (00010101) ^ (11110110) = (11100011)
    */
    function xor(bytes1 b1, bytes1 b2) public pure returns(bytes1) {
        return b1 ^ b2;
    }

   /**
    * @dev bitwise NOT(~), performs boolean NOT operation on each bit
    *
    * bytes1 a = 0x15; // [00010101]
    *
    * i.e. ~00010101 = 11101010
    */
    function not(bytes1 b1) public pure returns(bytes1) {
        return ~b1;
    }

   /**
    * @dev bitwise left SHIFT(<<), shifts bits to the left according to given value
    *
    * bytes1 a = 0x15; // [00010101]
    *
    * i.e. 00010101 << 2 = 01010100
    */
    function leftShift(bytes1 a, uint n) public pure returns (bytes1) {
        return a << n;
    }

  /**
    * @dev bitwise right SHIFT(>>), shifts bits to the right according to given value
    *
    * bytes1 a = 0x15; // [00010101]
    *
    * i.e. 00010101 >> 2 = 00000101
    */
    function rightShift(bytes1 a, uint n) public pure returns (bytes1) {
        return a >> n;
    }
}
```

---

#### Conditional Operators

L'opérateur conditionnel (ternaire) est un opérateur qui prend trois opérandes : une condition suivie d'un point d'interrogation (`?`), puis une expression à exécuter si la condition est vraie suivie de deux points (`:`), et enfin l'expression à exécuter si la condition est fausse. Cet opérateur est fréquemment utilisé comme raccourci au `if` statement.

```
/ SPDX-License-Identifier: UNLICENSED
pragma solidity >=0.7.0 <0.8.0;

contract ConditionalOperators {
   /**
    * @dev if condition true ? then x: else y
    *
    */
    function getMax(uint x, uint y) public pure returns(uint) {
        return x >= y ? x: y;
    }

    // same functionality with the above
    function _getMax(uint x, uint y) internal pure returns(uint) {
        if (x >= y) {
            return x;
        }

        return y;
    }
}
```
