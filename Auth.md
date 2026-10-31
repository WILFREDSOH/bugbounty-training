## Username enumeration via different responses
- Faille : le serveur repond differemment selon qu'un username existe ou non
- Outil : Burp Intruder (Sniper), listes Candidate usernames / passwords du lab
- Methode 1 : username en position §...§, trier par Length -> "ae" sortait du lot (username valide)
- Methode 2 : username=ae fixe, password en position, repere le Status 302 -> mot de passe trouve
- Impact : enumeration de comptes + brute force -> prise de compte
- Correction : message d'erreur identique quel que soit le cas, limiter les tentatives (rate limit)
- REFLEXE : toute difference dans une reponse (longueur, statut, message, temps) = fuite d'info


## 2FA simple bypass
- Faille : le serveur ne verifie pas que l'etape 2FA a ete franchie
- Exploitation : se connecter en carlos:montoya, a la page /login2 aller directement sur /my-account
- Impact : contournement total de la double authentification -> prise de compte
- Correction : cote serveur, interdire l'acces aux pages post-login tant que le 2FA n'est pas valide
- REFLEXE : une etape de securite ne protege que si le serveur verifie qu'elle a ete passee


## Password reset broken logic
- Faille : le jeton de reset n'est pas lie au compte ; le serveur se fie au username du corps
- Exploitation : Repeater, dans POST /forgot-password changer username=wiener en username=carlos + nouveau mdp
- Impact : reinitialisation du mot de passe de n'importe quel compte -> prise de compte
- Correction : lier le token au compte cote serveur, ne jamais se fier a un username fourni par le client
- REFLEXE : un jeton (reset, session, verif) doit etre valide cote serveur et lie a son proprietaire


## Juice Shop - Who broke my lock
- Brute force admin : Burp Intruder sur password, email=admin@juice-sh.op, liste SecLists -> admin123
- Faille : mot de passe faible + aucune limite de tentatives
- Reset Jim : question de securite "eldest sibling's middle name" -> Jim=Kirk (Star Trek) -> Samuel
- Faille : question de securite devinable par OSINT (indice laisse dans un avis)
- Corrections : limiter les tentatives, mdp forts, abandonner les questions de securite
