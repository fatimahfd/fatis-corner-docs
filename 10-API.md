# 10 — API
**Fati's Corner Documentation**

| | |
|---|---|
| **Projet** | Fati's Corner |
| **Version** | 1.0 — MVP |
| **Format** | REST / JSON |
| **Auteur** | Fatima (Fatoush) |
| **Basé sur** | 05-Requirements.md, 09-Database.md |

---

## 1. Introduction

Ce document liste toutes les routes REST de l'API Fati's Corner. Chaque route indique la méthode HTTP, le rôle requis, et une brève description. La documentation interactive complète (Swagger/OpenAPI) sera générée automatiquement depuis le code pendant le développement — ce document sert de **contrat de référence** avant l'implémentation.

## 2. Conventions générales

- Toutes les routes sont préfixées par `/api`
- Authentification via en-tête : `Authorization: Bearer <token JWT>`
- Réponses au format JSON, avec codes de statut HTTP standards :
  - `200 OK` — succès
  - `201 Created` — ressource créée
  - `400 Bad Request` — données invalides
  - `401 Unauthorized` — non authentifié
  - `403 Forbidden` — authentifié mais rôle insuffisant
  - `404 Not Found` — ressource inexistante
  - `500 Internal Server Error` — erreur serveur

---

## 3. Authentification

| Méthode | Route | Rôle requis | Description |
|---|---|---|---|
| POST | `/api/auth/register` | Public | Créer un compte client *(RF-01)* |
| POST | `/api/auth/login` | Public | Se connecter, retourne un token JWT *(RF-02)* |
| POST | `/api/auth/refresh` | Authentifié | Rafraîchir le token JWT |

**Exemple — POST `/api/auth/login`**

Requête :
```json
{
  "email": "fatoush@example.com",
  "motDePasse": "********"
}
```

Réponse (200) :
```json
{
  "token": "eyJhbGciOi...",
  "user": { "id": 1, "nom": "Fatoush", "role": "CLIENT" }
}
```

---

## 4. Profil utilisateur

| Méthode | Route | Rôle requis | Description |
|---|---|---|---|
| GET | `/api/users/me` | Authentifié | Consulter son propre profil *(RF-03)* |
| PUT | `/api/users/me` | Authentifié | Modifier son propre profil *(RF-03)* |

---

## 5. Catégories

| Méthode | Route | Rôle requis | Description |
|---|---|---|---|
| GET | `/api/categories` | Public | Lister toutes les catégories *(RF-06)* |
| POST | `/api/categories` | ADMIN | Créer une catégorie *(RF-09)* |
| PUT | `/api/categories/{id}` | ADMIN | Modifier une catégorie *(RF-09)* |
| DELETE | `/api/categories/{id}` | ADMIN | Supprimer une catégorie *(RF-09)* |

---

## 6. Produits

| Méthode | Route | Rôle requis | Description |
|---|---|---|---|
| GET | `/api/products` | Public | Lister les produits (filtrable par catégorie via `?categoryId=`) *(RF-06)* |
| GET | `/api/products/{id}` | Public | Détail d'un produit *(RF-07)* |
| POST | `/api/products` | ADMIN | Créer un produit *(RF-08)* |
| PUT | `/api/products/{id}` | ADMIN | Modifier un produit *(RF-08)* |
| DELETE | `/api/products/{id}` | ADMIN | Supprimer un produit *(RF-08)* |

**Exemple — GET `/api/products/{id}`**

Réponse (200) :
```json
{
  "id": 12,
  "nom": "Cookie double chocolat",
  "description": "Cookie moelleux au cœur fondant",
  "prix": 3.50,
  "imageUrl": "https://res.cloudinary.com/.../cookie.jpg",
  "categorie": { "id": 3, "nom": "Cookie Bar" },
  "disponible": true
}
```

---

## 7. Panier

