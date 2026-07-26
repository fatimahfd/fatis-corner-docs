# Fati's Corner — Vision du Projet

**Phase 0 — Analyse des besoins**
**Version :** 1.0
**Auteur :** Fatima
**Date :** Juillet 2026

---

## 1. Vision

Fati's Corner ambitionne de devenir la référence digitale des cafés-restaurants de quartier en Mauritanie : une expérience chaleureuse et moderne, où commander un dessert, un café ou un smoothie est aussi simple que quelques clics sur son téléphone.

## 2. Mission

Offrir aux petits cafés-restaurants un outil professionnel — actuellement inaccessible sans WhatsApp ou Facebook — qui digitalise l'ensemble de leur activité : commandes, gestion des clients, statistiques et fidélisation.

## 3. Problème identifié

- Les petits cafés-restaurants gèrent leurs commandes manuellement via WhatsApp ou Facebook.
- Aucune vue d'ensemble sur les ventes, les produits populaires ou le comportement client.
- Expérience client lente et peu professionnelle (pas de suivi de commande, pas de menu structuré).
- Aucune donnée centralisée pour prendre des décisions (stock, promotions, statistiques).

## 4. Proposition de valeur

| Pour | Fati's Corner offre |
|---|---|
| Le client | Un moyen rapide, moderne et fiable de consulter le menu, commander et suivre sa commande en temps réel |
| Le restaurant (admin) | Une gestion centralisée des produits, commandes, employés et statistiques |
| L'employé | Une vue claire des commandes à préparer, sans confusion ni erreurs |

## 5. Portée du MVP (2-3 semaines)

Pour livrer une première version fonctionnelle et démontrable rapidement, le MVP se concentre sur le cœur du produit :

**Inclus dans le MVP :**
- Authentification (JWT + refresh token), 3 rôles : Client, Administrateur, Employé
- Gestion des catégories et produits (CRUD admin)
- Menu client : recherche, filtres, détails produit
- Panier
- Commandes avec les 6 états (En attente → Confirmée → En préparation → Prête → Livrée / Annulée)
- Paiement à la livraison / en espèces (le paiement mobile money est prévu en base mais pas intégré)
- Notifications Firebase sur les changements de statut de commande
- Dashboard admin basique (commandes, produits, chiffre d'affaires simple)

**Repoussé en évolutions futures (post-MVP) :**
- Intégration réelle des paiements mobile money (Bankily, Masrvi)
- Avis clients
- Codes promo et offres spéciales
- Statistiques avancées (par période, nouveaux clients, etc.)
- Applications mobiles distinctes iOS/Android, réservations, QR code tables, IA, multi-branches

## 6. Public cible

- **Clients finaux** : jeunes urbains et familles à Nouakchott habitués aux commandes via réseaux sociaux, cherchant plus de rapidité et de fiabilité.
- **Gérants de café-restaurant** : petites structures voulant se professionnaliser sans les coûts d'un système d'entreprise classique.

## 7. Critères de succès du MVP

- Un client peut créer un compte, parcourir le menu, commander et suivre sa commande de bout en bout sans blocage.
- Un administrateur peut gérer produits/catégories et voir les commandes en temps réel.
- Un employé peut consulter et mettre à jour le statut des commandes du jour.
- L'application est stable, sans bug bloquant, et déployée (backend + mobile testable).

## 8. Stack technique retenue

Backend : Java 21, Spring Boot, Spring Security, Spring Data JPA, Hibernate, JWT, Lombok, MapStruct, Swagger
Base de données : PostgreSQL (Neon)
Stockage images : Cloudinary
Notifications : Firebase Cloud Messaging
Mobile : Flutter

---

*Ce document constitue la Phase 0 du planning (cf. Cahier des Charges §11). Il sert de référence pour cadrer les décisions de conception des phases suivantes.*
