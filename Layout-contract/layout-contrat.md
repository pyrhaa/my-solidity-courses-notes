## Layout

Tout vos fichiers sources `.sol` devraient commencer avec un comment indiquant votre licence.

```
/*
EXAMPLES
// SPDX-License-Identifier: GPL-3.0
// SPDX-License-Identifier: MIT
// SPDX-License-Identifier: Apache 2.0
// SPDX-License-Identifier: UNLICENSED
*/
```

Le keyword `pragma` spécifie quelle version du compilateur est valide pour compiler ce fichier de contrat.

```
pragma solidity >= 0.7.0 <0.8.0;
```

Solidity supporte les instructions d'importation pour rendre modulaire votre code. Similaire à ceux disponibles en JavaScript (à partir de ES6). Cependant, Solidity ne prend pas en charge le `default export`.

```
import "filename";
import "filename" as symbolName;
import "some_library.sol" as lib;
```

Aussi, pour documenter votre code, les comments sont une option.

```
// This is a single-line comment.

/*
This is a
multi-line comment.
*/
```

Mais nous pouvons aussi commenter à l'intérieur de celui-ci grâce au **Natspec** avec les triple slash (///) ou les asterisk block (/\*\*\*/). C'est très utile pour donner des infos à propos d'une fonction ou autres.

```
// SPDX-License-Identifier: UNLICENSED
pragma solidity >= 0.7.0 <0.8.0;

/// @title Distance calculator
contract DistanceEquation {
   /**
    * @dev Calculates a distance according to the given values.
    * @param speed Speed in meter/seconds.
    * @param time Time in seconds.
    * @return distance The calculated distance in meter.
    */
    function distance(uint speedn, uint time) public pure returns(uint distance) {
        distance = speed * time;
    }
}
```
