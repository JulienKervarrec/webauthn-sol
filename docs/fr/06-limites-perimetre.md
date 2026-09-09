# Chapitre 6 -- Limites et perimetre de ce parcours

Ce parcours couvre l integralite de la logique de `src/WebAuthn.sol` : la
struct `WebAuthnAuth`, la fonction `verify` et ses six controles successifs
(malleabilite, type, challenge, flags UP/UV, hash du message, verification
signature avec repli RIP-7212 -> FreshCryptoLib).

Sont volontairement laisses hors champ : le contenu des bibliotheques
externes importees (`FreshCryptoLib/FCL_ecdsa.sol`, la partie Base64 d
OpenZeppelin, `solady/utils/LibString.sol`), qui appartiennent a d autres
depots et ne sont utilisees ici qu en dependances ; la suite de tests
Foundry du depot, mentionnee dans le README (`forge test`) mais non
executee dans le cadre de ce parcours documentaire ; et les sept points
explicitement non verifies par la bibliotheque elle-meme (origine,
topOrigin, rpIdHash, backup state, extensions, compteur de signature,
attestation), qui sont documentes au chapitre 2 comme faisant partie du
perimetre de confiance delegue a l authenticator plutot que comme des
oublis.

L objectif reste le meme que pour les parcours precedents de cette
bibliotheque : comprendre precisement comment une bibliotheque Solidity
relativement courte transforme une assertion WebAuthn produite par un
telephone ou une cle de securite en une verification cryptographique
verifiable on-chain, sans pretendre couvrir l integralite de la
specification WebAuthn.
