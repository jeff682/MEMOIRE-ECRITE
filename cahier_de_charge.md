Voici la version mise à jour du cahier des charges, sans la phrase sur le bouton de citation, et avec un **diagramme de cas d’utilisation** textuel (facile à comprendre et à retranscrire en image).

---

# Cahier des charges – Mémoire Écrite (Django & DRF)  
## Version complète, accessible à un enfant de 16 ans

## 1. Contexte du projet

Dans de nombreuses régions d’Afrique, les sources écrites sont rares ou difficiles d’accès. Les travaux de recherche, les critiques littéraires et les analyses de films sont souvent dispersés dans des universités, des bibliothèques peu connectées ou des revues payantes. Par conséquent, les étudiants, les enseignants et les journalistes manquent de ressources fiables et gratuites.

**Mémoire Écrite** est une plateforme web qui veut résoudre ce problème. Elle permettra de publier en libre accès des résultats de recherche, des critiques littéraires et des critiques cinématographiques. Tous les contenus seront évalués par un comité scientifique, recevront un identifiant unique (DOI) et pourront être réutilisés par d’autres sites grâce à une API. L’objectif est de devenir une source de référence pour les savoirs sur l’Afrique et les diasporas africaines.

---

## 2. Objectifs généraux

- Collecter et diffuser des contenus de qualité dans trois catégories : recherche, littérature, cinéma.
- Garantir la fiabilité grâce à une relecture par les pairs (peer review).
- Faciliter la citation en attribuant un DOI à chaque article publié.
- Permettre l’interopérabilité : d’autres plateformes (Google Scholar, bibliothèques numériques) pourront récupérer nos métadonnées via une API.
- Être accessible sur ordinateur, tablette et téléphone (design responsive).
- Respecter les normes du libre accès (licences Creative Commons).

---

## 3. Public cible

| Type d’utilisateur | Besoins principaux |
|---|---|
| Chercheur ou enseignant | Publier ses travaux, consulter les publications récentes, recevoir une reconnaissance (DOI). |
| Étudiant | Lire gratuitement des articles pour ses cours, trouver des critiques de livres ou de films. |
| Journaliste culturel | Découvrir des analyses de films ou de romans pour préparer ses chroniques. |
| Auteur (critique) | Soumettre des textes et suivre leur évaluation. |
| Comité de lecture | Évaluer les soumissions, accepter ou refuser, attribuer les DOI. |
| Visiteur lambda | Parcourir les contenus sans avoir besoin de créer un compte. |

---

## 4. Termes et définitions (pour tout comprendre)

| Terme | Définition simple |
|---|---|
| **DOI** (Digital Object Identifier) | Un numéro unique qui identifie un article de manière permanente. Exemple : `10.5678/memoire.2026.03`. |
| **API** (Application Programming Interface) | Un système qui permet à d’autres sites ou logiciels de venir chercher automatiquement des informations sur notre plateforme. |0
| **DRF** (Django REST Framework) | Une boîte à outils pour créer une API facilement avec Django. |
| **Pair review** (relecture par les pairs) | Un processus où d’autres experts vérifient la qualité d’un article avant qu’il ne soit publié. |
| **Open access** (libre accès) | Un article que tout le monde peut lire gratuitement, sans abonnement. |
| **Frontend** | La partie visible du site (ce que vous voyez dans votre navigateur). |
| **Backend** | La partie cachée (le serveur, la base de données, la logique). |
| **Modèle (model)** | Dans Django, c’est la représentation d’une donnée (par exemple un article, un auteur). |
| **Vue (view)** | Une fonction qui reçoit une requête et renvoie une réponse (page HTML ou données JSON). |
| **Sérialiseur (serializer)** | Dans DRF, un traducteur qui convertit les données de la base de données vers du JSON. |
| **JWT** (JSON Web Token) | Un moyen sécurisé de garder une session utilisateur. Utilisé pour l’authentification via l’API. |

---

## 5. Diagramme de cas d’utilisation

Le diagramme ci‑dessous (texte structuré) montre **qui** peut faire **quoi** sur la plateforme. Pour le dessiner, une flèche relie l’acteur à l’action.

### 5.1. Acteurs (utilisateurs)

- **Visiteur** (non connecté)
- **Auteur** (connecté)
- **Réviseur** (membre du comité de lecture)
- **Éditeur** (administrateur)

### 5.2. Cas d’utilisation par acteur

