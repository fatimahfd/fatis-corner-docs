# API REST — Fati's Corner

**Phase 8 — API REST**
**Version :** 1.0
**Auteur :** Fatima
**Date :** Juillet 2026

Base URL : `/api/v1`
Documentation interactive générée via Swagger/OpenAPI sur `/swagger-ui.html` (cf. Phase 6).
Auth : header `Authorization: Bearer <JWT>` sauf endpoints marqués **public**.

---

## 1. Authentification

| Méthode | Route | Accès | Description |
|---|---|---|---|
| POST | `/auth/register` | Public | Crée un compte client |
| POST | `/auth/login` | Public | Authentifie, renvoie JWT + refresh token |
| POST | `/auth/refresh` | Public | Génère un nouveau JWT à partir du refresh token |
| POST | `/auth/logout` | Authentifié | Invalide le refresh token côté serveur |
| POST | `/auth/forgot-password` | Public | Envoie un lien de réinitialisation par email |
| POST | `/auth/reset-password` | Public | Réinitialise le mot de passe via le token reçu |

## 2. Utilisateurs

| Méthode | Route | Accès | Description |
|---|---|---|---|
| GET | `/users/me` | Authentifié | Profil de l'utilisateur connecté |
| PUT | `/users/me` | Authentifié | Met à jour son propre profil |
| GET | `/users` | Admin | Liste tous les utilisateurs (filtre par rôle) |
| POST | `/users/employes` | Admin | Crée un compte employé |
| PATCH | `/users/{id}/desactiver` | Admin | Désactive un compte (employé ou client) |

## 3. Catégories

| Méthode | Route | Accès | Description |
|---|---|---|---|
| GET | `/categories` | Public | Liste des catégories |
| POST | `/categories` | Admin | Crée une catégorie |
| PUT | `/categories/{id}` | Admin | Modifie une catégorie |
| DELETE | `/categories/{id}` | Admin | Supprime une catégorie |

## 4. Produits

| Méthode | Route | Accès | Description |
|---|---|---|---|
| GET | `/products` | Public | Liste paginée, filtres `?categorie=&recherche=&disponible=` |
| GET | `/products/{id}` | Public | Détail d'un produit |
| GET | `/products/populaires` | Public | Produits les plus commandés |
| POST | `/products` | Admin | Crée un produit |
| PUT | `/products/{id}` | Admin | Modifie un produit |
| DELETE | `/products/{id}` | Admin | Supprime un produit |

## 5. Panier

| Méthode | Route | Accès | Description |
|---|---|---|---|
| GET | `/cart` | Client | Récupère le panier courant |
| POST | `/cart/items` | Client | Ajoute un article `{produitId, quantite}` |
| PUT | `/cart/items/{itemId}` | Client | Modifie la quantité d'un article |
| DELETE | `/cart/items/{itemId}` | Client | Retire un article |
| DELETE | `/cart` | Client | Vide le panier |

## 6. Commandes

| Méthode | Route | Accès | Description |
|---|---|---|---|
| POST | `/orders` | Client | Crée une commande à partir du panier `{modePaiement}` |
| GET | `/orders/mes-commandes` | Client | Historique des commandes du client connecté |
| GET | `/orders/{id}` | Client (propriétaire) | Détail + statut d'une commande |
| GET | `/orders` | Employé, Admin | Liste des commandes, filtres `?statut=&date=` |
| GET | `/orders/du-jour` | Employé | Commandes du jour, triées par heure |
| PATCH | `/orders/{id}/statut` | Employé, Admin | Change le statut `{statut}` |
| PATCH | `/orders/{id}/annuler` | Client, Employé, Admin | Annule une commande |

## 7. Notifications

| Méthode | Route | Accès | Description |
|---|---|---|---|
| GET | `/notifications` | Authentifié | Liste ses notifications, filtre `?lue=false` |
| PATCH | `/notifications/{id}/lue` | Authentifié | Marque une notification comme lue |

## 8. Dashboard administrateur

| Méthode | Route | Accès | Description |
|---|---|---|---|
| GET | `/dashboard/resume` | Admin | Chiffre d'affaires + nombre de commandes (aujourd'hui / total) |
| GET | `/dashboard/produits-populaires` | Admin | Classement des produits les plus vendus |

---

## 9. Format de réponse standard

**Succès**
```json
{
  "data": { },
  "message": "OK"
}
```

**Erreur**
```json
{
  "error": "VALIDATION_ERROR",
  "message": "Le champ email est invalide",
  "details": [ ]
}
```

## 10. Codes HTTP utilisés

| Code | Usage |
|---|---|
| 200 | Succès (lecture, mise à jour) |
| 201 | Ressource créée |
| 400 | Erreur de validation |
| 401 | Non authentifié / token invalide |
| 403 | Authentifié mais rôle insuffisant |
| 404 | Ressource introuvable |
| 409 | Conflit (ex : email déjà utilisé) |

---

*Cette spécification d'API sert de contrat direct pour la Phase 9 (Backend Spring Boot — implémentation des controllers) et la Phase 10 (Frontend — appels API depuis Flutter).*
