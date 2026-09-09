# Chapitre 4 -- Les flags authenticatorData et le calcul du hash signe

Apres le type et le challenge, la fonction verifie deux bits dans
`authenticatorData[32]` (le 33e octet, l octet des flags dans la structure
WebAuthn) : `_AUTH_DATA_FLAGS_UP` (bit 0, "User Present" -- l utilisateur a
interagi physiquement avec l authenticator) et, seulement si `requireUV`
est vrai, `_AUTH_DATA_FLAGS_UV` (bit 2, "User Verified" -- une verification
plus forte comme une empreinte ou un code). Ces etapes correspondent aux
etapes 16 et 17 de la specification ; l etape 18 (extensions client) est
sautee.

Vient ensuite le calcul du message effectivement signe (etapes 19 et 20).
WebAuthn ne signe pas directement le challenge : il signe la concatenation
de `authenticatorData` et du hash SHA-256 de `clientDataJSON`. Le code
calcule donc `clientDataJSONHash = sha256(clientDataJSON)`, puis
`messageHash = sha256(abi.encodePacked(authenticatorData, clientDataJSONHash))`.
C est ce `messageHash`, et non le challenge original, qui est verifie
contre la signature `(r, s)` et la cle publique `(x, y)`.

Une garde precede tout ce travail : si `s > _P256_N_DIV_2` (la moitie de
l ordre de la courbe secp256r1), la fonction retourne `false`
immediatement. C est une protection standard contre la malleabilite de
signature ECDSA -- sans cette borne, une meme signature valide pourrait
etre reecrite sous une seconde forme valide avec un `s` different.
