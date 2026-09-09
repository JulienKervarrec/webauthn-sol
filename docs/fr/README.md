# Parcours francais : webauthn-sol (verification de signatures WebAuthn on-chain)

Lecture commentee du depot base/webauthn-sol : une bibliotheque Solidity qui verifie une assertion d authentification WebAuthn (passkeys) via le precompile RIP-7212, avec repli sur FreshCryptoLib.

Sommaire :

Chapitre 1 Presentation de webauthn-sol. Chapitre 2 La structure WebAuthnAuth et les hypotheses de conception. Chapitre 3 Verifier le type et le challenge. Chapitre 4 Les flags authenticatorData et le calcul du hash signe. Chapitre 5 RIP-7212 puis repli sur FreshCryptoLib. Chapitre 6 Limites et perimetre de ce parcours.

Ce parcours est une lecture pedagogique du code source et de la documentation du depot, sans installation ni execution du projet.
