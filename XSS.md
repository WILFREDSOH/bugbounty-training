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


## DOM XSS in innerHTML sink using source location.search
- Faille : location.search inseree via innerHTML sans filtrage
- Piege : <script> ne s'execute PAS via innerHTML
- Exploitation : <img src=x onerror=alert(1)> (image cassee -> evenement onerror -> JS)
- CLE : si <script> est bloque, utiliser un gestionnaire d'evenement (onerror, onload...)
- Correction : utiliser textContent au lieu de innerHTML, ou assainir la donnee


## [Practitioner] Reflected XSS into attribute with angle brackets HTML-encoded
- Defense : chevrons < > encodes -> impossible de creer une balise
- MAIS : donnee dans value="..." et les guillemets " ne sont pas encodes
- Exploitation : "onmouseover="alert(1) -> ferme value, ajoute un evenement a la balise existante
- Declenchement : passer la souris sur le champ
- CLE : si je ne peux pas creer de balise, j'injecte un evenement dans la balise ou je suis deja
- Correction : encoder AUSSI les guillemets en sortie


## [Practitioner] Reflected XSS into a JavaScript string with angle brackets HTML encoded
- Contexte : donnee a l'interieur d'une chaine JS : var searchTerms = '...'
- Chevrons encodes -> pas de balise, mais je suis deja dans un <script>
- Exploitation : '-alert(1)-' -> ferme la chaine, execute alert, garde la syntaxe valide
- CLE : 3e contexte = dans du JS -> sortir de la chaine et ecrire du code valide
- Correction : encoder les apostrophes/guillemets dans le contexte JS
