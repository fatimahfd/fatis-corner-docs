# Étude des Besoins — Fati's Corner

**Phase 2 — Étude des besoins**
**Version :** 1.0
**Auteur :** Fatima
**Date :** Juillet 2026

---

## 1. Objectif de l'étude

Identifier précisément les besoins de chaque acteur du système, les prioriser pour le MVP (2-3 semaines), et poser les bases fonctionnelles avant de passer aux User Stories et Cas d'utilisation.

## 2. Analyse de l'existant

Les petits cafés-restaurants gèrent aujourd'hui leurs commandes via WhatsApp ou Facebook :

| Limite constatée | Conséquence |
|---|---|
| Pas de menu structuré | Le client doit demander les prix/disponibilités manuellement |
| Pas de suivi de commande | Incertitude sur l'état de la commande |
| Pas de centralisation | Chaque commande est traitée individuellement, sans vue d'ensemble |
| Pas de statistiques | Aucune donnée pour piloter le business (produits populaires, CA, etc.) |
| Gestion du personnel informelle | Risque d'erreurs et de confusion en cuisine |

## 3. Besoins par acteur

### 3.1 Client
- Besoin de **rapidité** : commander en quelques clics sans échange manuel
- Besoin de **clarté** : voir le menu, les prix, la disponibilité en temps réel
- Besoin de **confiance** : suivre l'état de sa commande du début à la fin
- Besoin de **simplicité** : créer un compte et commander sans friction
- Besoin de **personnalisation** *(future)* : historique, avis, préférences

### 3.2 Administrateur
- Besoin de **contrôle** : gérer produits, catégories et disponibilité facilement
- Besoin de **visibilité** : voir les commandes en cours et le chiffre d'affaires
- Besoin de **gestion du personnel** : attribuer des rôles, suivre l'activité des employés
- Besoin de **pilotage** *(future)* : statistiques avancées pour ajuster l'offre

### 3.3 Employé
- Besoin de **clarté opérationnelle** : voir uniquement les commandes du jour, sans surcharge d'information
- Besoin de **rapidité de mise à jour** : changer le statut d'une commande en un geste
- Besoin de **fiabilité** : ne pas rater de commande entrante

## 4. Besoins fonctionnels priorisés (méthode MoSCoW)

| Priorité | Besoin |
|---|---|
| **Must have (MVP)** | Auth (JWT + rôles), gestion produits/catégories, menu (recherche/filtre), panier, commandes (6 états), paiement espèces/à la livraison, notifications de statut, dashboard admin basique |
| **Should have (si le temps le permet)** | Historique des commandes client, gestion des produits populaires |
| **Could have (évolution future)** | Avis clients, codes promo, statistiques avancées |
| **Won't have (hors scope actuel)** | Paiement mobile money intégré, réservation de table, QR code, multi-branches, IA de recommandation |

## 5. Besoins non fonctionnels

- Temps de réponse API < 2 secondes
- Sécurité : JWT, BCrypt, validation des entrées, protection XSS et injection SQL
- Disponibilité 24h/24
- Interface mobile simple et intuitive, adaptée à des utilisateurs non technophiles

## 6. Contraintes du projet

- **Temps :** ~15 jours ouvrés, 3-4h/jour (2-3 semaines)
- **Ressources humaines :** développement solo
- **Technique :** stack imposée (Flutter, Spring Boot, PostgreSQL/Neon, Cloudinary, Firebase)
- **Conception UI/UX :** maquettes réalisées avec **Figma AI**, pour accélérer la production des écrans tout en gardant une identité visuelle cohérente avec la marque (chaleureuse, gourmande)

## 7. Risques identifiés

| Risque | Impact | Mitigation |
|---|---|---|
| Sous-estimation du temps de développement mobile (Flutter) | Retard sur le planning | Prioriser le MVP strict, repousser le « Should have » si besoin |
| Complexité de l'intégration Firebase (notifications) | Retard sur la Phase 9-10 | Prévoir un fallback simple (statut visible dans l'appli sans push) si bloquant |
| Dépendance à une seule personne (pas de relecture croisée) | Bugs non détectés | Prévoir une phase de tests dédiée (Phase 11) |

---

*Ce document alimente directement la Phase 3 (User Stories) et la Phase 4 (Cas d'utilisation).*