| Acteur | Actions possibles |
|--------|------------------|
| **Visiteur** | – Consulter la page d’accueil<br>– Parcourir la liste des articles publiés<br>– Lire un article en détail<br>– Utiliser la barre de recherche simple<br>– Appliquer des filtres (type, discipline, année)<br>– Accéder à la page À propos / Contact<br>– Créer un compte (s’inscrire) |
| **Auteur** | – S’identifier (se connecter)<br>– Visualiser son tableau de bord<br>– Soumettre un nouvel article (titre, résumé, fichier PDF, mots‑clés)<br>– Modifier un article en brouillon<br>– Consulter l’état de ses soumissions (en attente, en relecture, accepté, refusé)<br>– Se déconnecter |
| **Réviseur** | – Se connecter<br>– Voir la liste des articles soumis en attente de relecture<br>– Lire le texte intégral d’un article<br>– Donner un avis : accepter, refuser, ou demander des modifications<br>– Laisser un commentaire à l’auteur<br>– Se déconnecter |
| **Éditeur** | – Tout ce que fait le réviseur<br>– Attribuer un DOI à un article accepté<br>– Publier un article (le rendre visible sur le site public)<br>– Gérer les utilisateurs (créer, supprimer, changer les rôles)<br>– Gérer les catégories (types de document, disciplines)<br>– Consulter les statistiques du site (nombre de soumissions, de consultations) |

**Note** : Le visiteur qui souhaite soumettre un article doit d’abord créer un compte (devenir auteur). Le comité de lecture (réviseurs) et les éditeurs sont choisis par l’administration.

---

## 6. Règles fonctionnelles détaillées

### 6.1. Cycle de vie d’un article

1. Un auteur connecté remplit un formulaire de soumission (titre, résumé, fichier PDF, mots‑clés, type, discipline).
2. L’article passe en statut **« en attente de relecture »**.
3. Un réviseur assigné prend l’article, le lit et peut décider : **accepter**, **refuser** ou **demander des modifications**.
4. Si accepté, l’éditeur attribue un DOI, puis l’article devient **publié** et visible sur le site.
5. L’auteur reçoit un email à chaque changement d’état.

### 6.2. Fonctionnalités de recherche et filtres

- Une barre de recherche globale (titre, auteur, résumé, mots‑clés).
- Des filtres par type (recherche, littérature, cinéma).
- Des filtres par discipline (histoire, anthropologie, géographie, sociologie, études littéraires, cinéma, arts).
- Des filtres par année de publication.
- Possibilité de combiner plusieurs filtres.

### 6.3. API publique

- Une API REST (avec Django REST Framework) exposera les métadonnées des articles publiés : titre, auteur, résumé, DOI, date, type, discipline.
- Endpoints principaux : `/api/articles/`, `/api/articles/{id}/`, `/api/search/`.
- Authentification via JWT pour les actions protégées (soumission, évaluation).
- Documentation interactive (Swagger) pour faciliter l’intégration par d’autres développeurs.

---

## 7. Architecture technique : Django + DRF

Nous choisissons **Django** (framework web en Python) et **Django REST Framework** (extension API) car ils sont puissants, sécurisés, bien documentés et adaptés à un projet évolutif.

### 7.1. Applications Django nécessaires (apps)

| Nom de l’app | Rôle et justification |
|--------------|------------------------|
| `accounts`   | Gère tout ce qui concerne les utilisateurs : inscription, connexion, profils, rôles (auteur, réviseur, éditeur). |
| `articles`   | Contient les modèles (Article, Fichier), les formulaires de soumission, les vues pour lister et afficher un article. |
| `review`     | Gère l’évaluation : modèle Review (commentaire, décision), assignation des réviseurs, historique des statuts. |
| `search`     | Gère la recherche avancée et les filtres (moteur de recherche plein texte). |
| `api`        | Construite avec DRF, expose les endpoints publics et privés (sérialiseurs, vues API). |
| `core`       | Contient les modèles transversaux (catégories, disciplines), les fonctions utilitaires. |

### 7.2. Modèles principaux (esquisse)

- **User** (Django par défaut) + extension `Profile` (dans `accounts`) : ajoute `role`, `institution`, `bio`.
- **Category** (`core`) : Type de contenu (recherche, littérature, cinéma) et Discipline (histoire, géographie…).
- **Article** (`articles`) : titre, résumé, fichier PDF, DOI, date de soumission, date de publication, statut, auteur principal, catégories, mots‑clés.
- **Review** (`review`) : lien vers un Article, réviseur, commentaire, décision, date.

---

## 8. Minimum Viable Product (MVP) – versions progressives

### MVP 1 (Fondation – 2 à 3 semaines)

- Pages statiques en HTML/CSS (accueil, contenu, recherche, connexion, soumission) – sans base de données.
- Navigation responsive.
- Formulaire de soumission qui envoie un email (simulation).
- **Objectif** : valider le design et les parcours.

