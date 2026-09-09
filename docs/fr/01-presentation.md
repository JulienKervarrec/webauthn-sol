# Chapitre 1 -- Presentation de webauthn-sol

Ce depot est une bibliotheque Solidity a un seul fichier, `src/WebAuthn.sol`,
qui verifie on-chain une assertion d authentification WebAuthn (la norme
derriere Face ID, Touch ID, les cles de securite et les gestionnaires de
mots de passe comme iCloud Keychain). Elle reprend le travail de
WebAuthn.sol de Daimo, avec une difference cle : elle essaie d abord le
precompile RIP-7212 (verification secp256r1 native, tres bon marche en
gas) et ne se rabat sur FreshCryptoLib (une implementation Solidity pure,
plus couteuse) que si ce precompile echoue ou n existe pas sur la chaine.

L objectif de la bibliotheque est de permettre a un smart contract wallet
(comme un compte Base/Coinbase Smart Wallet) d accepter une signature
produite par un passkey plutot que par une cle privee ECDSA secp256k1
classique. La fonction publique est une seule fonction, `verify`, qui
prend un challenge, un booleen `requireUV`, une struct `WebAuthnAuth` et
une cle publique (x, y), et retourne un booleen.

Le README est explicite sur un point important : cette bibliotheque ne
verifie pas l integralite de la specification WebAuthn, seulement les
etapes pertinentes pour ce cas d usage precis. Ce parcours documente a la
fois ce qui est verifie et, tout aussi important, ce qui est
deliberement ignore.
