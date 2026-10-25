## Information disclosure in error messages
- Faille : message d'erreur verbeux revele la version d'un framework
- Exploitation : /product?productId=xyz (texte au lieu d'un nombre) -> stack trace
- Impact : version connue = failles connues associees ; chemins internes exposes
- Correction : messages d'erreur generiques, desactiver le debug en production
- REFLEXE : envoyer des valeurs inattendues (texte, caracteres speciaux, vide) pour faire parler le serveur


## Information disclosure on debug page
- Faille : page de debug (phpinfo.php) oubliee, liee en commentaire HTML
- Exploitation : Ctrl+U -> commentaire vers /cgi-bin/phpinfo.php -> chercher SECRET_KEY
- Impact : fuite des variables d'environnement et de la config serveur
- Correction : supprimer les pages de debug en production
- REFLEXE : tester les chemins de debug classiques (/debug, /phpinfo.php, /status, /test)


## Source code disclosure via backup files
- Faille : fichier de sauvegarde du code source accessible (.bak)
- Exploitation : /robots.txt -> /backup -> ProductTemplate.java.bak -> mot de passe BDD en dur
- Impact : fuite du code source et de secrets codes en dur (mdp, cles API)
- Correction : ne jamais deployer de fichiers .bak/.old sur un serveur public
- REFLEXE : tester les extensions .bak .old .save .zip .txt sur les fichiers reperes
