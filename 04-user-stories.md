# User Stories — Fati's Corner

**Phase 3 — User Stories**
**Version :** 1.0
**Auteur :** Fatima
**Date :** Juillet 2026

**Format :** En tant que `<rôle>`, je veux `<action>` afin de `<bénéfice>`.
**Priorité :** alignée sur la matrice MoSCoW de la Phase 2 (Must / Should / Could).

---

## 1. Authentification

**US-01** *(Must)*
En tant que **client**, je veux créer un compte avec mon nom, email et mot de passe, afin de pouvoir commander et suivre mes commandes.
*Critères d'acceptation :* email unique, mot de passe hashé (BCrypt), validation des champs, message d'erreur clair si l'email existe déjà.

**US-02** *(Must)*
En tant qu'**utilisateur** (client, admin ou employé), je veux me connecter avec mon email et mot de passe, afin d'accéder à mon espace personnel.
*Critères d'acceptation :* génération d'un JWT + refresh token, message d'erreur explicite en cas d'identifiants invalides, redirection selon le rôle.

**US-03** *(Must)*
En tant qu'**utilisateur**, je veux me déconnecter, afin de sécuriser mon compte sur un appareil partagé.
*Critères d'acceptation :* invalidation du token côté client, retour à l'écran de connexion.

**US-04** *(Should)*
En tant qu'**utilisateur**, je veux réinitialiser mon mot de passe oublié, afin de retrouver l'accès à mon compte sans contacter le support.
*Critères d'acceptation :* envoi d'un lien/code de réinitialisation par email, expiration du lien après un délai raisonnable.

---

## 2. Menu & Produits

**US-05** *(Must)*
En tant que **client**, je veux consulter le menu organisé par catégories, afin de trouver rapidement ce que je cherche.
*Critères d'acceptation :* affichage des catégories (desserts, cookies, crêpes, gaufres, cafés, boissons, milkshakes, smoothies), chargement en moins de 2 secondes.

**US-06** *(Must)*
En tant que **client**, je veux rechercher et filtrer les produits, afin de gagner du temps face à un menu chargé.
*Critères d'acceptation :* recherche par nom, filtre par catégorie/disponibilité, résultats instantanés.

**US-07** *(Must)*
En tant que **client**, je veux voir le détail d'un produit (description, prix, ingrédients, temps de préparation), afin de faire un choix éclairé avant de commander.
*Critères d'acceptation :* fiche produit complète avec image, indication claire si le produit est indisponible.

**US-08** *(Must)*
En tant qu'**administrateur**, je veux ajouter, modifier et supprimer des produits et catégories, afin de garder le menu à jour.
*Critères d'acceptation :* formulaire complet (nom, description, image, prix, catégorie, disponibilité, ingrédients, taille, temps de préparation), validation avant enregistrement.

**US-09** *(Should)*
En tant que **client**, je veux voir les produits populaires, afin de découvrir les meilleures options rapidement.
*Critères d'acceptation :* section « populaires » basée sur le nombre de commandes.

---

## 3. Panier & Commande

**US-10** *(Must)*
En tant que **client**, je veux ajouter un produit à mon panier et ajuster la quantité, afin de préparer ma commande avant de la valider.
*Critères d'acceptation :* calcul automatique du sous-total, possibilité de retirer un article, panier persistant tant que la session est active.

**US-11** *(Must)*
En tant que **client**, je veux valider ma commande et choisir mon mode de paiement (espèces ou à la livraison), afin de finaliser mon achat.
*Critères d'acceptation :* récapitulatif avant validation, création de la commande en base avec statut « En attente ».

**US-12** *(Must)*
En tant que **client**, je veux suivre l'état de ma commande en temps réel (En attente → Confirmée → En préparation → Prête → Livrée), afin de savoir quand elle sera prête.
*Critères d'acceptation :* mise à jour visible sans rafraîchir manuellement, notification push à chaque changement de statut.

**US-13** *(Should)*
En tant que **client**, je veux consulter l'historique de mes commandes passées, afin de recommander facilement mes produits favoris.
*Critères d'acceptation :* liste chronologique avec statut et détails de chaque commande.

**US-14** *(Must)*
En tant qu'**employé**, je veux voir la liste des commandes du jour, afin de savoir ce qu'il faut préparer en priorité.
*Critères d'acceptation :* tri par heure de commande, mise en évidence des commandes urgentes.

**US-15** *(Must)*
En tant qu'**employé**, je veux changer le statut d'une commande en un geste, afin de tenir le client informé sans perte de temps.
*Critères d'acceptation :* boutons d'action rapide, mise à jour immédiate visible côté client.

**US-16** *(Must)*
En tant qu'**administrateur**, je veux consulter toutes les commandes et leur statut, afin de garder une vue d'ensemble de l'activité.
*Critères d'acceptation :* filtre par statut, par date, par client.

---

## 4. Dashboard Administrateur

**US-17** *(Must)*
En tant qu'**administrateur**, je veux voir un résumé du chiffre d'affaires et du nombre de commandes, afin de suivre la performance du restaurant en un coup d'œil.
*Critères d'acceptation :* chiffres à jour, affichage clair (aujourd'hui / total).

**US-18** *(Should)*
En tant qu'**administrateur**, je veux voir les produits les plus vendus, afin d'ajuster l'offre en fonction de la demande.
*Critères d'acceptation :* classement basé sur le nombre de commandes par produit.

**US-19** *(Must)*
En tant qu'**administrateur**, je veux gérer les comptes employés (créer, désactiver), afin de contrôler qui a accès à la gestion des commandes.
*Critères d'acceptation :* attribution du rôle Employé, désactivation sans suppression des données historiques.

---

## 5. Notifications

**US-20** *(Must)*
En tant que **client**, je veux recevoir une notification quand ma commande change de statut, afin de ne pas avoir à vérifier l'appli en permanence.
*Critères d'acceptation :* notification Firebase déclenchée à chaque changement de statut, message clair et court.

---

## 6. Évolutions futures (hors MVP — non détaillées ici)

- Avis clients (noter, commenter)
- Codes promo et offres spéciales
- Paiement mobile money intégré (Bankily, Masrvi)
- Statistiques avancées par période

---

*Ces User Stories alimentent directement la Phase 4 (Cas d'utilisation / diagrammes Use Case).*
