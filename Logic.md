# Logique metier (business logic)

## Excessive trust in client-side controls
- Faille : le prix est envoye par le client et accepte tel quel par le serveur
- Exploitation : Burp Intercept ON, POST /cart, changer price=133700 en price=1, Forward
- Impact : achat a prix arbitraire (fraude)
- Correction : prix, remises, quantites calcules et verifies cote serveur
- REFLEXE : intercepter chaque transaction, reperer les champs sensibles modifiables (price, quantity, discount)
