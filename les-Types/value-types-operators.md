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
