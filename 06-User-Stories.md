# 06 — User Stories
**Fati's Corner Documentation**

| | |
|---|---|
| **Projet** | Fati's Corner |
| **Version** | 1.0 — MVP |
| **Auteur** | Fatima (Fatoush) |
| **Basé sur** | 05-Requirements.md |

---

## 1. Introduction

Ce document reformule chaque exigence fonctionnelle de `05-Requirements.md` sous forme de **User Stories**, format standard en développement agile :

> **En tant que** [rôle], **je veux** [action], **afin de** [bénéfice].

Chaque story est accompagnée de **critères d'acceptation** — des conditions précises qui permettent de vérifier qu'une fonctionnalité est réellement terminée et correcte.

---

## 2. Authentification & Profil

### US-01 — Créer un compte *(RF-01)*
**En tant que** client, **je veux** créer un compte avec mon email et un mot de passe, **afin de** pouvoir accéder à mon espace personnel et passer commande.

**Critères d'acceptation :**
- L'email doit être valide et unique dans le système
- Le mot de passe doit respecter une longueur minimale
- Un message d'erreur clair s'affiche si l'email existe déjà

### US-02 — Se connecter *(RF-02)*
**En tant que** client, **je veux** me connecter avec mon email et mon mot de passe, **afin de** retrouver mon compte et mes informations.

**Critères d'acceptation :**
- Une connexion réussie retourne un token d'accès valide
- Un mauvais mot de passe affiche un message d'erreur clair sans préciser si c'est l'email ou le mot de passe qui est incorrect (sécurité)

### US-03 — Gérer mon profil *(RF-03)*
**En tant que** client, **je veux** consulter et modifier mes informations personnelles, **afin de** garder mon profil à jour.

**Critères d'acceptation :**
- Les champs modifiables sont clairement identifiés
- Les modifications sont sauvegardées immédiatement après validation

### US-04 — Connexion admin *(RF-05)*
**En tant qu'** admin, **je veux** me connecter avec un accès dédié, **afin de** gérer le restaurant en toute sécurité, séparément des comptes clients.

**Critères d'acceptation :**
- Un compte client ne peut pas accéder aux routes admin, même en cas d'erreur
- Toute tentative d'accès non autorisé est rejetée par le backend

---

## 3. Menu

### US-05 — Consulter les catégories *(RF-06)*
**En tant que** client, **je veux** voir les catégories de produits, **afin de** naviguer facilement dans le menu.

### US-06 — Consulter un produit *(RF-07)*
**En tant que** client, **je veux** voir le détail d'un produit (nom, description, prix, image), **afin de** décider si je souhaite le commander.

### US-07 — Gérer les produits *(RF-08)*
**En tant qu'** admin, **je veux** ajouter, modifier ou supprimer un produit, **afin de** garder le menu à jour.

**Critères d'acceptation :**
- Un produit supprimé n'apparaît plus côté client
- Les champs obligatoires (nom, prix, catégorie) sont validés avant l'enregistrement

### US-08 — Gérer les catégories *(RF-09)*
**En tant qu'** admin, **je veux** ajouter, modifier ou supprimer une catégorie, **afin d'**organiser le menu clairement.

---

## 4. Panier & Commande

### US-09 — Ajouter au panier *(RF-10)*
**En tant que** client, **je veux** ajouter un produit à mon panier, **afin de** préparer ma commande avant de la valider.

### US-10 — Modifier le panier *(RF-11)*
**En tant que** client, **je veux** modifier la quantité ou retirer un produit de mon panier, **afin de** ajuster ma commande avant validation.

### US-11 — Passer une commande *(RF-12, RF-13)*
**En tant que** client, **je veux** valider ma commande en choisissant retrait ou livraison, **afin de** recevoir mes produits de la manière qui me convient.

**Critères d'acceptation :**
- Le panier doit contenir au moins un produit pour valider une commande
- Le mode (retrait/livraison) est obligatoire et clairement demandé avant validation

### US-12 — Paiement simulé *(RF-14)*
**En tant que** client, **je veux** valider un paiement simulé, **afin de** finaliser ma commande sans intégration bancaire réelle en V1.

### US-13 — Historique des commandes *(RF-15)*
**En tant que** client, **je veux** consulter l'historique de mes commandes passées, **afin de** suivre mes achats précédents.

### US-14 — Notifications de statut *(RF-16)*
**En tant que** client, **je veux** être notifié quand le statut de ma commande change, **afin de** savoir quand elle sera prête ou livrée.

### US-15 — Gérer les commandes *(RF-17)*
**En tant qu'** admin, **je veux** consulter les commandes et modifier leur statut, **afin de** coordonner la préparation et la livraison.

---

## 5. Favoris

### US-16 — Ajouter un favori *(RF-18)*
**En tant que** client, **je veux** marquer un produit comme favori, **afin de** le retrouver rapidement plus tard.

### US-17 — Consulter mes favoris *(RF-19)*
**En tant que** client, **je veux** consulter la liste de mes produits favoris, **afin de** commander plus rapidement mes produits préférés.

---

## 6. Réservation

### US-18 — Réserver une table *(RF-20)*
**En tant que** client, **je veux** réserver une table pour une date et une heure précises, **afin de** garantir ma place au restaurant.

**Critères d'acceptation :**
- La date/heure choisie doit être dans le futur
- Une confirmation est affichée après réservation réussie

### US-19 — Gérer ma réservation *(RF-21)*
**En tant que** client, **je veux** consulter, modifier ou annuler ma réservation, **afin de** garder le contrôle sur mes plans.

### US-20 — Gérer les réservations (admin) *(RF-22)*
**En tant qu'** admin, **je veux** consulter et gérer les réservations des clients, **afin d'**organiser l'occupation des tables.

---

## 7. Synthèse de couverture

| Domaine | Nombre de User Stories | Exigences couvertes |
|---|---|---|
| Authentification & Profil | 4 | RF-01 à RF-05 |
| Menu | 4 | RF-06 à RF-09 |
| Panier & Commande | 7 | RF-10 à RF-17 |
| Favoris | 2 | RF-18, RF-19 |
| Réservation | 3 | RF-20 à RF-22 |

Toutes les exigences fonctionnelles du MVP sont couvertes par au moins une user story.

---

*Ces user stories serviront de base directe pour les diagrammes UML (`07-UML.md`), en particulier le diagramme de cas d'utilisation.*
