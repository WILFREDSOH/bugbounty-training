# S'entraîner sur DVWA (en local)

Environnement d'entraînement volontairement vulnérable. À lancer UNIQUEMENT en local.

## Lancer
    docker run --rm -it -p 8081:80 vulnerables/web-dvwa

Puis ouvrir http://localhost:8081
- Login : admin / password
- Cliquer "Create / Reset Database", puis se reconnecter
- DVWA Security -> choisir le niveau (Low pour débuter)

## Modules à travailler
Command Injection, SQL Injection, XSS, File Upload, CSRF, Brute Force.
Monter en difficulté (Low -> Medium -> High) pour apprendre à contourner les filtres.