### MVP 2 (Premier backend – 3 à 4 semaines)

- Installation de Django, création des apps `accounts`, `articles`, `core`.
- Authentification (inscription, connexion, déconnexion).
- Dépôt d’un article avec enregistrement en base et stockage du PDF.
- Page publique de la liste des articles publiés.
- Page détail d’un article.
- **Objectif** : la soumission et la consultation fonctionnent.

### MVP 3 (Évaluation et API – 4 à 5 semaines)

- Implémentation de l’app `review` : un réviseur peut évaluer un article, changer le statut.
- Attribution automatique d’un DOI (simulation).
- API REST publique (lecture seule) : `/api/articles/`.
- Documentation simple de l’API.
- **Objectif** : le cycle complet de publication est opérationnel, l’API est prête pour d’autres plateformes.

### Versions ultérieures (post-MVP)

- Recherche avancée avec filtres multiples.
- Notifications par email.
- Tableau de bord pour l’auteur.
- Export des références (BibTeX).
- Intégration avec OpenAIRE ou Crossref.

---

## 9. Profils des personnes nécessaires pour le projet

| Rôle | Compétences | Mission |
|------|-------------|---------|
| **Chef de projet** | Organisation, communication | Planifier les tâches, animer les réunions, suivre le planning. |
| **Développeur·se backend** | Python, Django, DRF, PostgreSQL, Git | Construire les modèles, l’API, l’authentification, gérer la sécurité. |
| **Développeur·se frontend** | HTML, CSS, JavaScript, bases de Django templates | Intégrer les maquettes, rendre le site responsive, coder les interactions (menus, filtres, modales). |
| **Designer UI/UX** | Figma, sens des couleurs mates, accessibilité | Dessiner toutes les pages (maquettes haute fidélité), définir la charte graphique. |
| **Rédacteur·trice technique** | Expression écrite, vulgarisation | Écrire les textes du site, la charte éditoriale, l’aide aux auteurs. |
| **Responsable contenu** | Réseaux sociaux, relation avec les chercheurs | Inviter des auteurs, promouvoir la plateforme, alimenter les premières publications. |
| **Testeur·se QA** | Méthode, curiosité | Tester sur plusieurs navigateurs et mobiles, signaler les bugs. |
| **Admin sys (optionnel)** | Linux, Docker, hébergement | Mettre en ligne, configurer le serveur, les sauvegardes, le SSL. |

En équipe réduite (2‑3 personnes), on peut fusionner : développeur full‑stack + chef de projet + designer (ou utiliser un designer freelance).

---

## 10. Planning prévisionnel (semaines)

| Semaines | Tâches | Livrables |
|----------|--------|------------|
| 1-2 | Conception des maquettes, validation design | Maquettes Figma |
| 3 | MVP 1 : pages HTML/CSS statiques | Site statique navigable |
| 4-5 | MVP 2 : Django, auth, soumission simple | Site avec authentification et dépôt d’article |
| 6-7 | MVP 3 : évaluation, API DRF, DOI | Cycle complet, API documentée |
| 8 | Tests et corrections | Rapport de test, site sur serveur de test |
| 9 | Mise en ligne, communication | Site public sur mémoire-ecrite.org |

---

## 11. Budget prévisionnel (première année)

| Poste | Coût (€) |
|-------|----------|
| Nom de domaine (.org) | 12 € |
| Hébergement VPS (2 CPU, 4 Go RAM) | 25 €/mois = 300 € |
| Certificat SSL (Let’s Encrypt) | 0 € |
| Service email (optionnel) | 15 €/mois |
| Stockage | inclus dans le VPS |
| Développement (si pro) | 3 000 – 6 000 € (sinon bénévole) |

**Budget minimum (domaine + hébergement)** : environ 312 € par an.

---

## 12. Indicateurs de succès (après 6 mois)

- Le site contient au moins **50 articles publiés**.
- **1000 visiteurs uniques** par mois.
- **5 revues ou blogs** citent un article de Mémoire Écrite.
- L’API est utilisée par **2 applications externes**.
- Temps de chargement < 2 secondes sur 3G.
- 80 % des auteurs déclarent qu’ils soumettraient à nouveau.

---

## 13. Validation et prochaines étapes

Après approbation, l’équipe :

1. Créera le dépôt Git.
2. Installera Django et DRF.
3. Démarrera les maquettes haute fidélité.
4. Programmera des sprints (méthode agile).

**Mémoire Écrite deviendra ainsi une ressource précieuse pour l’éducation et la recherche en Afrique et au‑delà.**

--- 

*Document rédigé en langage accessible – dernière mise à jour : mai 2026. Le diagramme de cas d’utilisation se trouve au point 5.*