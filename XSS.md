## Reflected XSS into HTML context with nothing encoded
- Faille : l'entree de recherche est reaffichee dans le HTML sans encodage
- Exploitation : rechercher <script>alert(1)</script> -> execution du JS
- Impact : vol de session/cookies, actions au nom de la victime (via un lien piege)
- Correction : encoder les caracteres speciaux en sortie (< devient &lt;)
- REFLEXE : tout ce que je tape qui se reaffiche dans la page -> tester une injection


## Stored XSS into HTML context with nothing encoded
- Faille : un commentaire est stocke et reaffiche sans encodage a chaque visite
- Exploitation : poster <script>alert(1)</script> dans le champ Comment
- Impact : TOUS les visiteurs executent le code (vol de session de masse) -> le plus grave
- Difference avec le reflechi : pas besoin de piege, c'est stocke cote serveur
- Correction : encoder en sortie + valider les entrees


## DOM XSS in document.write sink using source location.search
- Faille : le JS du site insere location.search dans un document.write sans filtrage
- Source : location.search (le ?search=...) ; Sink : document.write
- Contexte : ma donnee atterrit dans un attribut src="..." d'une balise img
- Exploitation : "><script>alert(1)</script> (" ferme l'attribut, > ferme la balise)
- Correction : ne pas ecrire une donnee utilisateur dans un sink dangereux ; encoder selon le contexte
- CLE : adapter le payload au CONTEXTE ou la donnee atterrit (plein HTML / attribut / JS)
