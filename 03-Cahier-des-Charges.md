# 03 — Cahier des Charges
**Fati's Corner Documentation**

| | |
|---|---|
| **Projet** | Fati's Corner |
| **Version** | 1.0 — MVP |
| **Auteur** | Fatima (Fatoush) |
| **Basé sur** | 01-Project-Vision.md, 02-Business-Analysis.md |

---

## 1. Présentation du projet

Fati's Corner est un café-restaurant chaleureux proposant une offre complète (boissons, desserts, pâtisserie, petit-déjeuner) accompagnée d'une application mobile permettant de digitaliser l'expérience client : consultation du menu, commande, réservation, suivi, et gestion interne du restaurant.

## 2. Contexte

Les cafés modernes proposent souvent une simple vente de produits, sans réelle expérience client ni présence numérique cohérente permettant de prolonger cette expérience au-delà du lieu physique (voir `02-Business-Analysis.md`).

## 3. Problématique

Comment créer une plateforme numérique qui permette à un café chaleureux comme Fati's Corner d'offrir une expérience client fluide et moderne (commande, réservation, suivi), tout en simplifiant la gestion interne du restaurant ?

## 4. Solution proposée

Une solution composée de :
- Une **application mobile cliente** (Flutter) : menu, commande, réservation, suivi
- Un **backend** (Spring Boot + PostgreSQL) exposant une API REST sécurisée
- Une **interface d'administration basique** permettant à l'équipe du restaurant de gérer produits, commandes et réservations

## 5. Objectifs

### Objectif général
Développer une plateforme numérique fonctionnelle (V1/MVP) permettant à Fati's Corner de gérer ses commandes, réservations et son menu, tout en offrant une expérience client fluide.

### Objectifs spécifiques
- Digitaliser la prise de commande (retrait ou livraison)
- Permettre la réservation de table en ligne
- Simplifier la gestion des produits et des commandes côté restaurant
- Fidéliser les clients grâce à un espace personnel (historique, favoris)
- Offrir un suivi clair de l'état des commandes via notifications

## 6. Périmètre du projet (Scope MVP)

> Ce périmètre est le résultat d'une décision explicite de prioriser un socle solide avant d'étendre les fonctionnalités (voir section 6.3 — Hors périmètre).

### 6.1 Fonctionnalités Client (MVP)

| Fonctionnalité | Description |
|---|---|
| Inscription / Connexion | Création de compte et authentification sécurisée |
| Profil | Consultation et modification des informations personnelles |
| Consultation du menu | Parcours des catégories et produits disponibles |
| Panier | Ajout, modification, suppression de produits avant commande |
| Commande | Passage de commande avec choix entre **retrait sur place** ou **livraison** |
| Paiement simulé | Validation fictive du paiement (pas d'intégration bancaire réelle en V1) |
| Historique des commandes | Consultation des commandes passées et de leur statut |
| Favoris | Marquer des produits comme favoris pour un accès rapide |
| Notifications | Alertes sur le statut de la commande (confirmée, en préparation, prête, livrée) |
| Réservation de table | Réservation d'une table pour une date/heure donnée |

### 6.2 Fonctionnalités Admin (MVP)

| Fonctionnalité | Description |
|---|---|
| Gestion des produits | Ajout, modification, suppression de produits et catégories |
| Suivi des commandes | Visualisation et mise à jour du statut des commandes |
| Suivi des réservations | Visualisation et gestion des réservations de table |

### 6.3 Hors périmètre pour le MVP (reporté en V2+)

- Avis clients (notation, commentaires)
- Programme de fidélité et coupons de réduction
- Suivi de livraison en temps réel (géolocalisation)
- Paiement réel (intégration bancaire)
- Expériences spéciales (Reading Corner, Memory Wall, Today's Hug, etc.)
- Application web et dashboard admin avancé
- Statistiques et rapports détaillés côté admin

*Ces éléments restent partie intégrante de la vision long terme du projet (voir `01-Project-Vision.md` et le futur Brand Book) mais ne font pas partie du développement initial.*

## 7. Contraintes du projet

- **Équipe** : projet actuellement porté par une seule personne (contexte étudiant)
- **Budget** : nul à limité (pas d'intégration de paiement réel, pas d'infrastructure payante en V1)
- **Délai** : pas de deadline externe imposée — rythme de travail étape par étape
- **Technique** : stack imposée — Flutter (frontend), Spring Boot (backend), PostgreSQL (base de données)

## 8. Livrables attendus pour cette phase

- Le présent cahier des charges validé
- La base pour la rédaction de `05-Requirements.md` (exigences détaillées) et `06-User-Stories.md`
- Un périmètre clair servant de référence pour toutes les décisions techniques futures (architecture, base de données, API)

---

*Ce document constitue la référence officielle du périmètre fonctionnel du MVP. Toute demande d'ajout de fonctionnalité en cours de développement devra être consignée dans `13-Decisions-Log.md` avant d'être intégrée.*
