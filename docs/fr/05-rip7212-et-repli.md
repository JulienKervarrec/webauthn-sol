# Chapitre 5 -- RIP-7212 puis repli sur FreshCryptoLib

La verification cryptographique finale illustre le compromis central de
cette bibliotheque. `_VERIFIER` est l adresse `0x100`, celle proposee par
la RIP-7212 pour un precompile natif de verification secp256r1. Le code
encode `(messageHash, r, s, x, y)` et fait un `staticcall` vers cette
adresse.

Le commentaire du code explique un piege subtil : un `staticcall` ne
revert pas si l adresse n a pas de code (donc si la chaine ne supporte pas
encore RIP-7212, l appel "reussit" silencieusement avec un retour vide).
Le code distingue donc `success` (l appel bas niveau n a pas revert) de
`valid` (`ret.length > 0`, signe que le precompile existe vraiment et a
repondu). Seulement si les deux sont vrais, le resultat decode est utilise
directement. Dans tous les autres cas -- precompile absent, ou present
mais signature jugee invalide (auquel cas `ret.length` vaut aussi 0
d apres le commentaire) -- le code retombe sur
`FCL_ecdsa.ecdsa_verify(...)`, l implementation Solidity pure de
FreshCryptoLib.

Cette double verification a un cout assume, note explicitement dans le
code : une signature reellement invalide sera verifiee deux fois (une
fois par le precompile, une fois par FCL) avant d etre rejetee -- un cout
de gas que le commentaire suggere de simuler hors chaine pour eviter de le
payer inutilement en production. Le README ajoute une limite pratique :
FreshCryptoLib utilise le precompile `ModExp` (`0x05`), absent sur
certaines chaines comme Polygon zkEVM ; sur ces chaines, sans RIP-7212 non
plus, la bibliotheque ne fonctionne simplement pas.
