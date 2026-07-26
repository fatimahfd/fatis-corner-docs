# Cahier des Charges — Fati's Corner

**Version :** 2.0 (révisée)
**Auteur :** Fatima
**Date :** Juillet 2026
**Phase :** 1 — Cahier des charges

---

## 1. Présentation du projet

**Nom du projet :** Fati's Corner
**Slogan :** *Made with Love, Served with Warmth*

**Description**

Fati's Corner est une application mobile moderne destinée à un café-restaurant spécialisé dans : desserts, cookies, crêpes, gaufres, milkshakes, smoothies, boissons chaudes, cafés, chocolats, jus naturels.

L'objectif est d'offrir une expérience chaleureuse permettant aux clients de consulter le menu, commander facilement et profiter d'un service rapide.

## 2. Contexte

Aujourd'hui, beaucoup de petits cafés utilisent uniquement WhatsApp ou Facebook pour gérer leurs commandes. Fati's Corner souhaite disposer d'une application professionnelle permettant : gestion complète du restaurant, commandes en ligne, gestion des clients, paiements, statistiques, fidélisation.

## 3. Objectifs

- Digitaliser le restaurant
- Réduire le temps de commande
- Faciliter la gestion du personnel
- Améliorer l'expérience client
- Augmenter les ventes
- Centraliser les données

## 4. Utilisateurs

### Client
Créer un compte, se connecter, consulter le menu, rechercher des produits, ajouter au panier, passer commande, payer, suivre sa commande, consulter son historique, laisser un avis *(évolution future)*, gérer son profil.

### Administrateur
Gérer les utilisateurs, catégories, produits, commandes, paiements, employés, consulter les statistiques, publier des promotions *(évolution future)*.

### Employé
Consulter les commandes, changer le statut, gérer les préparations, voir les commandes du jour.

## 5. Fonctionnalités

### 5.1 Authentification
Inscription, connexion, déconnexion, mot de passe oublié, réinitialisation, JWT, refresh token, vérification email.

### 5.2 Produits
Chaque produit possède : nom, description, image, prix, catégorie, disponibilité, ingrédients, taille, temps de préparation.

### 5.3 Catégories
Desserts, cookies, crêpes, gaufres, cafés, boissons, milkshakes, smoothies.

### 5.4 Menu
Filtrer, rechercher, trier, voir les détails, voir les produits populaires.

### 5.5 Panier
Ajouter, supprimer, modifier quantité, vider panier, calcul automatique.

### 5.6 Commandes
États : En attente → Confirmée → En préparation → Prête → Livrée / Annulée.

### 5.7 Paiement (MVP)
- Espèces
- Paiement à la livraison

Le champ « mode de paiement » est prévu en base de données pour anticiper l'intégration future de :
- **Bankily**
- **Masrvi**

*(Intégration réelle prévue en évolution future — hors scope du MVP 2-3 semaines)*

### 5.8 Promotions *(évolution future)*
Codes promo, réductions, offres spéciales.

### 5.9 Avis *(évolution future)*
Noter, commenter, modifier son avis.

### 5.10 Notifications
Commande confirmée, commande prête, paiement accepté *(promotion : évolution future)*, via Firebase Cloud Messaging.

### 5.11 Dashboard Administrateur (MVP)
Chiffre d'affaires simple, ventes, commandes, produits populaires.
*(Statistiques avancées par période et nouveaux clients : évolution future)*

## 6. Exigences non fonctionnelles

### Performance
- Temps de réponse < 2 secondes
- API rapide
- Pagination

### Sécurité
- JWT
- BCrypt
- Validation des données
- Protection XSS
- Protection contre l'injection SQL
- Contrôle des rôles

*Note : la protection CSRF a été retirée du périmètre — l'authentification par JWT stateless (envoyé en header) n'est pas vulnérable au CSRF de la même manière qu'une authentification par cookie de session.*

### Disponibilité
Application disponible 24h/24.

### Compatibilité
Mobile (Android/iOS via Flutter).

### Accessibilité
Interface simple, navigation intuitive.

## 7. Architecture

```
Application Flutter (mobile)
        ↓
    API REST
        ↓
Backend Spring Boot
        ↓
PostgreSQL (Neon)
        ↓
Cloudinary (stockage images)
        ↓
Firebase Cloud Messaging (notifications)
```

## 8. Technologies

### Backend
Java 21, Spring Boot, Spring Security, Spring Data JPA, Hibernate, Maven, JWT, Lombok, MapStruct, Validation, Swagger/OpenAPI

### Base de données
PostgreSQL (hébergée sur Neon)

### Stockage images
Cloudinary

### Notifications
Firebase Cloud Messaging

### Mobile
Flutter

### Outils
Git, GitHub, Postman, Docker, IntelliJ IDEA, VS Code, Figma

## 9. Structure générale du projet

```
fatis-corner/
├── backend/
├── frontend/        (application Flutter)
├── database/
├── docs/
├── design/
└── docker/
```

## 10. Livrables

- Cahier des charges *(ce document)*
- Maquettes UI/UX
- User Stories & Cas d'utilisation
- Diagrammes UML
- Base de données PostgreSQL
- API REST documentée
- Application Flutter (mobile)
- Dashboard Administrateur (web ou intégré à l'appli selon décision Phase 6)
- Documentation technique
- Déploiement

## 11. Roadmap du projet

| Phase | Livrable | Statut |
|---|---|---|
| 0 | Vision du projet | ✅ |
| 1 | Cahier des charges | 🔄 |
| 2 | Étude des besoins | ⏳ |
| 3 | User Stories | ⏳ |
| 4 | Cas d'utilisation (Use Cases) | ⏳ |
| 5 | Diagrammes UML | ⏳ |
| 6 | Architecture du projet | ⏳ |
| 7 | Base de données | ⏳ |
| 8 | API REST | ⏳ |
| 9 | Backend Spring Boot | ⏳ |
| 10 | Frontend (Flutter) | ⏳ |
| 11 | Tests | ⏳ |
| 12 | Déploiement | ⏳ |

**Contrainte de temps :** ~15 jours ouvrés à 3-4h/jour (2-3 semaines).

## 12. Évolutions futures

- Intégration réelle des paiements mobile money (Bankily, Masrvi)
- Système d'avis clients
- Codes promo et offres spéciales
- Statistiques avancées (par période, nouveaux clients)
- Programme de fidélité
- Système de réservation
- QR Code pour les tables
- Intelligence artificielle pour les recommandations
- Gestion multi-branches
