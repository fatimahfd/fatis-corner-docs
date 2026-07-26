# Cas d'Utilisation — Fati's Corner

**Phase 4 — Cas d'utilisation (Use Cases)**
**Version :** 1.0
**Auteur :** Fatima
**Date :** Juillet 2026

---

## 1. Acteurs du système

| Acteur | Rôle |
|---|---|
| **Client** | Utilisateur final qui consulte le menu et passe des commandes |
| **Employé** | Membre du personnel qui prépare et met à jour les commandes |
| **Administrateur** | Gère le catalogue, les commandes, les employés et les statistiques |

## 2. Vue d'ensemble des cas d'utilisation par acteur

**Client :** S'inscrire, Se connecter, Consulter le menu, Rechercher/filtrer un produit, Ajouter au panier, Passer commande, Suivre une commande, Consulter l'historique, Recevoir une notification.

**Employé :** Se connecter, Consulter les commandes du jour, Changer le statut d'une commande.

**Administrateur :** Se connecter, Gérer les produits, Gérer les catégories, Gérer les commandes, Gérer les employés, Consulter le dashboard.

---

## 3. Cas d'utilisation détaillés

### UC-01 — S'inscrire
- **Acteur :** Client
- **Précondition :** L'utilisateur n'a pas de compte
- **Scénario nominal :**
  1. Le client accède à l'écran d'inscription
  2. Il saisit nom, email, mot de passe
  3. Le système valide les champs et vérifie l'unicité de l'email
  4. Le système crée le compte et hash le mot de passe
  5. Le client est redirigé vers la connexion
- **Scénario alternatif :** Email déjà utilisé → message d'erreur, retour au formulaire
- **Postcondition :** Le compte client est créé en base

### UC-02 — Se connecter
- **Acteur :** Client, Employé, Administrateur
- **Précondition :** L'utilisateur possède un compte
- **Scénario nominal :**
  1. L'utilisateur saisit email et mot de passe
  2. Le système vérifie les identifiants
  3. Le système génère un JWT et un refresh token
  4. L'utilisateur est redirigé vers son espace selon son rôle
- **Scénario alternatif :** Identifiants invalides → message d'erreur
- **Postcondition :** Session active avec token valide

### UC-03 — Consulter le menu
- **Acteur :** Client
- **Précondition :** Aucune (accessible sans compte pour la consultation)
- **Scénario nominal :**
  1. Le client ouvre l'application
  2. Le système affiche les catégories et produits disponibles
- **Postcondition :** Le menu est affiché

### UC-04 — Rechercher / filtrer un produit
- **Acteur :** Client
- **Précondition :** Le client consulte le menu
- **Scénario nominal :**
  1. Le client saisit un mot-clé ou sélectionne une catégorie
  2. Le système filtre les produits correspondants
- **Postcondition :** Liste filtrée affichée

### UC-05 — Ajouter au panier
- **Acteur :** Client
- **Précondition :** Le client est connecté
- **Scénario nominal :**
  1. Le client sélectionne un produit et une quantité
  2. Le système ajoute l'article au panier et recalcule le total
- **Scénario alternatif :** Produit devenu indisponible → message d'erreur, retrait automatique du panier
- **Postcondition :** Panier mis à jour

### UC-06 — Passer commande
- **Acteur :** Client
- **Précondition :** Le panier contient au moins un article
- **Scénario nominal :**
  1. Le client valide son panier
  2. Il choisit un mode de paiement (espèces ou à la livraison)
  3. Le système crée la commande avec le statut « En attente »
  4. Le système notifie l'administrateur/employé de la nouvelle commande
- **Postcondition :** Commande enregistrée, panier vidé

### UC-07 — Suivre une commande
- **Acteur :** Client
- **Précondition :** Le client a passé au moins une commande
- **Scénario nominal :**
  1. Le client consulte l'état de sa commande en cours
  2. Le système affiche le statut actuel en temps réel
- **Postcondition :** Statut à jour affiché

### UC-08 — Consulter l'historique
- **Acteur :** Client
- **Précondition :** Le client est connecté
- **Scénario nominal :**
  1. Le client accède à la section historique
  2. Le système liste ses commandes passées avec leur statut final
- **Postcondition :** Historique affiché

### UC-09 — Recevoir une notification
- **Acteur :** Client
- **Précondition :** Le statut d'une commande change
- **Scénario nominal :**
  1. Le système détecte le changement de statut
  2. Firebase Cloud Messaging envoie une notification push au client
- **Postcondition :** Client informé sans ouvrir l'application

### UC-10 — Consulter les commandes du jour
- **Acteur :** Employé
- **Précondition :** L'employé est connecté
- **Scénario nominal :**
  1. L'employé accède à la liste des commandes du jour
  2. Le système affiche les commandes triées par heure
- **Postcondition :** Liste affichée

### UC-11 — Changer le statut d'une commande
- **Acteur :** Employé
- **Précondition :** Une commande est en cours de traitement
- **Scénario nominal :**
  1. L'employé sélectionne une commande
  2. Il fait passer son statut à l'étape suivante (Confirmée → En préparation → Prête → Livrée)
  3. Le système notifie le client du changement
- **Scénario alternatif :** Annulation de la commande → statut « Annulée », notification au client
- **Postcondition :** Statut mis à jour, client notifié

### UC-12 — Gérer les produits
- **Acteur :** Administrateur
- **Précondition :** L'administrateur est connecté
- **Scénario nominal :**
  1. L'administrateur ajoute, modifie ou supprime un produit
  2. Le système valide les champs et enregistre les modifications
- **Postcondition :** Catalogue mis à jour, visible immédiatement côté client

### UC-13 — Gérer les catégories
- **Acteur :** Administrateur
- **Précondition :** L'administrateur est connecté
- **Scénario nominal :**
  1. L'administrateur crée, modifie ou supprime une catégorie
- **Postcondition :** Catégories mises à jour

### UC-14 — Gérer les commandes
- **Acteur :** Administrateur
- **Précondition :** Des commandes existent dans le système
- **Scénario nominal :**
  1. L'administrateur consulte toutes les commandes
  2. Il peut filtrer par statut, date ou client
- **Postcondition :** Vue d'ensemble des commandes obtenue

### UC-15 — Gérer les employés
- **Acteur :** Administrateur
- **Précondition :** L'administrateur est connecté
- **Scénario nominal :**
  1. L'administrateur crée un compte employé ou désactive un compte existant
- **Postcondition :** Liste des employés à jour

### UC-16 — Consulter le dashboard
- **Acteur :** Administrateur
- **Précondition :** L'administrateur est connecté
- **Scénario nominal :**
  1. L'administrateur accède au dashboard
  2. Le système affiche le chiffre d'affaires, le nombre de commandes et les produits populaires
- **Postcondition :** Vue synthétique de l'activité affichée

---

*Ces cas d'utilisation servent de base directe à la Phase 5 (Diagrammes UML), notamment au diagramme de cas d'utilisation et au diagramme de séquence pour les scénarios clés (UC-06 Passer commande, UC-11 Changer le statut).*
