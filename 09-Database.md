# 09 — Database
**Fati's Corner Documentation**

| | |
|---|---|
| **Projet** | Fati's Corner |
| **Version** | 1.0 — MVP |
| **SGBD** | PostgreSQL |
| **Auteur** | Fatima (Fatoush) |
| **Basé sur** | 07-UML.md (diagramme de classes) |

---

## 1. Introduction

Ce document détaille le modèle de données de Fati's Corner : tables, colonnes, types, contraintes et relations. Il traduit en schéma concret le diagramme de classes de `07-UML.md`, et servira de base directe à l'implémentation des entités JPA côté Spring Boot.

## 2. Conventions

- Toutes les tables ont une clé primaire `id` de type `BIGINT`, auto-incrémentée
- Les noms de tables sont au pluriel, en `snake_case` (ex: `users`, `order_items`)
- Les clés étrangères suivent le format `<table_singulier>_id` (ex: `user_id`, `product_id`)
- Les timestamps `created_at` et `updated_at` sont présents sur toutes les tables (bonne pratique de traçabilité)

---

## 3. Tables

### 3.1 `users`

| Colonne | Type | Contraintes |
|---|---|---|
| id | BIGINT | PK, auto-increment |
| nom | VARCHAR(100) | NOT NULL |
| email | VARCHAR(150) | NOT NULL, UNIQUE |
| mot_de_passe_hash | VARCHAR(255) | NOT NULL |
| role | VARCHAR(20) | NOT NULL — valeurs : `CLIENT`, `ADMIN`, `EMPLOYE` |
| telephone | VARCHAR(20) | NULL |
| created_at | TIMESTAMP | NOT NULL, défaut = maintenant |
| updated_at | TIMESTAMP | NOT NULL |

### 3.2 `categories`

| Colonne | Type | Contraintes |
|---|---|---|
| id | BIGINT | PK, auto-increment |
| nom | VARCHAR(100) | NOT NULL, UNIQUE |
| description | TEXT | NULL |
| created_at | TIMESTAMP | NOT NULL |

### 3.3 `products`

| Colonne | Type | Contraintes |
|---|---|---|
| id | BIGINT | PK, auto-increment |
| nom | VARCHAR(150) | NOT NULL |
| description | TEXT | NULL |
| prix | DECIMAL(10,2) | NOT NULL, > 0 |
| image_url | VARCHAR(255) | NULL — URL Cloudinary |
| disponible | BOOLEAN | NOT NULL, défaut = true |
| category_id | BIGINT | FK → `categories.id`, NOT NULL |
| created_at | TIMESTAMP | NOT NULL |
| updated_at | TIMESTAMP | NOT NULL |

### 3.4 `cart_items`

> Panier "vivant" — un item par produit ajouté par un utilisateur, supprimé après passage de commande.

| Colonne | Type | Contraintes |
|---|---|---|
| id | BIGINT | PK, auto-increment |
| user_id | BIGINT | FK → `users.id`, NOT NULL |
| product_id | BIGINT | FK → `products.id`, NOT NULL |
| quantite | INTEGER | NOT NULL, > 0 |
| created_at | TIMESTAMP | NOT NULL |

### 3.5 `orders`

| Colonne | Type | Contraintes |
|---|---|---|
| id | BIGINT | PK, auto-increment |
| user_id | BIGINT | FK → `users.id`, NOT NULL |
| statut | VARCHAR(20) | NOT NULL — `EN_ATTENTE`, `CONFIRMEE`, `EN_PREPARATION`, `PRETE`, `LIVREE`, `ANNULEE` |
| mode_recuperation | VARCHAR(20) | NOT NULL — `RETRAIT`, `LIVRAISON` |
| adresse_livraison | VARCHAR(255) | NULL — requis uniquement si `mode_recuperation = LIVRAISON` |
| total | DECIMAL(10,2) | NOT NULL, >= 0 |
| date_commande | TIMESTAMP | NOT NULL |
| created_at | TIMESTAMP | NOT NULL |
| updated_at | TIMESTAMP | NOT NULL |

### 3.6 `order_items`

> Copie figée du produit au moment de la commande (nom + prix), pour garder un historique fiable même si le produit change de prix plus tard.

| Colonne | Type | Contraintes |
|---|---|---|
| id | BIGINT | PK, auto-increment |
| order_id | BIGINT | FK → `orders.id`, NOT NULL |
| product_id | BIGINT | FK → `products.id`, NOT NULL |
| nom_produit | VARCHAR(150) | NOT NULL — copié au moment de la commande |
| quantite | INTEGER | NOT NULL, > 0 |
| prix_unitaire | DECIMAL(10,2) | NOT NULL — copié au moment de la commande |

