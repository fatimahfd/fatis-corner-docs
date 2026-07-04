# 07 — UML
**Fati's Corner Documentation**

| | |
|---|---|
| **Projet** | Fati's Corner |
| **Version** | 1.0 — MVP |
| **Auteur** | Fatima (Fatoush) |
| **Basé sur** | 06-User-Stories.md |
| **Format** | PlantUML (texte → diagramme visuel) |

---

## 1. Introduction

Ce document contient les diagrammes UML modélisant le système Fati's Corner (MVP), au format **PlantUML**. Chaque bloc de code peut être collé dans [plantuml.com/plantuml](https://www.plantuml.com/plantuml/uml/) ou une extension VS Code (ex: "PlantUML" by jebbs) pour obtenir le rendu visuel.

> Les diagrammes **Composants** et **Déploiement**, plus liés à l'infrastructure technique, seront détaillés dans `08-Architecture.md` afin d'éviter la duplication.

---

## 2. Diagramme de cas d'utilisation (Use Case)

Basé directement sur les user stories de `06-User-Stories.md`.

```plantuml
@startuml UseCase-FatisCorner

left to right direction

actor Client
actor Admin

rectangle "Fati's Corner" {
  usecase "Créer un compte" as UC1
  usecase "Se connecter" as UC2
  usecase "Gérer mon profil" as UC3
  usecase "Consulter le menu" as UC4
  usecase "Gérer le panier" as UC5
  usecase "Passer une commande" as UC6
  usecase "Consulter historique" as UC7
  usecase "Gérer les favoris" as UC8
  usecase "Réserver une table" as UC9
  usecase "Gérer les produits" as UC10
  usecase "Gérer les commandes" as UC11
  usecase "Gérer les réservations" as UC12
}

Client --> UC1
Client --> UC2
Client --> UC3
Client --> UC4
Client --> UC5
Client --> UC6
Client --> UC7
Client --> UC8
Client --> UC9

Admin --> UC2
Admin --> UC10
Admin --> UC11
Admin --> UC12

UC6 ..> UC5 : <<include>>
UC6 ..> UC2 : <<include>>

@enduml
```

---

## 3. Diagramme de classes (Class Diagram)

```plantuml
@startuml Class-FatisCorner

class User {
  +id: Long
  +nom: String
  +email: String
  +motDePasseHash: String
  +role: Role
}

enum Role {
  CLIENT
  ADMIN
  EMPLOYE
}

class Category {
  +id: Long
  +nom: String
}

class Product {
  +id: Long
  +nom: String
  +description: String
  +prix: BigDecimal
  +imageUrl: String
}

class CartItem {
  +id: Long
  +quantite: int
}

class Order {
  +id: Long
  +dateCommande: DateTime
  +statut: OrderStatus
  +modeRecuperation: DeliveryMode
  +total: BigDecimal
}

enum OrderStatus {
  EN_ATTENTE
  CONFIRMEE
  EN_PREPARATION
  PRETE
  LIVREE
  ANNULEE
}

enum DeliveryMode {
  RETRAIT
  LIVRAISON
}

class OrderItem {
  +id: Long
  +quantite: int
  +prixUnitaire: BigDecimal
}

class Favorite {
  +id: Long
}

class Reservation {
  +id: Long
  +dateReservation: DateTime
  +nombrePersonnes: int
  +statut: ReservationStatus
}

enum ReservationStatus {
  CONFIRMEE
  ANNULEE
  TERMINEE
}

User "1" -- "0..*" CartItem
CartItem "0..*" -- "1" Product

User "1" -- "0..*" Order
Order "1" -- "1..*" OrderItem
OrderItem "0..*" -- "1" Product

User "1" -- "0..*" Favorite
Favorite "0..*" -- "1" Product

User "1" -- "0..*" Reservation

Product "0..*" -- "1" Category

@enduml
```

---

## 4. Diagramme de séquence (Sequence Diagram) — Passer une commande

```plantuml
@startuml Sequence-PasserCommande

actor Client
participant "App Flutter" as App
participant "API Spring Boot" as API
participant "Base de données" as DB

Client -> App : Valider le panier
App -> API : POST /orders (JWT, items, modeRecuperation)
API -> API : Valider le token JWT
API -> DB : Vérifier disponibilité des produits
DB --> API : OK
API -> DB : Créer la commande (statut = EN_ATTENTE)
DB --> API : Commande créée
API -> API : Simuler le paiement
API -> DB : Mettre à jour statut (CONFIRMEE)
DB --> API : OK
API --> App : Confirmation commande + statut
App --> Client : Affichage confirmation + notification

@enduml
```

---

## 5. Diagramme d'activité (Activity Diagram) — Passer une commande

```plantuml
@startuml Activity-PasserCommande

start
:Consulter le menu;
:Ajouter des produits au panier;
:Valider le panier;
if (Panier vide ?) then (oui)
  :Afficher message d'erreur;
  stop
else (non)
  :Choisir mode (retrait / livraison);
  :Simuler le paiement;
  if (Paiement simulé réussi ?) then (oui)
    :Créer la commande (statut EN_ATTENTE);
    :Notifier le client (commande confirmée);
    stop
  else (non)
    :Afficher erreur de paiement;
    stop
  endif
endif

@enduml
```

---

## 6. Diagramme d'états (State Diagram) — Statut de la commande

```plantuml
@startuml State-Commande

[*] --> EN_ATTENTE
EN_ATTENTE --> CONFIRMEE : paiement validé
CONFIRMEE --> EN_PREPARATION : admin démarre préparation
EN_PREPARATION --> PRETE : préparation terminée
PRETE --> LIVREE : remise au client / livraison effectuée
EN_ATTENTE --> ANNULEE : annulation
CONFIRMEE --> ANNULEE : annulation
LIVREE --> [*]
ANNULEE --> [*]

@enduml
```

---

## 7. Diagramme de packages (Package Diagram) — Backend

```plantuml
@startuml Package-Backend

package "controller" 
package "service"
package "repository"
package "entity"
package "dto"
package "mapper"
package "security"
package "config"
package "exception"

controller --> service
service --> repository
repository --> entity
controller --> dto
service --> mapper
mapper --> entity
mapper --> dto
controller --> security

@enduml
```

---

## 8. Notes de conception

- Les diagrammes **Composants** et **Déploiement** seront traités dans `08-Architecture.md`, car ils décrivent l'infrastructure technique (serveurs, conteneurs, services externes) plutôt que la logique métier
- Le rôle **EMPLOYE** apparaît déjà dans le modèle de classes (enum `Role`) conformément à la décision prise dans `05-Requirements.md`, même si aucun cas d'utilisation ne lui est encore associé
- Ces diagrammes serviront de base directe à la conception de la base de données (`09-Database.md`) et de l'architecture (`08-Architecture.md`)

---

*Ces diagrammes seront ajustés si des évolutions de conception apparaissent pendant le développement — toute modification significative devra être tracée dans `13-Decisions-Log.md`.*
