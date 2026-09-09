# Chapitre 3 -- Verifier le type et le challenge

Les deux premieres verifications de `verify` correspondent aux etapes 11
et 12 de la specification WebAuthn (le code cite les numeros d etapes
officiels en commentaire, ce qui rend la correspondance avec le
standard facile a auditer).

Etape 11 : le champ `type` du JSON doit valoir exactement `"webauthn.get"`
(la reponse a une demande d authentification, par opposition a
`"webauthn.create"`, la creation d un credential). Le code extrait la
sous-chaine a `typeIndex` sur 21 caracteres -- exactement la longueur de
`"type":"webauthn.get"` -- et compare son hash `keccak256` a une constante
precalculee `_EXPECTED_TYPE_HASH`. Extraire une tranche a un index fourni
par l appelant plutot que de parser tout le JSON est un choix d
optimisation de gas assume ; c est a l appelant (ou a un test) de garantir
que l index fourni est correct, la fonction ne le revalide pas
independamment.

Etape 12 : le challenge doit apparaitre, encode en base64url, a
`challengeIndex` dans le JSON. Le code reconstruit la chaine attendue
`"challenge":"<base64url(challenge)>"` avec `Base64.encodeURL`
d OpenZeppelin, extrait la meme longueur de sous-chaine a `challengeIndex`,
et compare les deux hashs. Les etapes 13, 14 et 15 de la specification
(verification de l origine, du `topOrigin`, et de la structure JSON
complete) sont explicitement sautees, comme annonce au chapitre 2.
