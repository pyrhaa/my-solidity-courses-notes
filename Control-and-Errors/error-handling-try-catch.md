#### try / catch

Les exceptions lors de la création du contrat et des appels externes peuvent être détectées avec le `try`/`catch`.
`try` keyword doit être suivi par expression à un appel d'une external function ou une création de contrat (`new ContractName()`).

L'erreur qui est produite après par `revert("reasonString")` ou `require(false, "reasonString")`
a entraîné l'invocation de la clause catch du type `catch Error(string memory reason)`.
