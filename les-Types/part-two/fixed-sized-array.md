## Fixed-sized byte arrays

Les value types `bytes1`, `bytes2`, …, `bytes32` incluent une séquence d'octets de 1 à 32. Le mot clé `byte` est un alias pour `byte1`. À côté, le value type de taille fixe a une member function nommée `length` qui donne la longueur fixe du byte array. Et donc, des bytes avec une fixed-size variable peuvent être passés entre les contrats.

Le type `byte[]` est un tableau d'octets, mais en raison des règles de remplissage, il gaspille 31 octets(bytes) d'espace pour chaque élément (sauf en storage). Il est préférable d'utiliser le type `bytes` à la place.

> Notes:
>
> > Si vous souhaitez accéder à la représentation en octets d'une string `s`, utilisez `bytes(s).length` / `bytes(s)[7] = 'x';`.
> > Gardez à l'esprit que vous accédez aux low-level bytes de la représentation UTF-8, et non aux caractères individuels.

Nous utilisons la représentation hexadécimale pour les initialiser dans Solidity :

```
bytes1 a = 0x15; // [00010101]
bytes1 b = 0xf6; // [11110110]
```
