# Injection de commande (Command Injection)

## DVWA - niveau Low
- Faille : l'entree est passee a une commande systeme (ping) sans nettoyage
- Exploitation : 127.0.0.1; whoami  -> ping PUIS whoami execute
- Commandes utiles : whoami, id, ls -la, cat /etc/passwd
- Impact : execution de commandes -> lecture de fichiers, souvent controle total du serveur
- Separateurs : ;  &&  |  &  ||
- Correction : ne jamais passer l'entree a une commande systeme ; liste blanche stricte (ex. IP valide)


# Command Injection (RCE)

## DVWA - niveau Low
- Faille : l'entree est passee a une commande systeme sans nettoyage (ping -c 4 ENTREE)
- Exploitation : 127.0.0.1; whoami  -> le ; enchaine une 2e commande
- Resultats : whoami=www-data (compte du serveur web), id=uid 33, cat /etc/passwd lit un fichier
- Separateurs de commandes : ;  &&  |  &  ||
- Impact : RCE = execution de commandes sur le serveur -> lecture de fichiers, reverse shell, root
- Correction : ne jamais passer une entree a une commande systeme ; utiliser des fonctions sures
- /etc/passwd : liste des comptes ; shell /bin/bash = compte connectable ; mdp dans /etc/shadow


## DVWA - Medium et High (contournement de filtre)
- Medium : filtre efface && et ; -> contourne avec | (pipe) : 127.0.0.1 | whoami
- High : filtre efface ;, &, ||, et "| " (pipe+espace) MAIS pas "|" seul
  -> contourne SANS espace : 127.0.0.1|whoami
- LECON : une liste noire est fragile ; tester chaque variante (espace, casse, encodage)
- Bonne defense = liste blanche (n'autoriser que le format attendu, ex. une IP)
