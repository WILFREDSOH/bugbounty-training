## Unprotected admin functionality
- Faille : panneau admin accessible sans authentification
- Decouverte : /robots.txt revele /administrator-panel
- Impact : n'importe qui supprime/modifie des utilisateurs
- Correction : controle d'acces cote serveur sur /administrator-panel

## Unprotected admin functionality with unpredictable URL
- Faille : panneau admin a URL "secrete" mais exposee
- Decouverte : URL ecrite en clair dans le JS (code source, Ctrl+U)
- Impact : acces admin complet
- Correction : controle d'acces cote serveur, pas une URL secrete


## User role controlled by request parameter
- Faille : le role admin est decide par une valeur envoyee par le client (cookie Admin=false)
- Decouverte : requete /my-account dans Burp, cookie Admin=false visible
- Exploitation : Send to Repeater, changer Admin=false en Admin=true, acces /admin
- Impact : n'importe quel utilisateur devient admin et supprime des comptes
- Correction : stocker le role cote serveur, ne jamais se fier a une valeur du client


## User role can be modified in user profile
- Faille : le serveur accepte que le client lui renvoie son propre roleid
- Decouverte : POST /my-account/change-email renvoie "roleid":1 dans la reponse
- Exploitation : Repeater, ajouter "roleid":2 dans le JSON, le serveur l'accepte
- Impact : elevation de privilege (utilisateur -> admin)
- Correction : le role ne doit jamais etre modifiable via une requete client
- Reflexe : tout champ vu dans une reponse, tester de le renvoyer modifie

## User ID controlled by request parameter (IDOR pur)
- Faille : l'identifiant du compte est dans l'URL (?id=wiener) et modifiable
- Exploitation : remplacer id=wiener par id=carlos -> acces a son compte et sa cle API
- Impact : vol de donnees de n'importe quel utilisateur
- Correction : verifier cote serveur que l'utilisateur connecte a le droit de voir cet id
- REFLEXE OR : tout id=, user=, account=, doc=, invoice= dans une requete -> le changer et regarder


## User ID controlled by request parameter, with unpredictable user IDs
- Faille : IDOR avec GUID aleatoire, mais le GUID est expose ailleurs sur le site
- Decouverte : le GUID de carlos apparait dans ses articles de blog (Ctrl+U, chercher carlos)
- Exploitation : recuperer le GUID, le mettre dans /my-account?id=<GUID carlos>
- Impact : vol de cle API malgre un id "imprevisible"
- Correction : controle d'acces cote serveur (un id secret n'est pas une protection)
- REFLEXE : id aleatoire -> chercher ou le site le divulgue (profils, articles, API, JS)

## User ID controlled by request parameter with data leakage in redirect
- Faille : IDOR ou le site redirige (302) mais fuite les donnees avant la redirection
- Cle : le navigateur suit la redirection et cache le contenu ; Burp montre la reponse brute
- Exploitation : Repeater, id=carlos, lire la reponse 302 -> cle API dedans
- Impact : vol de donnees malgre une "protection" par redirection
- Correction : ne renvoyer aucune donnee sensible dans une reponse de redirection
- REFLEXE : toujours lire la reponse dans Burp, pas seulement l'ecran du navigateur
