# 05 — Requirements
**Fati's Corner Documentation**

| | |
|---|---|
| **Projet** | Fati's Corner |
| **Version** | 1.0 — MVP |
| **Auteur** | Fatima (Fatoush) |
| **Basé sur** | 03-Cahier-des-Charges.md |

---

## 1. Introduction

Ce document traduit le périmètre défini dans `03-Cahier-des-Charges.md` en exigences précises, numérotées et vérifiables. Il sert de référence directe pour la conception (UML, base de données, API) et pour la rédaction des tests.

## 2. Rôles du système

Le système prévoit **trois rôles** dès la conception, afin d'éviter une refonte future de l'authentification et de la base de données — même si seuls Client et Admin sont pleinement développés en V1.

| Rôle | Statut en V1 | Description |
|---|---|---|
| **Client** | Développé | Utilisateur final qui consulte le menu, commande, réserve |
| **Admin** | Développé | Gère les produits, commandes, réservations |
| **Employé** | Prévu dans la structure, non développé | Rôle futur destiné à la préparation des commandes (accès restreint, sans droits de gestion complets) |

---

## 3. Exigences fonctionnelles (RF)

### 3.1 Authentification & Profil

| ID | Exigence |
|---|---|
| RF-01 | Le client peut créer un compte avec email, mot de passe et informations de base |
| RF-02 | Le client peut se connecter avec son email et son mot de passe |
| RF-03 | Le client peut consulter et modifier son profil |
| RF-04 | Le système attribue un rôle (Client, Admin, Employé) à chaque compte |
| RF-05 | L'admin peut se connecter via une authentification dédiée avec ses droits spécifiques |

### 3.2 Menu

| ID | Exigence |
|---|---|
| RF-06 | Le client peut consulter la liste des catégories de produits (Coffee Bar, Dessert Bar, etc.) |
| RF-07 | Le client peut consulter le détail d'un produit (nom, description, prix, image) |
| RF-08 | L'admin peut ajouter, modifier ou supprimer un produit |
| RF-09 | L'admin peut ajouter, modifier ou supprimer une catégorie |

### 3.3 Panier & Commande

| ID | Exigence |
|---|---|
| RF-10 | Le client peut ajouter un produit au panier |
| RF-11 | Le client peut modifier la quantité ou retirer un produit du panier |
| RF-12 | Le client peut passer une commande à partir du panier |
| RF-13 | Le client choisit un mode de récupération : retrait sur place ou livraison |
| RF-14 | Le système simule une validation de paiement (sans intégration bancaire réelle) |
| RF-15 | Le client peut consulter l'historique de ses commandes passées |
| RF-16 | Le client reçoit une notification à chaque changement de statut de sa commande (confirmée, en préparation, prête, livrée) |
| RF-17 | L'admin peut consulter la liste des commandes et modifier leur statut |

### 3.4 Favoris

| ID | Exigence |
|---|---|
| RF-18 | Le client peut ajouter ou retirer un produit de ses favoris |
| RF-19 | Le client peut consulter la liste de ses produits favoris |

### 3.5 Réservation

| ID | Exigence |
|---|---|
| RF-20 | Le client peut réserver une table pour une date et une heure données |
| RF-21 | Le client peut consulter, modifier ou annuler sa réservation |
| RF-22 | L'admin peut consulter et gérer les réservations |

---

## 4. Exigences non fonctionnelles (RNF)

| ID | Catégorie | Exigence |
|---|---|---|
| RNF-01 | Performance | Les temps de réponse de l'API doivent rester raisonnables pour une utilisation fluide (cible indicative : réponses courantes sous 1 seconde en conditions normales) |
| RNF-02 | Sécurité | Les mots de passe sont hashés (jamais stockés en clair) |
| RNF-03 | Sécurité | L'authentification utilise des tokens JWT avec expiration |
| RNF-04 | Sécurité | Chaque route de l'API vérifie le rôle de l'utilisateur avant d'autoriser l'action (contrôle d'accès basé sur les rôles) |
| RNF-05 | Sécurité | Toutes les données entrantes sont validées côté backend avant traitement |
| RNF-06 | Disponibilité | Le backend doit rester accessible de façon stable pendant les phases de test et de démonstration |
| RNF-07 | Responsive | L'interface Flutter doit s'adapter correctement à différentes tailles d'écran mobile |
| RNF-08 | Maintenabilité | Le code respecte les principes SOLID et une architecture en couches claire (Controller / Service / Repository) |
| RNF-09 | Maintenabilité | Le code est documenté et suit des conventions de nommage cohérentes |
| RNF-10 | Scalabilité | L'architecture (base de données, API) doit pouvoir accueillir de nouveaux rôles, fonctionnalités et volumes de données sans refonte majeure |

---

## 5. Traçabilité

Chaque exigence de ce document devra être :
- Reformulée en **User Story** dans `06-User-Stories.md`
- Reflétée dans le **modèle de données** (`09-Database.md`)
- Exposée via une **route API** correspondante (`10-API.md`)

---

*Toute nouvelle exigence identifiée en cours de développement devra être ajoutée ici avec un identifiant unique (RF-xx / RNF-xx), et tracée dans `13-Decisions-Log.md`.*
