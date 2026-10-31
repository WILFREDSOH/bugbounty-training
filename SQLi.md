## Juice Shop - Login bypass par injection SQL
- Faille : l'email est colle dans la requete SQL sans nettoyage
- Connexion admin : email ' OR 1=1--  -> condition toujours vraie + reste commente
- Connexion ciblee (Bender) : bender@juice-sh.op'--  -> ferme la chaine, commente le mdp
- Impact : connexion a n'importe quel compte sans mot de passe
- Correction : requetes parametrees (prepared statements), ne jamais concatener l'entree
- CLE : une apostrophe ' qui change le comportement = signal d'injection SQL