| Méthode | Route | Rôle requis | Description |
|---|---|---|---|
| GET | `/api/cart` | Authentifié | Consulter son panier *(RF-10, RF-11)* |
| POST | `/api/cart/items` | Authentifié | Ajouter un produit au panier *(RF-10)* |
| PUT | `/api/cart/items/{id}` | Authentifié | Modifier la quantité d'un item *(RF-11)* |
| DELETE | `/api/cart/items/{id}` | Authentifié | Retirer un item du panier *(RF-11)* |

---

## 8. Commandes

| Méthode | Route | Rôle requis | Description |
|---|---|---|---|
| POST | `/api/orders` | Authentifié (CLIENT) | Passer une commande à partir du panier *(RF-12, RF-13, RF-14)* |
| GET | `/api/orders/me` | Authentifié (CLIENT) | Consulter son historique de commandes *(RF-15)* |
| GET | `/api/orders/{id}` | Authentifié (propriétaire ou ADMIN) | Détail d'une commande |
| GET | `/api/orders` | ADMIN | Lister toutes les commandes *(RF-17)* |
| PUT | `/api/orders/{id}/status` | ADMIN | Modifier le statut d'une commande *(RF-17)* |

**Exemple — POST `/api/orders`**

Requête :
```json
{
  "modeRecuperation": "LIVRAISON",
  "adresseLivraison": "Rue X, Nouakchott"
}
```

Réponse (201) :
```json
{
  "id": 45,
  "statut": "CONFIRMEE",
  "total": 12.50,
  "modeRecuperation": "LIVRAISON",
  "items": [
    { "nomProduit": "Cookie double chocolat", "quantite": 2, "prixUnitaire": 3.50 },
    { "nomProduit": "Milkshake vanille", "quantite": 1, "prixUnitaire": 5.50 }
  ]
}
```

> Note : le corps de la requête ne contient pas les items — ils sont déduits automatiquement du panier de l'utilisateur authentifié. Cela évite toute incohérence entre panier et commande.

---

## 9. Favoris

| Méthode | Route | Rôle requis | Description |
|---|---|---|---|
| GET | `/api/favorites` | Authentifié | Lister ses favoris *(RF-19)* |
| POST | `/api/favorites/{productId}` | Authentifié | Ajouter un produit aux favoris *(RF-18)* |
| DELETE | `/api/favorites/{productId}` | Authentifié | Retirer un produit des favoris *(RF-18)* |

---

## 10. Réservations

| Méthode | Route | Rôle requis | Description |
|---|---|---|---|
| POST | `/api/reservations` | Authentifié (CLIENT) | Créer une réservation *(RF-20)* |
| GET | `/api/reservations/me` | Authentifié (CLIENT) | Consulter ses réservations *(RF-21)* |
| PUT | `/api/reservations/{id}` | Authentifié (propriétaire) | Modifier sa réservation *(RF-21)* |
| DELETE | `/api/reservations/{id}` | Authentifié (propriétaire) | Annuler sa réservation *(RF-21)* |
| GET | `/api/reservations` | ADMIN | Lister toutes les réservations *(RF-22)* |

---

## 11. Gestion des erreurs (format standard)

Toute erreur retourne une structure JSON cohérente, gérée par un `@ExceptionHandler` global côté Spring Boot :

```json
{
  "timestamp": "2026-07-04T18:30:00Z",
  "status": 400,
  "error": "Bad Request",
  "message": "Le champ 'email' est invalide",
  "path": "/api/auth/register"
}
```

## 12. Documentation interactive

Une fois le backend développé, **Swagger/OpenAPI** générera automatiquement une documentation interactive testable dans le navigateur, à l'adresse `/swagger-ui.html`. Ce document (`10-API.md`) reste la référence de conception, complémentaire à Swagger.

---

## 13. Résumé de couverture

Toutes les exigences fonctionnelles (RF-01 à RF-22) de `05-Requirements.md` sont couvertes par au moins une route de ce document.

---

*Toute nouvelle route ajoutée en cours de développement devra être documentée ici et tracée dans `13-Decisions-Log.md`.*