### 3.7 `favorites`

| Colonne | Type | Contraintes |
|---|---|---|
| id | BIGINT | PK, auto-increment |
| user_id | BIGINT | FK → `users.id`, NOT NULL |
| product_id | BIGINT | FK → `products.id`, NOT NULL |
| created_at | TIMESTAMP | NOT NULL |
| | | **Contrainte UNIQUE** sur (`user_id`, `product_id`) — un produit ne peut être favori qu'une fois par utilisateur |

### 3.8 `reservations`

| Colonne | Type | Contraintes |
|---|---|---|
| id | BIGINT | PK, auto-increment |
| user_id | BIGINT | FK → `users.id`, NOT NULL |
| date_reservation | TIMESTAMP | NOT NULL |
| nombre_personnes | INTEGER | NOT NULL, > 0 |
| statut | VARCHAR(20) | NOT NULL — `CONFIRMEE`, `ANNULEE`, `TERMINEE` |
| created_at | TIMESTAMP | NOT NULL |

---

## 4. Diagramme ERD

```
erDiagram
  USERS ||--o{ CART_ITEMS : possede
  USERS ||--o{ ORDERS : passe
  USERS ||--o{ FAVORITES : marque
  USERS ||--o{ RESERVATIONS : reserve

  PRODUCTS ||--o{ CART_ITEMS : est_ajoute
  PRODUCTS ||--o{ ORDER_ITEMS : est_commande
  PRODUCTS ||--o{ FAVORITES : est_favori
  CATEGORIES ||--o{ PRODUCTS : contient

  ORDERS ||--|{ ORDER_ITEMS : contient

  USERS {
    bigint id PK
    string nom
    string email
    string mot_de_passe_hash
    string role
  }
  CATEGORIES {
    bigint id PK
    string nom
  }
  PRODUCTS {
    bigint id PK
    string nom
    decimal prix
    bigint category_id FK
  }
  CART_ITEMS {
    bigint id PK
    bigint user_id FK
    bigint product_id FK
    int quantite
  }
  ORDERS {
    bigint id PK
    bigint user_id FK
    string statut
    string mode_recuperation
    decimal total
  }
  ORDER_ITEMS {
    bigint id PK
    bigint order_id FK
    bigint product_id FK
    int quantite
    decimal prix_unitaire
  }
  FAVORITES {
    bigint id PK
    bigint user_id FK
    bigint product_id FK
  }
  RESERVATIONS {
    bigint id PK
    bigint user_id FK
    timestamp date_reservation
    int nombre_personnes
    string statut
  }
```

*(Ce bloc peut être collé dans [mermaid.live](https://mermaid.live) pour visualiser le schéma relationnel.)*

## 5. Points de conception importants

### 5.1 Pourquoi `order_items` copie le nom et le prix du produit ?

Si un produit change de prix après qu'une commande a été passée, l'historique des anciennes commandes ne doit **pas** changer rétroactivement. C'est une règle métier essentielle en e-commerce/restauration : **une facture passée est figée dans le temps**.

### 5.2 Pourquoi une contrainte UNIQUE sur `favorites` ?

Elle empêche qu'un même produit soit ajouté plusieurs fois aux favoris d'un même utilisateur — une règle de cohérence des données qu'il vaut mieux garantir au niveau de la base plutôt que seulement côté code.

### 5.3 Pourquoi `role` est une simple colonne VARCHAR et pas une table séparée ?

Avec seulement 3 rôles fixes (CLIENT, ADMIN, EMPLOYE) qui ne changeront pas dynamiquement, une colonne avec des valeurs contrôlées (enum côté Java) est plus simple et suffisante. Une table `roles` séparée serait pertinente seulement si les permissions devenaient très granulaires (ex: système de permissions personnalisées) — ce qui n'est pas le cas ici.

## 6. Index recommandés (performance)

| Table | Colonne(s) | Raison |
|---|---|---|
| users | email | Recherche fréquente lors de la connexion |
| products | category_id | Filtrage du menu par catégorie |
| orders | user_id | Consultation de l'historique client |
| cart_items | user_id | Chargement du panier d'un utilisateur |

---

## 7. Prochaine étape

Ce modèle de données servira de base directe à :
- La création des **entités JPA** en Spring Boot
- La définition des **routes API** dans `10-API.md`

---

*Toute évolution du schéma (nouvelle table, nouvelle colonne) devra être documentée ici et tracée dans `13-Decisions-Log.md`.*
