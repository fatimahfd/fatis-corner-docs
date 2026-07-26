# Diagrammes UML — Fati's Corner

**Phase 5 — Diagrammes UML**
**Version :** 1.0
**Auteur :** Fatima
**Date :** Juillet 2026

Les diagrammes ci-dessous sont en syntaxe Mermaid — ils s'affichent directement dans GitHub (README, docs) et dans la plupart des éditeurs (VS Code avec l'extension Mermaid, GitLab, etc.).

---

## 1. Diagramme de classes

Modèle du domaine, basé sur le cahier des charges (Phase 1) et l'étude des besoins (Phase 2).

```mermaid
classDiagram
    class Utilisateur {
        +UUID id
        +String nom
        +String email
        +String motDePasseHash
        +Role role
        +Date dateCreation
    }
    class Client {
        +List~Commande~ commandes
        +List~Adresse~ adresses
    }
    class Employe {
        +Boolean actif
    }
    class Administrateur {
        +gererProduits()
        +gererEmployes()
        +consulterDashboard()
    }
    class Produit {
        +UUID id
        +String nom
        +String description
        +String image
        +Decimal prix
        +Boolean disponible
        +List~String~ ingredients
        +String taille
        +Integer tempsPreparation
    }
    class Categorie {
        +UUID id
        +String nom
    }
    class Panier {
        +UUID id
        +List~LignePanier~ lignes
        +calculerTotal() Decimal
    }
    class LignePanier {
        +Integer quantite
        +Decimal sousTotal()
    }
    class Commande {
        +UUID id
        +StatutCommande statut
        +ModePaiement modePaiement
        +Decimal total
        +Date dateCommande
        +changerStatut(StatutCommande)
    }
    class LigneCommande {
        +Integer quantite
        +Decimal prixUnitaire
    }
    class Notification {
        +UUID id
        +String message
        +Date dateEnvoi
        +Boolean lue
    }

    Utilisateur <|-- Client
    Utilisateur <|-- Employe
    Utilisateur <|-- Administrateur
    Client "1" --> "1" Panier : possède
    Panier "1" --> "*" LignePanier : contient
    LignePanier "*" --> "1" Produit : référence
    Client "1" --> "*" Commande : passe
    Commande "1" --> "*" LigneCommande : contient
    LigneCommande "*" --> "1" Produit : référence
    Produit "*" --> "1" Categorie : appartient à
    Commande "1" --> "*" Notification : génère
    Employe "1" --> "*" Commande : met à jour
```

**Note :** le champ `modePaiement` de `Commande` inclut déjà les valeurs `MOBILE_MONEY_BANKILY` et `MOBILE_MONEY_MASRVI` dans l'énumération, même si leur intégration réelle est repoussée en évolution future (cf. cahier des charges §5.7).

---

## 2. Diagramme de séquence — Passer une commande (UC-06)

```mermaid
sequenceDiagram
    actor Client
    participant App as Application Flutter
    participant API as API REST (Spring Boot)
    participant DB as PostgreSQL
    participant FCM as Firebase Cloud Messaging

    Client->>App: Valide le panier
    App->>API: POST /commandes
    API->>DB: Vérifie disponibilité des produits
    DB-->>API: Produits disponibles
    API->>DB: Crée la commande (statut = EN_ATTENTE)
    DB-->>API: Commande créée
    API->>FCM: Notifie l'administrateur/employé
    API-->>App: 201 Created + détails commande
    App-->>Client: Confirmation affichée
```

---

## 3. Diagramme d'état — Cycle de vie d'une commande (UC-11)

```mermaid
stateDiagram-v2
    [*] --> EnAttente
    EnAttente --> Confirmee : Employé/Admin confirme
    Confirmee --> EnPreparation : Préparation démarrée
    EnPreparation --> Prete : Préparation terminée
    Prete --> Livree : Remise au client
    EnAttente --> Annulee : Annulation
    Confirmee --> Annulee : Annulation
    Livree --> [*]
    Annulee --> [*]
```

Chaque transition déclenche une notification Firebase vers le client (cf. US-20, UC-09).

---

## 4. Diagramme de séquence — Authentification (UC-02)

```mermaid
sequenceDiagram
    actor Utilisateur
    participant App as Application Flutter
    participant API as API REST (Spring Boot)
    participant DB as PostgreSQL

    Utilisateur->>App: Saisit email + mot de passe
    App->>API: POST /auth/login
    API->>DB: Vérifie les identifiants
    DB-->>API: Utilisateur trouvé + rôle
    API->>API: Génère JWT + refresh token
    API-->>App: 200 OK + tokens
    App-->>Utilisateur: Redirection selon le rôle
```

---

*Ces diagrammes servent de référence directe pour la Phase 6 (Architecture) et la Phase 7 (Base de données) — le diagramme de classes se traduit presque directement en schéma de tables.*
