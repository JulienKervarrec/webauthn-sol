# Chapitre 2 -- La structure WebAuthnAuth et les hypotheses de conception

`WebAuthnAuth` regroupe six champs : `authenticatorData` (les octets bruts
produits par l authenticator, qui encodent notamment les flags de
presence/verification de l utilisateur), `clientDataJSON` (le JSON signe
par le client, contenant le type d operation, le challenge et l origine),
`challengeIndex` et `typeIndex` (les positions ou trouver "challenge" et
"type" dans ce JSON, pour eviter un parsing JSON complet on-chain,
couteux en gas), et enfin `r`/`s`, les deux composantes de la signature
ECDSA sur la courbe secp256r1 (aussi appelee P-256, la courbe utilisee par
les passkeys, differente de secp256k1 utilisee par Ethereum).

Le commentaire NatSpec de `verify` liste explicitement quatre choses
verifiees (type "webauthn.get", challenge attendu, flags de presence et
signature valide) et sept choses volontairement NON verifiees : l origine
dans `clientDataJSON` (delegue a l authenticator), le `topOrigin` en
contexte cross-origin/iframe (suppose absent), le `rpIdHash` (delegue a
l authenticator via App Site Association / Asset Links), l etat de backup
du credential, les extensions client, le compteur de signature
anti-rejeu, et l objet d attestation. Cette liste n est pas un oubli :
c est une delimitation deliberee du perimetre de confiance, qui s appuie
sur le fait que les authenticators de qualite (iCloud Keychain, Google
Password Manager) appliquent deja ces controles correctement.
