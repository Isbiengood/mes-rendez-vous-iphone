# Mes Rendez-vous V2 — autre méthode de connexion

## Problème vérifié
- Connexion Google Sheets en lecture seule fonctionnelle sur Edge Android.
- Ajout des droits écriture Sheets + Calendar : popup_closed, malgré essais.
- V1 Apps Script opérationnelle. Ne pas la modifier.

## Architecture proposée (non déployée)
- PWA plein écran, même expérience utilisateur.
- Cloudflare Worker distinct des ressources Ani'Moi.
- OAuth 2.0 authorization code + redirection Google, callback traité par Worker.
- Secrets OAuth stockés uniquement côté serveur, jamais sur GitHub.
- Jetons et sessions protégés, cookies HttpOnly Secure SameSite, validation state / CSRF, contrôle du compte autorisé.
- Synchronisation Google Sheets / Google Agenda via API, avec réconciliation des écritures partielles et prévention des doublons.
- Tests sur événements fictifs uniquement avant toute activation des opérations CRUD.
- Vérification du forfait et des quotas gratuits avant toute création de ressource.

## Étapes
1. Valider architecture et disponibilité du Worker gratuit.
2. Créer un Worker séparé (jamais dans le serveur Ani'Moi).
3. Configurer domaine de callback OAuth et secrets via interface sécurisée.
4. Tester login et lecture en environnement isolé.
5. Tester écriture, modification, suppression et rappels sur données fictives.
6. Basculer vers V2 seulement après validation, conserver V1 en sauvegarde.

## Ne jamais
- Partager le client secret ni les jetons OAuth dans le chat ou le dépôt.
- Utiliser un endpoint Apps Script anonyme donnant accès aux rendez-vous.
- Activer la facturation sans accord.
