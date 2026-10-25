## Username enumeration via different responses
- Faille : le serveur repond differemment selon qu'un username existe ou non
- Outil : Burp Intruder (Sniper), listes Candidate usernames / passwords du lab
- Methode 1 : username en position §...§, trier par Length -> "ae" sortait du lot (username valide)
- Methode 2 : username=ae fixe, password en position, repere le Status 302 -> mot de passe trouve
- Impact : enumeration de comptes + brute force -> prise de compte
- Correction : message d'erreur identique quel que soit le cas, limiter les tentatives (rate limit)
- REFLEXE : toute difference dans une reponse (longueur, statut, message, temps) = fuite d'info
